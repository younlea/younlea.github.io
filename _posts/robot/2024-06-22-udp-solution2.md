---
title: "UDP sample code using callbac"
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
project_root/
├── include/
│   ├── customudpcomm.hpp
│   └── udpcomm.hpp
├── obj/
├── src/
│   ├── customudpcomm.cpp
│   ├── main.cpp
│   └── udpcomm.cpp
└── Makefile
```

## udp_comm.hpp
``` cpp
#ifndef UDPCOMM_HPP
#define UDPCOMM_HPP

#include <iostream>
#include <string>
#include <functional>
#include <thread>
#include <mutex>
#include <netinet/in.h>

class UdpComm {
public:
    using ReceiveCallback = std::function<void(const std::string&, const std::string&, const sockaddr_in&)>;

    UdpComm(const std::string& local_ip, int local_port, const std::string& remote_ip, int remote_port);
    virtual ~UdpComm();

    void send_data(const std::string& command, const std::string& data);
    void set_receive_callback(ReceiveCallback callback);

protected:
    virtual void on_receive(const std::string& command, const std::string& data, const sockaddr_in& addr) = 0;

private:
    void receive_udp();

    std::string local_ip_;
    int local_port_;
    std::string remote_ip_;
    int remote_port_;
    int sockfd_;
    sockaddr_in local_addr_;
    sockaddr_in remote_addr_;

    std::thread receive_thread_;
    std::mutex socket_mutex_;
    ReceiveCallback receive_callback_;
};

#endif // UDPCOMM_HPP
``` 
## udp_comm.cpp
``` cpp
#include "udpcomm.hpp"
#include <cstring>
#include <arpa/inet.h>
#include <unistd.h>

UdpComm::UdpComm(const std::string& local_ip, int local_port, const std::string& remote_ip, int remote_port)
    : local_ip_(local_ip), local_port_(local_port), remote_ip_(remote_ip), remote_port_(remote_port), sockfd_(-1) {
    sockfd_ = socket(AF_INET, SOCK_DGRAM, 0);
    if (sockfd_ < 0) {
        perror("socket creation failed");
        exit(EXIT_FAILURE);
    }

    memset(&local_addr_, 0, sizeof(local_addr_));
    memset(&remote_addr_, 0, sizeof(remote_addr_));

    local_addr_.sin_family = AF_INET;
    local_addr_.sin_addr.s_addr = inet_addr(local_ip.c_str());
    local_addr_.sin_port = htons(local_port);

    remote_addr_.sin_family = AF_INET;
    remote_addr_.sin_addr.s_addr = inet_addr(remote_ip.c_str());
    remote_addr_.sin_port = htons(remote_port);

    if (bind(sockfd_, (const struct sockaddr *)&local_addr_, sizeof(local_addr_)) < 0) {
        perror("bind failed");
        close(sockfd_);
        exit(EXIT_FAILURE);
    }

    receive_thread_ = std::thread(&UdpComm::receive_udp, this);
}

UdpComm::~UdpComm() {
    if (sockfd_ >= 0) {
        close(sockfd_);
    }
    if (receive_thread_.joinable()) {
        receive_thread_.join();
    }
}

void UdpComm::send_data(const std::string& command, const std::string& data) {
    if (command.length() > 4) {
        std::cerr << "Command is longer than 4 characters. Trimming to 4 characters." << std::endl;
    }
    std::string cmd = command.substr(0, 4);
    std::string message = cmd + data;

    std::lock_guard<std::mutex> lock(socket_mutex_);
    sendto(sockfd_, message.c_str(), message.length(), 0, (const struct sockaddr *)&remote_addr_, sizeof(remote_addr_));
}

void UdpComm::set_receive_callback(ReceiveCallback callback) {
    receive_callback_ = callback;
}

