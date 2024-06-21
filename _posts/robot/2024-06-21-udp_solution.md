---
title: "UDP sample code add cmd and data"
excerpt_separator: "<!--more-->"
categories:
  - robot
tags:
  - robot

toc : true
toc_sticky : true
---

# 다른 PC에서 돌리고 cmd 4byte와 data 보내는 방식
```
아래는 B PC와 C PC 간의 UDP 통신을 위한 코드를 작성했습니다. 각 메시지는 command와 data로 구성되며, co
mmand는 고정 길이로 설정하고 data는 가변 길이로 설정했습니다. 이 방식은 향후 다른 command를 추가하기 쉽게 합니다

	•	B PC (C++): 포트 5002에서 수신하고 포트 5003으로 송신. 수신한 메시지의 첫 4바이트는 command, 나머지는 data.
	•	C PC (Python): 포트 5003에서 수신하고 포트 5002로 송신. 수신한 메시지의 첫 4바이트는 command, 나머지는 data.
이 코드에서는 command를 고정 길이 4바이트로 설정하여 메시지의 앞부분에 위치시키고, data는 가변 길이로 설정합니다.
이를 통해 메시지의 구성 요소를 쉽게 식별하고 처리할 수 있습니다.
```

## udp_comm.cpp
``` cpp
#include <iostream>
#include <thread>
#include <cstring>
#include <arpa/inet.h>
#include <unistd.h>

#define B_RECV_PORT 5002
#define C_SEND_PORT 5003
#define BUFFER_SIZE 1024

void receive_udp(int sockfd) {
    char buffer[BUFFER_SIZE];
    struct sockaddr_in cliaddr;
    socklen_t len = sizeof(cliaddr);

    while (true) {
        int n = recvfrom(sockfd, buffer, BUFFER_SIZE, MSG_WAITALL, (struct sockaddr*)&cliaddr, &len);
        if (n < 0) {
            std::cerr << "recvfrom failed" << std::endl;
            continue;
        }
        buffer[n] = '\0';
        std::string sender_ip = inet_ntoa(cliaddr.sin_addr);
        int sender_port = ntohs(cliaddr.sin_port);

        // Extract command and data
        std::string received(buffer);
        std::string command = received.substr(0, 4);
        std::string data = received.substr(4);

        if (sender_port == C_SEND_PORT) {
            std::cout << "Received message from C: Command: " << command << ", Data: " << data << " from " << sender_ip << ":" << sender_port << std::endl;
        } else {
            std::cout << "Received message: Command: " << command << ", Data: " << data << " from " << sender_ip << ":" << sender_port << std::endl;
        }
    }
}

void send_udp(int sockfd, const char* ip, int port) {
    struct sockaddr_in servaddr;
    memset(&servaddr, 0, sizeof(servaddr));

    servaddr.sin_family = AF_INET;
    servaddr.sin_port = htons(port);
    servaddr.sin_addr.s_addr = inet_addr(ip);

    while (true) {
        std::string command, data;
        std::cout << "Enter command: ";
        std::cin >> command;
        std::cout << "Enter data: ";
        std::cin >> data;

        // Ensure command is 4 characters long
        command.resize(4, ' ');

        std::string message = command + data;

        int n = sendto(sockfd, message.c_str(), message.size(), MSG_CONFIRM, (const struct sockaddr*)&servaddr, sizeof(servaddr));
        if (n < 0) {
            std::cerr << "sendto failed" << std::endl;
        }
    }
}

int main() {
    int recv_sockfd, send_sockfd_c;

    // 수신 소켓 생성 및 바인드
    if ((recv_sockfd = socket(AF_INET, SOCK_DGRAM, 0)) < 0) {
        std::cerr << "Receive socket creation failed" << std::endl;
        return -1;
    }

    struct sockaddr_in recvaddr;
    memset(&recvaddr, 0, sizeof(recvaddr));
    recvaddr.sin_family = AF_INET;
    recvaddr.sin_addr.s_addr = INADDR_ANY;
    recvaddr.sin_port = htons(B_RECV_PORT);

    if (bind(recv_sockfd, (const struct sockaddr*)&recvaddr, sizeof(recvaddr)) < 0) {
        std::cerr << "Receive bind failed: " << strerror(errno) << std::endl;
        close(recv_sockfd);
        return -1;
    }

    // 송신 소켓 생성 (C)
    if ((send_sockfd_c = socket(AF_INET, SOCK_DGRAM, 0)) < 0) {
        std::cerr << "Send socket creation failed" << std::endl;
        close(recv_sockfd);
        return -1;
    }

    std::thread recv_thread(receive_udp, recv_sockfd);
    std::thread send_thread_c(send_udp, send_sockfd_c, "C_PC_IP", C_SEND_PORT); // C PC의 IP 주소로 설정

    recv_thread.join();
    send_thread_c.join();

    close(recv_sockfd);
    close(send_sockfd_c);
    return 0;
}
```

