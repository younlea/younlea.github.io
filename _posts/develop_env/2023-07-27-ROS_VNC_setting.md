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

[ROS2 doc](https://docs.ros.org/en/foxy/index.html)     

# 1. docker 설치
[docker](https://www.docker.com/)   
![image](https://github.com/younlea/younlea.github.io/assets/1435846/a9f859b3-bd95-45a3-84d2-07d9aa5760ed)


Docker Desktop Installer.exe 파일 실행해서 설치 (설치 완료후 로그 아웃 누르면 한번 나갔다 옴)    

Docker 실행   
![image](https://github.com/younlea/younlea.github.io/assets/1435846/39f21be3-b4f3-441e-9d93-e5207c9e7916)

Docker sign up or sign in 
![image](https://github.com/younlea/younlea.github.io/assets/1435846/02c75587-0141-4f18-b99c-86a9f53325d3)

실행 결과  
![image](https://github.com/younlea/younlea.github.io/assets/1435846/5c0c0983-9615-42ec-85f4-93ba3820ff9f)


# 2. Pull ROS2 VNC image  
docker 이미지를 가져 온다.   
```
docker pull tiryoh/ros2-desktop-vnc:foxy  
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/6d02ed6d-6cbc-4a69-9f30-85b056945668)

다 받으면 아래 처럼 나온다. 
![image](https://github.com/younlea/younlea.github.io/assets/1435846/b202b476-74ea-4e67-ac60-511cf948128c)

[github](https://github.com/Tiryoh/docker-ros2-desktop-vnc)    
[docker hub](https://hub.docker.com/r/tiryoh/ros2-desktop-vnc)   

# 3. Create ROS2 container   
이미지 실행 
```
docker run -it -p 8080:80 --name ros tiryoh/ros2-desktop-vnc:foxy  
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/655293e1-eb85-448e-a1cb-85497b0bb907)

동작 확인   
![image](https://github.com/younlea/younlea.github.io/assets/1435846/60471729-b00b-487c-8afd-99f41b599533)


browser에서 localhost:8080을 입력함   
![image](https://github.com/younlea/younlea.github.io/assets/1435846/964c128c-aa61-470e-8bed-f3089afff67f)

연결을 누르면 UI가 나오는걸 확인할수 있다.        
![image](https://github.com/younlea/younlea.github.io/assets/1435846/33e8bc3d-ed83-4147-8717-36a5a746209a)



# 4. turtlesim demo 실행      
``` 
ros2 run turtlesim turtlesim_node
ros2 run turtlesim turtle_teleop_key
rqt_graph
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/087a13a9-3296-47d0-bc2c-00df24a59c8d)



# 5. ROS sample 예제 (talker & listener)   
실제 talker와 listener를 실행시키고 노드 그래프를 볼수 있다.  
```
ros2 run demo_nodes_cpp talker
ros2 run demo_nodes_cpp listener
rqt_graph
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/6a8a7259-91ab-4d5f-b752-18cd7b4578fa)


# 6. ROS topic  
[ROS2 DOC](https://docs.ros.org/en/foxy/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.html)   

# 7. Gazebo 실행   
[gazebo](https://gazebosim.org/home)    
[gazebo to ROS2 sample guide](https://docs.ros.org/en/foxy/Tutorials/Advanced/Simulators/Ignition/Ignition.html?highlight=gazebo)    

![image](https://github.com/younlea/younlea.github.io/assets/1435846/65438323-1c6f-495a-b37f-9f7ccbbd10b4)

## 7-1.gazebo quick start   
![image](https://github.com/younlea/younlea.github.io/assets/1435846/fac651c9-0b8a-44c9-94f1-5e57d25298c5)
![image](https://github.com/younlea/younlea.github.io/assets/1435846/0fbb7a99-caa9-4fb4-b741-f7b0800cd04f)

