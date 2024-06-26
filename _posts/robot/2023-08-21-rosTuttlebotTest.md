---
title: "ROS에서 터틀봇 시뮬레이션(슬램, 네비게이션) "
excerpt_separator: "<!--more-->"
categories:
  - robot
tags:
  - 터틀봇
  - slam
  - navi2

toc : true
toc_sticky : true
---

# 기본 셋팅    
## 도커 셋팅   
[docker setting](https://younlea.github.io/develop_env/ROS_VNC_setting/)    

## Turtlebot3/Gazebo install   
업데이트   
package install    
bash setting 및 반영    
```
sudo apt-get update && sudo apt-get upgrade -y
sudo apt install ros-foxy-turtlebot3 ros-foxy-turtlebot3-simulations
echo 'ROS_DOMAIN_ID=30 # TURTLEBOT3' >> ~/.bashrc
echo 'export TURTLEBOT3_MODEL=burger # setting to buger' >> ~/.bashrc
source ~/.bashrc
```
[터틀봇 ref](https://emanual.robotis.com/docs/en/platform/turtlebot3/simulation/)    
![image](https://github.com/younlea/younlea.github.io/assets/1435846/d21ee214-d1fe-4e2f-842d-ff84e66bae33)  

# Turtlebot3/Gazebo with SLAM   
## 1. Gazebo 시뮬레이터를 실행
```
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/4364b3f2-22fe-46b6-847e-2934cf47d14f)
![image](https://github.com/younlea/younlea.github.io/assets/1435846/414ef18f-a6d5-4474-b415-e195c58d423a)  

## 2. SLAM 노드 실행 (cartographer)    
```
ros2 launch turtlebot3_cartographer cartographer.launch.py use_sim_time:=True
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/c207822f-dda3-48e7-95cd-cc1a23f7ee39)  

[cartographer github](https://google-cartographer-ros.readthedocs.io/en/latest/)    
[cartographer docs](https://google-cartographer-ros.readthedocs.io/en/latest/)    

## 3. 로봇 이동 노드 실행
```
ros2 run turtlebot3_teleop teleop_keyboard //<<- 수동
ros2 run turtlebot3_gazebo turtlebot3_drive //<<- 자동
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/030eda6a-a764-4536-a149-a4b2bbc6b642)
![image](https://github.com/younlea/younlea.github.io/assets/1435846/f50d4fe1-45a3-49c4-9eca-fda9eb319eca)

동작할때  rqt_graph 결과    
![image](https://github.com/younlea/younlea.github.io/assets/1435846/a066998d-6e02-4c55-8d4f-a227fd64bbf2)  

## 4. 맵 저장 노드 실행
```
ros2 run nav2_map_server map_saver_cli -f ./map
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/5aff00b1-9404-4152-bb61-f6a72a49304e)

# Turtlebot3/Gazebo with navi2 (using made map)    
네비게이션 테스트   
## 1. Gazebo 시뮬레이터를 실행
```
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```
## 2. Nav2 노드 실행   
지도가 있는 폴더로 이동 후 실행
```
ros2 launch turtlebot3_navigation2 navigation2.launch.py use_sim_time:=True map:=./map.yaml
```
> 먼저 “2D Pose Estimate”로 로봇의 초기 위치를 알려준다.   
![image](https://github.com/younlea/younlea.github.io/assets/1435846/9a60750d-1fff-4a78-bd76-66c9f79ae8ad)   

> 다음에 “Navigation2 Goal”로 목표 위치를 알려주면 이동한다.   
![image](https://github.com/younlea/younlea.github.io/assets/1435846/4cd2628c-02d7-4b9b-bd82-db7c301d2057)   
이동을 잘 안하긴 한다 .ㅡㅡ;   좀더 테스트 해봐야 함.    
[참고 자료](https://navigation.ros.org/getting_started/index.html#navigating)   

### turtlebot3_simulations
[turtlebot3_simulations github](https://github.com/ROBOTIS-GIT/turtlebot3_simulations)    
```
https://github.com/ROBOTIS-GIT/turtlebot3_simulations/blob/master/turtlebot3_gazebo/src/turtlebot3_drive.cpp

```
### gazebo_ros_pkgs
[gazebo_ros_pkgs github](https://github.com/ros-simulation/gazebo_ros_pkgs)   
```
https://github.com/ros-simulation/gazebo_ros_pkgs/blob/noetic-devel/gazebo_plugins/src/gazebo_ros_diff_drive.cpp

```

# Nav2 관련   
navigation 관련 튜닝 포인트를 찾아 내야 함.     
[web page](https://navigation.ros.org/)     
![image](https://github.com/younlea/younlea.github.io/assets/1435846/7514c3f1-2331-49ec-8a65-536b0d669b00)   
[github](https://github.com/ros-planning/navigation2/)     

## navigation concepts   
[web page](https://navigation.ros.org/concepts/index.html)    

## nav2 plugin    
[web page](https://navigation.ros.org/plugin_tutorials/)    
[web page](https://navigation.ros.org/plugins/)     

## nav2 BT(behavior trees)   
[web page](https://navigation.ros.org/behavior_trees/index.html)   
[behaviortree](https://www.behaviortree.dev/)    

## simple commander_api 
[web page](https://navigation.ros.org/commander_api/index.html)   