## udp_comm.python
``` python
import socket
import threading

class UdpComm:
    def __init__(self, local_ip, local_port, remote_ip, remote_port):
        self.local_ip = local_ip
        self.local_port = local_port
        self.remote_ip = remote_ip
        self.remote_port = remote_port
        
        self.sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
        self.sock.bind((self.local_ip, self.local_port))

        self.recv_thread = threading.Thread(target=self.receive_udp)
        self.recv_thread.daemon = True
        self.recv_thread.start()

    def receive_udp(self):
        while True:
            data, addr = self.sock.recvfrom(1024)  # 버퍼 크기 1024 바이트
            self.on_receive(data.decode(), addr)

    def send_data(self, command, data):
        # Ensure command is 4 characters long
        command = command.ljust(4)
        message = command + data
        self.sock.sendto(message.encode(), (self.remote_ip, self.remote_port))

    def on_receive(self, message, addr):
        command = message[:4]
        data = message[4:]
        print(f"Received message: Command: {command}, Data: {data} from {addr}")

# IP 주소와 포트 설정
local_ip = "0.0.0.0"  # 모든 인터페이스에서 수신
local_port = 5003
remote_ip = "B_PC_IP"  # B PC의 IP 주소
remote_port = 5002

# UdpComm 인스턴스 생성
udp_comm = UdpComm(local_ip, local_port, remote_ip, remote_port)

# 사용자 입력을 통한 데이터 송신
while True:
    command = input("Enter command: ")
    data = input("Enter data: ")
    udp_comm.send_data(command, data)
```

