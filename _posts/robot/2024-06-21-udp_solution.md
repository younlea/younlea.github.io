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


# refactoring 
## udp_comm.cpp
``` cpp
```
