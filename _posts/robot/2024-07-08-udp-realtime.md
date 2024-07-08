---
title: "UDP sample code using callback using realtime tasek(xenomai)"
excerpt_separator: "<!--more-->"
categories:
  - robot
tags:
  - robot

toc : true
toc_sticky : true
---

# recevie하는 부분을 callback함수로 처리해서 만듬. 
```
udp_comm_project/
├── include/
│   ├── CustomUdpComm.hpp
│   └── UdpComm.hpp
├── src/
│   ├── CustomUdpComm.cpp
│   ├── UdpComm.cpp
│   └── main.cpp
├── Makefile
└── README.md

```

## udp_comm.hpp
``` cpp
#ifndef UDPCOMM_HPP
#define UDPCOMM_HPP

#include <alchemy/task.h>
#include <alchemy/timer.h>
#include <iostream>
#include <string>
#include <netinet/in.h>
#include <sys/socket.h>
#include <unistd.h>

class UdpComm {
public:
    UdpComm(const std::string& local_ip, int local_port);
    void send_data(const std::string& ip, int port, const std::string& command, const std::string& data);
    void set_receive_callback(void (*callback)(const std::string&, const std::string&, const sockaddr_in&));
    virtual ~UdpComm();

protected:
    int sockfd;
    sockaddr_in local_addr;
    RT_TASK recv_task;
    void (*receive_callback)(const std::string&, const std::string&, const sockaddr_in&) = nullptr;

    static void receive_udp(void *arg);
    void on_receive(const std::string& command, const std::string& data, const sockaddr_in& addr);
};

#endif // UDPCOMM_HPP



```
```
rt_task_create(&recv_task, "udp_recv_task", 0, 99, 0);
99는 우선 순위인데 높을수록 우선순위가 높다. 0이 제일 낮은것으로 추후 셋팅할때 참고 해길. 
```

## udp_comm.cpp
``` cpp
#include "UdpComm.hpp"

UdpComm::UdpComm(const std::string& local_ip, int local_port) {
    sockfd = socket(AF_INET, SOCK_DGRAM, 0); // UDP 소켓 생성
    if (sockfd < 0) {
        perror("socket creation failed");
        exit(EXIT_FAILURE);
    }

    int reuse = 1;
    if (setsockopt(sockfd, SOL_SOCKET, SO_REUSEADDR, (const char*)&reuse, sizeof(reuse)) < 0) {
        perror("setsockopt(SO_REUSEADDR) failed");
        close(sockfd);
        exit(EXIT_FAILURE);
    }

    local_addr.sin_family = AF_INET;
    local_addr.sin_addr.s_addr = inet_addr(local_ip.c_str());
    local_addr.sin_port = htons(local_port);

    if (bind(sockfd, (const struct sockaddr *)&local_addr, sizeof(local_addr)) < 0) {
        perror("bind failed");
        close(sockfd);
        exit(EXIT_FAILURE);
    }

    // 실시간 태스크 생성
    int ret = rt_task_create(&recv_task, "udp_recv_task", 0, 99, 0);
    if (ret) {
        std::cerr << "Failed to create RT task: " << strerror(-ret) << std::endl;
        close(sockfd);
        exit(EXIT_FAILURE);
    }

    ret = rt_task_start(&recv_task, &UdpComm::receive_udp, this);
    if (ret) {
        std::cerr << "Failed to start RT task: " << strerror(-ret) << std::endl;
        rt_task_delete(&recv_task);
        close(sockfd);
        exit(EXIT_FAILURE);
    }
}

UdpComm::~UdpComm() {
    rt_task_delete(&recv_task);
    close(sockfd);
}

void UdpComm::receive_udp(void *arg) {
    UdpComm *comm = static_cast<UdpComm*>(arg);
    char buffer[1024];
    sockaddr_in client_addr;
    socklen_t len = sizeof(client_addr);
    while (true) {
        int n = recvfrom(comm->sockfd, buffer, 1024, 0, (struct sockaddr*)&client_addr, &len);
        if (n > 0) {
            buffer[n] = '\0';
            std::string message(buffer);
            std::string command = message.substr(0, 4);
            std::string data = message.substr(4);
            if (comm->receive_callback) {
                comm->receive_callback(command, data, client_addr);
            }
        }
    }
}

void UdpComm::send_data(const std::string& ip, int port, const std::string& command, const std::string& data) {
    sockaddr_in dest_addr;
    dest_addr.sin_family = AF_INET;
    dest_addr.sin_port = htons(port);
    inet_pton(AF_INET, ip.c_str(), &dest_addr.sin_addr);

    std::string message = command.substr(0, 4) + data;
    sendto(sockfd, message.c_str(), message.size(), 0, (const struct sockaddr*)&dest_addr, sizeof(dest_addr));
}

void UdpComm::set_receive_callback(void (*callback)(const std::string&, const std::string&, const sockaddr_in&)) {
    receive_callback = callback;
}

void UdpComm::on_receive(const std::string& command, const std::string& data, const sockaddr_in& addr) {
    std::string client_ip = inet_ntoa(addr.sin_addr);
    int client_port = ntohs(addr.sin_port);
    std::cout << "Received command: " << command << " data: " << data << " from " << client_ip << ":" << client_port << std::endl;
    send_data(client_ip, client_port, "RESP", "Response to " + command);
}



```