-----------------------------
# refactoring
-----------------------------
```
아래는 UdpComm 클래스를 상속받아 command와 data 처리를 커스터마이즈할 수 있도록 리팩토링한 코드입니다.
•	UdpComm 클래스는 UDP 통신의 기본 기능을 제공합니다. 이 클래스는 수신한 데이터를 처리하는 on_receive 메서드를 가집니다.
	•	CustomUdpComm 클래스는 UdpComm 클래스를 상속받아 on_receive 메서드를 오버라이드하여 수신한 데이터를 커스터마이즈된 방식으로 처리합니다.
	•	main.cpp에서는 CustomUdpComm 인스턴스를 생성하고, command와 data를 전송 및 수신하는 기본 로직을 구현합니다.

이 구조를 통해 UdpComm 클래스의 기본 기능을 유지하면서, 수신한 데이터를 처리하는 방식을 쉽게 변경할 수 있습니다.
```
------------------------------
## udp_comm.h
``` cpp
#ifndef UDPCOMM_HPP
#define UDPCOMM_HPP

#include <string>
#include <thread>
#include <arpa/inet.h>

class UdpComm {
public:
    UdpComm(const std::string& local_ip, int local_port, const std::string& remote_ip, int remote_port);
    virtual ~UdpComm();

    void start();
    void stop();
    void send_data(const std::string& command, const std::string& data);

protected:
    virtual void on_receive(const std::string& command, const std::string& data, const std::string& sender_ip, int sender_port);

private:
    void receive_udp();
    
    int recv_sockfd;
    int send_sockfd;
    struct sockaddr_in servaddr;
    std::thread recv_thread;
    bool running;
};

#endif // UDPCOMM_HPP
```
## udp_comm.cpp
``` cpp
#include "UdpComm.hpp"
#include <iostream>
#include <cstring>
#include <unistd.h>

#define BUFFER_SIZE 1024

UdpComm::UdpComm(const std::string& local_ip, int local_port, const std::string& remote_ip, int remote_port) {
    running = false;

    // 수신 소켓 생성 및 바인드
    if ((recv_sockfd = socket(AF_INET, SOCK_DGRAM, 0)) < 0) {
        std::cerr << "Receive socket creation failed" << std::endl;
        throw std::runtime_error("Receive socket creation failed");
    }

    struct sockaddr_in recvaddr;
    memset(&recvaddr, 0, sizeof(recvaddr));
    recvaddr.sin_family = AF_INET;
    recvaddr.sin_addr.s_addr = inet_addr(local_ip.c_str());
    recvaddr.sin_port = htons(local_port);

    if (bind(recv_sockfd, (const struct sockaddr*)&recvaddr, sizeof(recvaddr)) < 0) {
        std::cerr << "Receive bind failed: " << strerror(errno) << std::endl;
        close(recv_sockfd);
        throw std::runtime_error("Receive bind failed");
    }

    // 송신 소켓 생성
    if ((send_sockfd = socket(AF_INET, SOCK_DGRAM, 0)) < 0) {
        std::cerr << "Send socket creation failed" << std::endl;
        close(recv_sockfd);
        throw std::runtime_error("Send socket creation failed");
    }

    memset(&servaddr, 0, sizeof(servaddr));
    servaddr.sin_family = AF_INET;
    servaddr.sin_port = htons(remote_port);
    servaddr.sin_addr.s_addr = inet_addr(remote_ip.c_str());
}

UdpComm::~UdpComm() {
    stop();
    close(recv_sockfd);
    close(send_sockfd);
}

void UdpComm::start() {
    running = true;
    recv_thread = std::thread(&UdpComm::receive_udp, this);
}

void UdpComm::stop() {
    running = false;
    if (recv_thread.joinable()) {
        recv_thread.join();
    }
}

void UdpComm::send_data(const std::string& command, const std::string& data) {
    std::string cmd = command;
    cmd.resize(4, ' ');  // Ensure command is 4 characters long
    std::string message = cmd + data;

    int n = sendto(send_sockfd, message.c_str(), message.size(), MSG_CONFIRM, (const struct sockaddr*)&servaddr, sizeof(servaddr));
    if (n < 0) {
        std::cerr << "sendto failed" << std::endl;
    }
}

void UdpComm::receive_udp() {
    char buffer[BUFFER_SIZE];
    struct sockaddr_in cliaddr;
    socklen_t len = sizeof(cliaddr);

    while (running) {
        int n = recvfrom(recv_sockfd, buffer, BUFFER_SIZE, MSG_WAITALL, (struct sockaddr*)&cliaddr, &len);
        if (n < 0) {
            if (running) {
                std::cerr << "recvfrom failed" << std::endl;
            }
            continue;
        }
        buffer[n] = '\0';
        std::string received(buffer);
        std::string command = received.substr(0, 4);
        std::string data = received.substr(4);

        std::string sender_ip = inet_ntoa(cliaddr.sin_addr);
        int sender_port = ntohs(cliaddr.sin_port);
        
        on_receive(command, data, sender_ip, sender_port);
    }
}

void UdpComm::on_receive(const std::string& command, const std::string& data, const std::string& sender_ip, int sender_port) {
    std::cout << "Received message: Command: " << command << ", Data: " << data << " from " << sender_ip << ":" << sender_port << std::endl;
}
```
## CustomUdpComm.hpp
``` cpp
#ifndef CUSTOMUDPCOMM_HPP
#define CUSTOMUDPCOMM_HPP

#include "UdpComm.hpp"

class CustomUdpComm : public UdpComm {
public:
    CustomUdpComm(const std::string& local_ip, int local_port, const std::string& remote_ip, int remote_port);
    virtual ~CustomUdpComm();

protected:
    void on_receive(const std::string& command, const std::string& data, const std::string& sender_ip, int sender_port) override;
};

#endif // CUSTOMUDPCOMM_HPP
```
## CustomUdpComm.cpp
``` cpp
#include "CustomUdpComm.hpp"
#include <iostream>

CustomUdpComm::CustomUdpComm(const std::string& local_ip, int local_port, const std::string& remote_ip, int remote_port)
    : UdpComm(local_ip, local_port, remote_ip, remote_port) {}

CustomUdpComm::~CustomUdpComm() {}

void CustomUdpComm::on_receive(const std::string& command, const std::string& data, const std::string& sender_ip, int sender_port) {
    std::cout << "Custom handler - Received message: Command: " << command << ", Data: " << data << " from " << sender_ip << ":" << sender_port << std::endl;
}
```
## main.cpp
``` cpp
#include "CustomUdpComm.hpp"
#include <iostream>

int main() {
    std::string local_ip = "0.0.0.0";  // 모든 인터페이스에서 수신
    int local_port = 5002;
    std::string remote_ip = "C_PC_IP";  // C PC의 IP 주소로 설정
    int remote_port = 5003;

    try {
        CustomUdpComm udp_comm(local_ip, local_port, remote_ip, remote_port);
        udp_comm.start();

        while (true) {
            std::string command, data;
            std::cout << "Enter command: ";
            std::cin >> command;
            std::cout << "Enter data: ";
            std::cin >> data;

            udp_comm.send_data(command, data);
        }

        udp_comm.stop();
    } catch (const std::exception& e) {
        std::cerr << "Exception: " << e.what() << std::endl;
    }

    return 0;
}
```
## Makefile
``` cpp
CXX = g++
CXXFLAGS = -std=c++11 -Wall

all: udp_comm

udp_comm: main.o UdpComm.o CustomUdpComm.o
	$(CXX) $(CXXFLAGS) -o udp_comm main.o UdpComm.o CustomUdpComm.o

main.o: main.cpp CustomUdpComm.hpp
	$(CXX) $(CXXFLAGS) -c main.cpp

UdpComm.o: UdpComm.cpp UdpComm.hpp
	$(CXX) $(CXXFLAGS) -c UdpComm.cpp

CustomUdpComm.o: CustomUdpComm.cpp CustomUdpComm.hpp UdpComm.hpp
	$(CXX) $(CXXFLAGS) -c CustomUdpComm.cpp

clean:
	rm -f udp_comm main.o UdpComm.o CustomUdpComm.o
```

