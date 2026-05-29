---
title: "TCP/IP server &amp; send file sample"
excerpt_separator: "<!--more-->"
date: 2020-09-22 14:30:55 +0900
categories:
  - algorithm
tags:
  - algorithm
  - 알고리즘

toc : true
toc_sticky : true
---

sample code

```
 #include <unistd.h>                                                                                                                                                                                                                          
 #include <stdio.h>                                                                                                                                                                                                                           
 #include <sys/socket.h>                                                                                                                                                                                                                      
 #include <stdlib.h>                                                                                                                                                                                                                          
 #include <netinet/in.h>                                                                                                                                                                                                                      
 #include <string.h>                                                                                                                                                                                                                          
 #include <fcntl.h>                                                                                                                                                                                                                           
 #define PORT 8080                                                                                                                                                                                                                            
                                                                                                                                                                                                                                              
 void send_image(int socket)                                                                                                                                                                                                                  
 {                                                                                                                                                                                                                                            
     FILE *picture;                                                                                                                                                                                                                           
     int size, stat, read_size;                                                                                                                                                                                                               
     char send_buffer[320*240];                                                                                                                                                                                                               
     picture = fopen("myimage.rgb","r");                                                                                                                                                                                                      
                                                                                                                                                                                                                                              
     fseek(picture, 0, SEEK_END);                                                                                                                                                                                                             
     size = ftell(picture);                                                                                                                                                                                                                   
     fseek(picture, 0, SEEK_SET);                                                                                                                                                                                                             
                                                                                                                                                                                                                                              
     read_size = fread(send_buffer, 1, sizeof(send_buffer)-1, picture);                                                                                                                                                                       
                                                                                                                                                                                                                                              
     do{                                                                                                                                                                                                                                      
         stat = write(socket, send_buffer, read_size);                                                                                                                                                                                        
     }while(stat < 0);                                                                                                                                                                                                                        
                                                                                                                                                                                                                                              
 }                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                              
 int main(int argc, char const *argv[])                                                                                                                                                                                                       
 {                                                                                                                                                                                                                                            
     int server_fd, new_socket, valread;                                                                                                                                                                                                      
     struct sockaddr_in address;                                                                                                                                                                                                              
     int opt = 1;                                                                                                                                                                                                                             
     int addrlen = sizeof(address);                                                                                                                                                                                                           
     char buffer[1024] = {0};                                                                                                                                                                                                                 
     char image[320*240] = {0,};                                                                                                                                                                                                              
     char data[10]="hello test";                                                                                                                                                                                                              
                                                                                                                                                                                                                                              
     int i =0;                                                                                                                                                                                                                                
     for(i=0; i < 320*240; i++)                                                                                                                                                                                                               
         image[i]=i%255;                                                                                                                                                                                                                      
                                                                                                                                                                                                                                              
     if((server_fd = socket(AF_INET, SOCK_STREAM, 0)) == 0 )                                                                                                                                                                                  
     {                                                                                                                                                                                                                                        
         perror("socket failed");                                                                                                                                                                                                             
         exit(EXIT_FAILURE);                                                                                                                                                                                                                  
     }                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                              
     if(setsockopt(server_fd, SOL_SOCKET, SO_REUSEADDR | SO_REUSEPORT, & opt, sizeof(opt)))                                                                                                                                                   
     {                                                                                                                                                                                                                                        
         perror("setsockopt");                                                                                                                                                                                                                
         exit(EXIT_FAILURE);                                                                                                                                                                                                                  
     } 
    address.sin_family = AF_INET;                                                                                                                                                                                                            
     address.sin_addr.s_addr = INADDR_ANY;                                                                                                                                                                                                    
     address.sin_port = htons(PORT);                                                                                                                                                                                                          
                                                                                                                                                                                                                                              
     if(bind(server_fd, (struct sockaddr *)&address, sizeof(address))<0)                                                                                                                                                                      
     {                                                                                                                                                                                                                                        
         perror("bind failed");                                                                                                                                                                                                               
         exit(EXIT_FAILURE);                                                                                                                                                                                                                  
     }                                                                                                                                                                                                                                        
     printf("server bind \n");                                                                                                                                                                                                                
                                                                                                                                                                                                                                              
     if(listen(server_fd, 3)< 0)                                                                                                                                                                                                              
     {                                                                                                                                                                                                                                        
         perror("listen");                                                                                                                                                                                                                    
         exit(EXIT_FAILURE);                                                                                                                                                                                                                  
     }                                                                                                                                                                                                                                        
     printf("server listen \n");                                                                                                                                                                                                              
                                                                                                                                                                                                                                              
     if((new_socket = accept(server_fd, (struct sockaddr *)&address, (socklen_t*)&addrlen))<0)                                                                                                                                                
     {                                                                                                                                                                                                                                        
         printf("server accept failed....\n");                                                                                                                                                                                                
         exit(0);                                                                                                                                                                                                                             
     }                                                                                                                                                                                                                                        
     printf("server accept \n");                                                                                                                                                                                                              
                                                                                                                                                                                                                                              
     int read_size = 0;                                                                                                                                                                                                                       
     while(1)                                                                                                                                                                                                                                 
     {                                                                                                                                                                                                                                        
         read_size = read(new_socket, buffer, 1024);                                                                                                                                                                                          
         if(read_size > 0 )                                                                                                                                                                                                                   
         {                                                                                                                                                                                                                                    
                 printf("%s\n",buffer);                                                                                                                                                                                                       
             if(!strncmp(buffer, "IMG", 3))                                                                                                                                                                                                   
             {                                                                                                                                                                                                                                
                 send_image(new_socket);                                                                                                                                                                                                      
                 printf("image sent\n");                                                                                                                                                                                                      
             }                                                                                                                                                                                                                                
             if(!strncmp(buffer, "DAT", 3))                                                                                                                                                                                                   
             {                                                                                                                                                                                                                                
                 write(new_socket, data, strlen(data));                                                                                                                                                                                       
                 printf("data  sent\n");                                                                                                                                                                                                      
             }                                                                                                                                                                                                                                
         }                                                                                                                                                                                                                                    
     }                                                                                                                                                                                                                                        
     return 0;                                                                                                                                                                                                                                
 }                                                             
```

windows program([receiver](https://younlea.github.io/Etc/c-winform-TCPIP-client-receive-image-and-draw/))

ref : [link](https://stackoverflow.com/questions/15445207/sending-image-jpeg-through-socket-in-c-linux)

ref : [iamge send and recevie](http://http://chuanshuoge1.blogspot.com/2014/02/c-tcpip-sending-image.html)

\