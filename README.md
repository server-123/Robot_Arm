# Robot_Arm
**Reference**: https://zeta7.notion.site/zeta7/JessiArm-be431f54912b472fb7f8977e5499612d
## ROS(Robot Operating System)
ROS is an open source robot operating system for robot operation.  
It provides a wealth of tools and libraries for developing and running various robots and robot applications.  
  
**Modularity and transparency**: ROS is modular, consisting of multiple nodes, and nodes can communicate transparently with each other.  
**Message communication**: Nodes communicate using message types defined in ROS. This makes it easier to exchange data.  
**Package Systems**: ROS uses a package system to organize and share code, compiled executable files, data, libraries, and more.  
**Simulation and visualization**: The abundance of tools to simulate and visualize robots makes development and testing easier.  
  
**Topic**: "Topic" is the mechanisms by which messages are exchanged between nodes. "Publish" a message about a particular topic, and another node "subscribes" to that topic to receive the message. Topics support asynchronous communication, and nodes perform specific functions by subscribing only to topics they are interested in.  
**Service**: "Service" is a synchronous communication mechanism consisting of "request" and "response". When a client node requests a service, the server node responds. The service is usually used when one-off work is required.  
**Action**: Similar to the "service", but used for long-running tasks. Clients send goals, and servers send interim results periodically until the task is complete.  
  
**rqt_graph**: As one of the GUI tools provided by ROS, it is a tool that visually displays graphs between nodes and topics in the currently running ROS system. This makes it easy to understand and debug the ROS system's "topic subscription" and "publication" relationships, and connections between nodes.
## YOLO(You Only Look Once)

## Darknet_ros
Darknet_ros is a ROS package for integrating the Darknet framework with the ROS.  
This package uses Darknet to perform real-time object detection, publishing the results as ROS messages, and communicating with other ROS nodes.  
  
**Real-time Object Detection**: Use the "YOLO" algorithm to detect multiple objects in real time.  
**Integration with ROS**: Convert Darknet results to ROS messages and communicate with other ROS nodes through ROS topics.  
**Set parameters**: The user can set the desired object detection behavior by adjusting the parameters in darknet_ros.  
**Visualization**: Supports RViz messages for visualizing results.
## Hardware (A Circuit Diagram)
![image](https://github.com/server-123/Robot_Arm/assets/73692229/263003c9-ebbb-4816-bfb2-d8b9b06ef14e)
Connect four servo motors.
## Software
### Jetpack & ROS install
**Reference**: https://github.com/server-123/AI_RC, **"Jetpack & ROS install"**
### Jessiarm install
```
cd ~/catkin_ws/src
git clone https://github.com/zeta0707/jessiarm.git

# The patch for the "/opt/ros/melodic/share/cv_bridge/cmake/cv_bridgeConfig.cmake" file is read from the "cv_bridge.patch" file and applied.
cd ~/Downloads/opencvDownTo34
sudo patch -p1 /opt/ros/melodic/share/cv_bridge/cmake/cv_bridgeConfig.cmake -p1 < cv_brige.patch
cd ~/catkin_ws

cma
```
### Test Servo Motor
```
sudo apt install python-pip -y
pip2 install adafruit_pca9685

cd ~/catkin_ws
python src/jessiarm/jessiarm_control/src/servokit_test.py
```
![image](https://github.com/server-123/Robot_Arm/assets/73692229/7f9341c7-48f3-4a6e-8324-f79089081031)
### Control by Keyboard
```
cd ~/catkin_ws
git clone https://github.com/ros-teleop/teleop_twist_keyboard.git
cma
```

```
roslaunch jessiarm_control teleop_keyboard.launch
```
j l : Robot Arm Left, Right  
i , : motor1  
o . : motor2  
u m : motor3  
t b : gripper close, open

```
rqt_graph
```
![image](https://github.com/server-123/Robot_Arm/assets/73692229/8c05987b-0f0b-4cc8-be66-ac7518fdfded)
### Automatic Move
Copy the **"automove.txt"** file.
```
cp ~/.ros/automove.txt ~/catkin_ws/src/jessiarm/jessiarm_control/src
cp ~/.ros/automove.txt ~/catkin_ws
```
Run the **"auto_move.py"** file.
```
cd catkin_ws
python src/jessiarm/jessiarm_control/src/auto_move.py
```
According to "automove.txt" file, the robot arm moves.
After it sleeps according to the variable written at the end, it excutes "while loop"
### Verify USB camera
```
ls /dev/video0*
nvgstcapture-1.0 --camsrc=0 --cap-dev-node=/dev/video0
```
### Blob Pick and Place
```
roslaunch jessiarm_control blob_control.launch
```
The content of **"find_ball.yaml"** file
```
# Predefined filter values
define: &blue_min [55,40,0]
define: &blue_max [150, 255, 255]
define: &white_min [16, 0, 66]
define: &white_max [133, 251, 255]
define: &pink_min [135, 41, 95]
define: &pink_max [255, 196, 255]
define: &green_min [39, 81, 71]
define: &green_max [75, 255, 255]
define: &orange_min [7, 109, 50]
define: &orange_max [76, 218, 234]
define: &red_min [0, 94, 92]
define: &red_max [255, 255, 255]

# Chose the filter.
blob_detector:
  blob_min: *green_min
  blob_max: *green_max
```
```
rqt_graph
```
![image](https://github.com/server-123/Robot_Arm/assets/73692229/0505243d-d113-4c45-ba93-5062990fb308)
### Yolo4 Pick and Place