# 폴더 구조 변경 리펙토링
```
 main.cpp는 CustomUdpComm 객체를 생성하고, 이를 사용하여 데이터 송수신을 수행합니다. daemon.cpp는 RT 스레드에서 주기적으로
데이터를 송신하는 함수를 포함하고 있습니다. main.cpp에서 pthread_create를 사용하여 RT 스레드를 생성하고,
daemon.cpp의 rt_thread_func 함수를 호출합니다
``` 
```
project/
├── include/
│   ├── UdpComm.hpp
│   └── CustomUdpComm.hpp
├── src/
│   ├── UdpComm.cpp
│   └── CustomUdpComm.cpp
├── daemon/
│   └── daemon.cpp
├── main.cpp
└── Makefile
```
## UdpComm.hpp
``` cpp
#ifndef UDPCOMM_HPP
#define UDPCOMM_HPP

#include <string>
#include <thread>
#include <arpa/inet.h>

class UdpComm {
public:
    UdpComm(const std::string& local_ip, int local_port, const std::string& remote_ip, int remote_port);
    virtual ~UdpComm();

    void start();
    void stop();
    void send_data(const std::string& command, const std::string& data);

protected:
    virtual void on_receive(const std::string& command, const std::string& data, const std::string& sender_ip, int sender_port);

private:
    void receive_udp();
    
    int recv_sockfd;
    int send_sockfd;
    struct sockaddr_in servaddr;
    std::thread recv_thread;
    bool running;
};

#endif // UDPCOMM_HPP
```
## UdpComm.cpp
``` cpp
#include "UdpComm.hpp"
#include <iostream>
#include <cstring>
#include <unistd.h>

#define BUFFER_SIZE 1024

UdpComm::UdpComm(const std::string& local_ip, int local_port, const std::string& remote_ip, int remote_port) {
    running = false;

    // 수신 소켓 생성 및 바인드
    if ((recv_sockfd = socket(AF_INET, SOCK_DGRAM, 0)) < 0) {
        std::cerr << "Receive socket creation failed" << std::endl;
        throw std::runtime_error("Receive socket creation failed");
    }

    struct sockaddr_in recvaddr;
    memset(&recvaddr, 0, sizeof(recvaddr));
    recvaddr.sin_family = AF_INET;
    recvaddr.sin_addr.s_addr = inet_addr(local_ip.c_str());
    recvaddr.sin_port = htons(local_port);

    if (bind(recv_sockfd, (const struct sockaddr*)&recvaddr, sizeof(recvaddr)) < 0) {
        std::cerr << "Receive bind failed: " << strerror(errno) << std::endl;
        close(recv_sockfd);
        throw std::runtime_error("Receive bind failed");
    }

    // 송신 소켓 생성
    if ((send_sockfd = socket(AF_INET, SOCK_DGRAM, 0)) < 0) {
        std::cerr << "Send socket creation failed" << std::endl;
        close(recv_sockfd);
        throw std::runtime_error("Send socket creation failed");
    }

    memset(&servaddr, 0, sizeof(servaddr));
    servaddr.sin_family = AF_INET;
    servaddr.sin_port = htons(remote_port);
    servaddr.sin_addr.s_addr = inet_addr(remote_ip.c_str());
}

UdpComm::~UdpComm() {
    stop();
    close(recv_sockfd);
    close(send_sockfd);
}

void UdpComm::start() {
    running = true;
    recv_thread = std::thread(&UdpComm::receive_udp, this);
}

void UdpComm::stop() {
    running = false;
    if (recv_thread.joinable()) {
        recv_thread.join();
    }
}

void UdpComm::send_data(const std::string& command, const std::string& data) {
    std::string cmd = command;
    cmd.resize(4, ' ');  // Ensure command is 4 characters long
    std::string message = cmd + data;

    int n = sendto(send_sockfd, message.c_str(), message.size(), MSG_CONFIRM, (const struct sockaddr*)&servaddr, sizeof(servaddr));
    if (n < 0) {
        std::cerr << "sendto failed" << std::endl;
    }
}

void UdpComm::receive_udp() {
    char buffer[BUFFER_SIZE];
    struct sockaddr_in cliaddr;
    socklen_t len = sizeof(cliaddr);

    while (running) {
        int n = recvfrom(recv_sockfd, buffer, BUFFER_SIZE, MSG_WAITALL, (struct sockaddr*)&cliaddr, &len);
        if (n < 0) {
            if (running) {
                std::cerr << "recvfrom failed" << std::endl;
            }
            continue;
        }
        buffer[n] = '\0';
        std::string received(buffer);
        std::string command = received.substr(0, 4);
        std::string data = received.substr(4);

        std::string sender_ip = inet_ntoa(cliaddr.sin_addr);
        int sender_port = ntohs(cliaddr.sin_port);
        
        on_receive(command, data, sender_ip, sender_port);
    }
}

void UdpComm::on_receive(const std::string& command, const std::string& data, const std::string& sender_ip, int sender_port) {
    std::cout << "Received message: Command: " << command << ", Data: " << data << " from " << sender_ip << ":" << sender_port << std::endl;
}
```
## CustomUdpComm.hpp
``` cpp
#ifndef CUSTOMUDPCOMM_HPP
#define CUSTOMUDPCOMM_HPP

#include "UdpComm.hpp"

class CustomUdpComm : public UdpComm {
public:
    CustomUdpComm(const std::string& local_ip, int local_port, const std::string& remote_ip, int remote_port);
    virtual ~CustomUdpComm();

protected:
    void on_receive(const std::string& command, const std::string& data, const std::string& sender_ip, int sender_port) override;
};

#endif // CUSTOMUDPCOMM_HPP
```
## CustomUdpComm.cpp
``` cpp
#include "CustomUdpComm.hpp"
#include <iostream>

CustomUdpComm::CustomUdpComm(const std::string& local_ip, int local_port, const std::string& remote_ip, int remote_port)
    : UdpComm(local_ip, local_port, remote_ip, remote_port) {}

CustomUdpComm::~CustomUdpComm() {}

void CustomUdpComm::on_receive(const std::string& command, const std::string& data, const std::string& sender_ip, int sender_port) {
    std::cout << "Custom handler - Received message: Command: " << command << ", Data: " << data << " from " << sender_ip << ":" << sender_port << std::endl;
}
```
## main.cpp
``` cpp
#include "CustomUdpComm.hpp"
#include <iostream>
#include <pthread.h>

CustomUdpComm* udp_comm_ptr = nullptr;

void* rt_thread_func(void* arg);

int main() {
    std::string local_ip = "0.0.0.0";  // 모든 인터페이스에서 수신
    int local_port = 5002;
    std::string remote_ip = "C_PC_IP";  // C PC의 IP 주소로 설정
    int remote_port = 5003;

    try {
        udp_comm_ptr = new CustomUdpComm(local_ip, local_port, remote_ip, remote_port);
        udp_comm_ptr->start();

        pthread_t rt_thread;
        if (pthread_create(&rt_thread, NULL, rt_thread_func, NULL)) {
            std::cerr << "Error creating RT thread" << std::endl;
            return 1;
        }

        while (true) {
            std::string command, data;
            std::cout << "Enter command: ";
            std::cin >> command;
            std::cout << "Enter data: ";
            std::cin >> data;

            udp_comm_ptr->send_data(command, data);
        }

        udp_comm_ptr->stop();
        delete udp_comm_ptr;
    } catch (const std::exception& e) {
        std::cerr << "Exception: " << e.what() << std::endl;
    }

    return 0;
}
```
## daemon.cpp
``` cpp
#include "../include/CustomUdpComm.hpp"
#include <iostream>
#include <unistd.h>
#include <pthread.h>

extern CustomUdpComm* udp_comm_ptr;

void* rt_thread_func(void* arg) {
    while (true) {
        // Example of sending data periodically
        //udp_comm_ptr->send_data("PING", "Hello from RT thread");

        // Sleep for 1 second
        sleep(1);
    }

    return nullptr;
}
```