## customudpcomm.hpp
``` cpp
#ifndef CUSTOMUDPCOMM_HPP
#define CUSTOMUDPCOMM_HPP

#include "UdpComm.hpp"

class CustomUdpComm : public UdpComm {
public:
    CustomUdpComm(const std::string& local_ip, int local_port);
    virtual void on_receive(const std::string& command, const std::string& data, const sockaddr_in& addr) override;
};

#endif // CUSTOMUDPCOMM_HPP

```

## customudpcomm.cpp
``` cpp
#include "CustomUdpComm.hpp"

CustomUdpComm::CustomUdpComm(const std::string& local_ip, int local_port) 
    : UdpComm(local_ip, local_port) {}

void CustomUdpComm::on_receive(const std::string& command, const std::string& data, const sockaddr_in& addr) {
    // 커스텀 처리 로직 추가
    std::string client_ip = inet_ntoa(addr.sin_addr);
    int client_port = ntohs(addr.sin_port);
    std::cout << "Custom received command: " << command << " data: " << data << " from " << client_ip << ":" << client_port << std::endl;
    send_data(client_ip, client_port, "RESP", "Custom response to " + command);
}

```

## main.cpp
``` cpp
#include "CustomUdpComm.hpp"

CustomUdpComm* comm_L;
CustomUdpComm* comm_R;

void on_receive_callback(const std::string& command, const std::string& data, const sockaddr_in& addr) {
    std::string client_ip = inet_ntoa(addr.sin_addr);
    int client_port = ntohs(addr.sin_port);
    std::cout << "Received message from " << client_ip << ":" << client_port << " - Command: " << command << ", Data: " << data << std::endl;
}

int main() {
    // 수신용 소켓 설정
    std::string local_ip_L = "0.0.0.0";
    int receive_port_L = 5003;
    std::string local_ip_R = "0.0.0.0";
    int receive_port_R = 5004;

    // 송신용 대상 설정
    std::string remote_ip_L = "192.168.0.1";
    int send_port_L = 5002;
    std::string remote_ip_R = "192.168.0.2";
    int send_port_R = 5002;

    comm_L = new CustomUdpComm(local_ip_L, receive_port_L);
    comm_R = new CustomUdpComm(local_ip_R, receive_port_R);

    comm_L->set_receive_callback(on_receive_callback);
    comm_R->set_receive_callback(on_receive_callback);

    while (true) {
        std::string command;
        std::string data;

        std::cout << "Enter command (4 characters): ";
        std::cin >> command;
        std::cout << "Enter data: ";
        std::cin >> data;

        if (command.size() > 4) {
            std::cerr << "Warning: Command length exceeds 4 characters. Truncating." << std::endl;
            command = command.substr(0, 4);
        }

        comm_L->send_data(remote_ip_L, send_port_L, command, data);
        comm_R->send_data(remote_ip_R, send_port_R, command, data);
    }

    return 0;
}


```

## Makefile
``` cpp
# 컴파일러
CXX = g++

# 컴파일러 플래그
CXXFLAGS = -I./include -Wall

# 링크 플래그
LDFLAGS = -lalchemy -lxenomai

# 소스 파일
SRCS = src/main.cpp src/UdpComm.cpp src/CustomUdp

``` 