void UdpComm::receive_udp() {
    char buffer[1024];
    sockaddr_in sender_addr;
    socklen_t sender_addr_len = sizeof(sender_addr);

    while (true) {
        int len = recvfrom(sockfd_, buffer, sizeof(buffer), 0, (struct sockaddr *)&sender_addr, &sender_addr_len);
        if (len > 0) {
            buffer[len] = '\0';
            std::string message(buffer);
            std::string command = message.substr(0, 4);
            std::string data = message.substr(4);

            if (receive_callback_) {
                receive_callback_(command, data, sender_addr);
            } else {
                on_receive(command, data, sender_addr);
            }
        }
    }
}
```

## customudpcomm.hpp
``` cpp
#ifndef CUSTOMUDPCOMM_HPP
#define CUSTOMUDPCOMM_HPP

#include "udpcomm.hpp"

class CustomUdpComm : public UdpComm {
public:
    CustomUdpComm(const std::string& local_ip, int local_port, const std::string& remote_ip, int remote_port)
        : UdpComm(local_ip, local_port, remote_ip, remote_port) {}

protected:
    void on_receive(const std::string& command, const std::string& data, const sockaddr_in& addr) override;
};

#endif // CUSTOMUDPCOMM_HPP
```

## customudpcomm.cpp
``` cpp
#include "customudpcomm.hpp"
#include <iostream>
#include <arpa/inet.h>

void CustomUdpComm::on_receive(const std::string& command, const std::string& data, const sockaddr_in& addr) {
    std::cout << "Received message: Command=" << command << ", Data=" << data
              << ", From=" << inet_ntoa(addr.sin_addr) << ":" << ntohs(addr.sin_port) << std::endl;
}
```

## main.cpp
``` cpp
#include "customudpcomm.hpp"
#include <iostream>
#include <thread>
#include <chrono>

void handle_receive(const std::string& command, const std::string& data, const sockaddr_in& addr) {
    std::cout << "Received message: Command=" << command << ", Data=" << data
              << ", From=" << inet_ntoa(addr.sin_addr) << ":" << ntohs(addr.sin_port) << std::endl;
}

void daemon_thread(CustomUdpComm& udp_comm) {
    while (true) {
        std::this_thread::sleep_for(std::chrono::seconds(1));
    }
}

int main() {
    CustomUdpComm udp_comm("0.0.0.0", 5003, "127.0.0.1", 5002);
    udp_comm.set_receive_callback(handle_receive);

    std::thread daemon(daemon_thread, std::ref(udp_comm));

    std::string command;
    std::string data;
    while (true) {
        std::cout << "Enter command: ";
        std::getline(std::cin, command);
        std::cout << "Enter data: ";
        std::getline(std::cin, data);
        udp_comm.send_data(command, data);
    }

    if (daemon.joinable()) {
        daemon.join();
    }

    return 0;
}
```

## Makefile
``` cpp
CXX = g++
CXXFLAGS = -std=c++11 -pthread

SRC_DIR = src
OBJ_DIR = obj
INC_DIR = include

SRCS = $(SRC_DIR)/main.cpp $(SRC_DIR)/udpcomm.cpp $(SRC_DIR)/customudpcomm.cpp
OBJS = $(OBJ_DIR)/main.o $(OBJ_DIR)/udpcomm.o $(OBJ_DIR)/customudpcomm.o

all: $(OBJS)
	$(CXX) $(CXXFLAGS) -o udp_program $(OBJS)

$(OBJ_DIR)/main.o: $(SRC_DIR)/main.cpp $(INC_DIR)/udpcomm.hpp $(INC_DIR)/customudpcomm.hpp
	$(CXX) $(CXXFLAGS) -I$(INC_DIR) -c $(SRC_DIR)/main.cpp -o $(OBJ_DIR)/main.o

$(OBJ_DIR)/udpcomm.o: $(SRC_DIR)/udpcomm.cpp $(INC_DIR)/udpcomm.hpp
	$(CXX) $(CXXFLAGS) -I$(INC_DIR) -c $(SRC_DIR)/udpcomm.cpp -o $(OBJ_DIR)/udpcomm.o

$(OBJ_DIR)/customudpcomm.o: $(SRC_DIR)/customudpcomm.cpp $(INC_DIR)/customudpcomm.hpp
	$(CXX) $(CXXFLAGS) -I$(INC_DIR) -c $(SRC_DIR)/customudpcomm.cpp -o $(OBJ_DIR)/customudpcomm.o

clean:
	rm -f $(OBJ_DIR)/*.o udp_program
``` 