## Makefile
``` cpp
CXX = g++
CXXFLAGS = -std=c++11 -Wall
INCLUDES = -I./include
SRC_DIR = ./src
OBJ_DIR = ./obj
DAEMON_DIR = ./daemon

all: udp_comm

udp_comm: $(OBJ_DIR)/main.o $(OBJ_DIR)/UdpComm.o $(OBJ_DIR)/CustomUdpComm.o $(OBJ_DIR)/daemon.o
	$(CXX) $(CXXFLAGS) -o udp_comm $(OBJ_DIR)/main.o $(OBJ_DIR)/UdpComm.o $(OBJ_DIR)/CustomUdpComm.o $(OBJ_DIR)/daemon.o

$(OBJ_DIR)/main.o: $(SRC_DIR)/main.cpp include/CustomUdpComm.hpp include/UdpComm.hpp | $(OBJ_DIR)
	$(CXX) $(CXXFLAGS) $(INCLUDES) -c $(SRC_DIR)/main.cpp -o $(OBJ_DIR)/main.o

$(OBJ_DIR)/UdpComm.o: $(SRC_DIR)/UdpComm.cpp include/UdpComm.hpp | $(OBJ_DIR)
	$(CXX) $(CXXFLAGS) $(INCLUDES) -c $(SRC_DIR)/UdpComm.cpp -o $(OBJ_DIR)/UdpComm.o

$(OBJ_DIR)/CustomUdpComm.o: $(SRC_DIR)/CustomUdpComm.cpp include/CustomUdpComm.hpp include/UdpComm.hpp | $(OBJ_DIR)
	$(CXX) $(CXXFLAGS) $(INCLUDES) -c $(SRC_DIR)/CustomUdpComm.cpp -o $(OBJ_DIR)/CustomUdpComm.o

$(OBJ_DIR)/daemon.o: $(DAEMON_DIR)/daemon.cpp include/CustomUdpComm.hpp include/UdpComm.hpp | $(OBJ_DIR)
	$(CXX) $(CXXFLAGS) $(INCLUDES) -c $(DAEMON_DIR)/daemon.cpp -o $(OBJ_DIR)/daemon.o

$(OBJ_DIR):
	mkdir -p $(OBJ_DIR)

clean:
	rm -f udp_comm $(OBJ_DIR)/*.o
```

