---
title: "ROS를 도커에서 VNC까지 띄워서 실행해 보자 "
excerpt_separator: "<!--more-->"
categories:
  - develop_env
tags:
  - ROS
  - VNC

toc : true
toc_sticky : true
---

# docker 설치
[docker](https://www.docker.com/)   
![image](https://github.com/younlea/company_project/assets/1435846/79d934a5-adfc-4d16-9976-4eec4691e6ae)

Docker Desktop Installer.exe 파일 실행해서 설치 (설치 완료후 로그 아웃 누르면 한번 나갔다 옴)    

Docker 실행   
![image](https://github.com/younlea/company_project/assets/1435846/57830e46-6f14-40ef-a9be-dbbc9c0e04a8)

Docker sign up or sign in 
![image](https://github.com/younlea/company_project/assets/1435846/2fabf3de-f122-41a7-a616-6552685bb24c)

실행 결과  
![image](https://github.com/younlea/company_project/assets/1435846/7f910a58-6397-4eda-bf96-2dc62d5d719a)

# Pull ROS2 VNC image  
docker 이미지를 가져 온다.   
```
docker pull tiryoh/ros2-desktop-vnc:foxy  
```
![image](https://github.com/younlea/company_project/assets/1435846/107a2aa9-4e71-4848-8b7a-e8ef7b0f82c3)
다 받으면 아래 처럼 나온다. 
![image](https://github.com/younlea/company_project/assets/1435846/29fe612c-3dd0-4d89-b90f-0853b2745967)


[github](https://github.com/Tiryoh/docker-ros2-desktop-vnc)    
[docker hub](https://hub.docker.com/r/tiryoh/ros2-desktop-vnc)   

# Create ROS2 container   
이미지 실행 
```
docker run -it -p 8080:80 --name ros tiryoh/ros2-desktop-vnc:foxy  
```
![image](https://github.com/younlea/company_project/assets/1435846/da61e65d-d1d2-4dee-85a3-1aa8671a3e69)   
동작 확인   
![image](https://github.com/younlea/company_project/assets/1435846/da9ce82f-ad47-4b17-b9a0-a5d0bed26b21)

browser에서 localhost:8080을 입력함   
![image](https://github.com/younlea/company_project/assets/1435846/665ab253-e9eb-4328-9ee2-093e1119163d)
연결을 누르면 UI가 나오는걸 확인할수 있다.        
![image](https://github.com/younlea/company_project/assets/1435846/533c86be-9f51-4a64-89f4-eba298f6e79b)


# turtlesim demo    
``` 
ros2 run turtlesim turtlesim_node
ros2 run turtlesim turtle_teleop_key
rqt_graph
```
![image](https://github.com/younlea/company_project/assets/1435846/9bded586-49de-4a66-90b8-9477f877e570)


# ROS sample 예제 (talker & listener)   
실제 talker와 listener를 실행시키고 노드 그래프를 볼수 있다.  
```
ros2 run demo_nodes_cpp talker
ros2 run demo_nodes_cpp listener
rqt_graph
```
![image](https://github.com/younlea/company_project/assets/1435846/fa5c00c4-83ed-4f2b-8b86-868d2bb30e84)

# ROS topic  
[ROS2 DOC](https://docs.ros.org/en/foxy/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.html)   

# Gazebo 실행   
![image](https://github.com/younlea/company_project/assets/1435846/dc8128a1-a118-4262-9121-de1def1a6031)
![image](https://github.com/younlea/company_project/assets/1435846/9e42d52e-90d4-4e77-b617-9341b3cd26d1)

# gazebo quick start   
![image](https://github.com/younlea/company_project/assets/1435846/3114fa27-da3f-4d51-839a-7a7b104ed574)
![image](https://github.com/younlea/company_project/assets/1435846/d4a77221-a17b-4a95-9f6d-eb8b6c0a89bf)