# python side
## udp_comm.py
```python
import socket
import threading

class UdpComm:
    def __init__(self, local_ip, local_port, remote_ip, remote_port):
        self.local_ip = local_ip
        self.local_port = local_port
        self.remote_ip = remote_ip
        self.remote_port = remote_port
        
        self.sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
        self.sock.bind((self.local_ip, self.local_port))

        self.recv_thread = threading.Thread(target=self.receive_udp)
        self.recv_thread.daemon = True
        self.recv_thread.start()

    def receive_udp(self):
        while True:
            data, addr = self.sock.recvfrom(1024)  # 버퍼 크기 1024 바이트
            self.on_receive(data.decode(), addr)

    def send_data(self, command, data):
        # Ensure command is 4 characters long
        command = command.ljust(4)
        message = command + data
        self.sock.sendto(message.encode(), (self.remote_ip, self.remote_port))

    def on_receive(self, message, addr):
        command = message[:4]
        data = message[4:]
        print(f"Received message: Command: {command}, Data: {data} from {addr}")

# 위 코드를 udp_comm.py 파일로 저장해야 합니다.
```
## main.py
``` python
from udp_comm import UdpComm  # UdpComm 클래스가 정의된 파일명에 맞게 수정해야 합니다.

# IP 주소와 포트 설정
local_ip = "127.0.0.1"  # 모든 인터페이스에서 수신
local_port = 5003
remote_ip = "127.0.0.1"  # B PC의 IP 주소 (자기 자신)
remote_port = 5002

# UdpComm 인스턴스 생성
udp_comm = UdpComm(local_ip, local_port, remote_ip, remote_port)

# 사용자 입력을 통한 데이터 송신
while True:
    command = input("Enter command: ")
    data = input("Enter data: ")
    udp_comm.send_data(command, data)
```

``` 
	•	main.py: UdpComm 클래스를 import하여 인스턴스를 생성하고, 사용자 입력을 받아 send_data 메서드를 통해 데이터를 송신합니다.
	•	udp_comm.py: UdpComm 클래스 정의 및 구현입니다. 생성자에서는 소켓을 바인딩하고, 별도의 쓰레드에서 데이터를 수신합니다. send_data 메서드는 입력받은 command와 data를 UDP로 전송하며, on_receive 메서드는 데이터를 수신하면서 콘솔에 출력합니다.

실행 방법

	1.	udp_comm.py 파일을 만들어 위의 코드를 저장합니다.
	2.	main.py 파일을 만들어 위의 코드를 저장합니다.
	3.	각 PC의 IP 주소와 포트 번호를 자신의 환경에 맞게 수정합니다.
	4.	먼저 udp_comm.py를 실행하여 UDP 통신을 대기시킵니다.
	5.	그 다음 main.py를 실행하여 데이터를 입력하고 전송해보세요.

이렇게 하면 C++로 작성된 서버와 Python으로 작성된 클라이언트가 데이터를 주고받을 수 있습니다.
```

# python call back 방식
## main.py
``` python
from udp_client import UDPClient

def on_receive_callback(message, addr):
    print(f"Received message from {addr}: {message}")

if __name__ == "__main__":
    server_ip = '127.0.0.1'
    receive_port = 5003
    send_port = 5002

    client = UDPClient(server_ip, receive_port, send_port)
    client.set_receive_callback(on_receive_callback)

    while True:
        message = input("Enter message to send: ")
        client.send_message(message)
```
## udp_client.py
``` python
import socket
import threading

class UDPClient:
    def __init__(self, server_ip, receive_port, send_port):
        self.server_ip = server_ip
        self.receive_port = receive_port
        self.send_port = send_port
        self.client_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

        self.receive_callback = None
        self.receive_thread = threading.Thread(target=self.receive_loop)
        self.receive_thread.daemon = True
        self.receive_thread.start()

        self.client_socket.bind(('0.0.0.0', self.receive_port))  # 수신할 포트 설정

    def set_receive_callback(self, callback):
        self.receive_callback = callback

    def receive_loop(self):
        while True:
            data, addr = self.client_socket.recvfrom(1024)
            if self.receive_callback:
                self.receive_callback(data.decode(), addr)

    def send_message(self, message):
        self.client_socket.sendto(message.encode(), (self.server_ip, self.send_port))

    def close(self):
        self.client_socket.close()

# 테스트용 예제
if __name__ == "__main__":
    def on_receive_callback(message, addr):
        print(f"Received message from {addr}: {message}")

    server_ip = '127.0.0.1'
    receive_port = 5003
    send_port = 5002

    client = UDPClient(server_ip, receive_port, send_port)
    client.set_receive_callback(on_receive_callback)

    while True:
        message = input("Enter message to send: ")
        client.send_message(message)
```

# python call back 방식(보낼ip와 받는 ip가 다를때)
## main.py
``` python
from udp_client import UDPClient

def on_receive_callback(command, data, addr):
    print(f"Received message from {addr}: Command: {command}, Data: {data}")

if __name__ == "__main__":
    local_ip = '127.0.0.1'  # 수신할 IP
    server_ip = '192.168.1.100'  # 보낼 IP
    receive_port = 5003  # 수신할 포트
    send_port = 5002  # 보낼 포트

    client = UDPClient(local_ip, server_ip, receive_port, send_port)
    client.set_receive_callback(on_receive_callback)

    while True:
        command = input("Enter command (4 characters): ")
        data = input("Enter data: ")
        client.send_message(command, data)
```
## udp_client.py
``` python
import socket
import threading

class UDPClient:
    def __init__(self, local_ip, server_ip, receive_port, send_port):
        self.local_ip = local_ip
        self.server_ip = server_ip
        self.receive_port = receive_port
        self.send_port = send_port
        self.client_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

        self.receive_callback = None
        self.receive_thread = threading.Thread(target=self.receive_loop)
        self.receive_thread.daemon = True
        self.receive_thread.start()

        self.client_socket.bind((self.local_ip, self.receive_port))  # 수신할 포트 설정

    def set_receive_callback(self, callback):
        self.receive_callback = callback

    def receive_loop(self):
        while True:
            data, addr = self.client_socket.recvfrom(1024)
            if self.receive_callback:
                command, data = self.parse_message(data.decode())
                self.receive_callback(command, data, addr)

    def send_message(self, command, data):
        # Ensure command is 4 characters long
        command = command.ljust(4)
        message = command + data
        self.client_socket.sendto(message.encode(), (self.server_ip, self.send_port))

    def parse_message(self, message):
        command = message[:4]
        data = message[4:]
        return command, data

    def close(self):
        self.client_socket.close()

# 테스트용 예제
if __name__ == "__main__":
    def on_receive_callback(command, data, addr):
        print(f"Received message from {addr}: Command: {command}, Data: {data}")

    local_ip = '127.0.0.1'  # 수신할 IP
    server_ip = '192.168.1.100'  # 보낼 IP
    receive_port = 5003  # 수신할 포트
    send_port = 5002  # 보낼 포트

    client = UDPClient(local_ip, server_ip, receive_port, send_port)
    client.set_receive_callback(on_receive_callback)

    while True:
        command = input("Enter command (4 characters): ")
        data = input("Enter data: ")
        client.send_message(command, data)
```


