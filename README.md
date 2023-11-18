![image](https://github.com/server-123/Robot_Arm/assets/73692229/4dad27f3-9f03-453b-87ec-32882c69eb31)# Robot_Arm
**Reference**: https://zeta7.notion.site/zeta7/JessiArm-be431f54912b472fb7f8977e5499612d
## ROS(Robot Operating System)
ROS is an open source robot operating system for robot operation.  
It provides a wealth of tools and libraries for developing and running various robots and robot applications.  
  
**Modularity and transparency**: It is modular, consisting of multiple nodes, and nodes can communicate transparently with each other.  
**Message communication**: Nodes communicate using message types defined in ROS. This makes it easier to exchange data.  
**Package Systems**: It uses a package system to organize and share code, compiled executable files, data, libraries, and more.  
**Simulation and visualization**: The abundance of tools to simulate and visualize robots makes development and testing easier.  
  
**Topic**: "Topic" is the mechanisms by which messages are exchanged between nodes. "Publish" a message about a particular topic, and another node "subscribes" to that topic to receive the message. Topics support asynchronous communication, and nodes perform specific functions by subscribing only to topics they are interested in.  
**Service**: "Service" is a synchronous communication mechanism consisting of "request" and "response". When a client node requests a service, the server node responds. The service is usually used when one-off work is required.  
**Action**: Similar to the "service", but used for long-running tasks. Clients send goals, and servers send interim results periodically until the task is complete.  
  
**rqt_graph**: As one of the GUI tools provided by ROS, it is a tool that visually displays graphs between nodes and topics in the currently running ROS system. This makes it easy to understand and debug the ROS system's "topic subscription" and "publication" relationships, and connections between nodes.
## YOLO(You Only Look Once)
YOLO is deep learning algorithm for Object Detection.  
It divides images into several grids with only one forward pass and predicts bounding boxes and class probabilities for objects in each grid.  
It is effectively used for real-time object detection.  
  
**Real-time processing**: It can perform object detection with only one forward pass, so it can be executed in real time.  
**Grid Systems**: It divides images into grids and predict which objects each grid cell belongs to.  
**Various Object Detection**: You can detect bounding boxes and classes for multiple objects in one image.  
**Minimize duplicate detection of objects**: When an object spans multiple grid cells, It creates only one bounding box to minimize duplicate detection.
## Darknet_ros
Darknet_ros is a ROS package for integrating the Darknet framework with the ROS.  
This package uses Darknet to perform real-time object detection, publishing the results as ROS messages, and communicating with other ROS nodes.  
  
**Real-time Object Detection**: It uses the "YOLO" algorithm to detect multiple objects in real time.  
**Integration with ROS**: It converts Darknet results to ROS messages and communicate with other ROS nodes through ROS topics.  
**Set parameters**: The user can set the desired object detection behavior by adjusting the parameters in darknet_ros.  
**Visualization**: It supports RViz(Robot Visualization) messages for visualizing results.

*RViz: It is used to visually represent robot and sensor data.
# Hardware (A Circuit Diagram)
![image](https://github.com/server-123/Robot_Arm/assets/73692229/263003c9-ebbb-4816-bfb2-d8b9b06ef14e)
Connect four servo motors.
# Software
## Jetpack & ROS install
**Reference**: https://github.com/server-123/AI_RC, **"Jetpack & ROS install"**
## Jessiarm install
```
cd ~/catkin_ws/src
git clone https://github.com/zeta0707/jessiarm.git

# The patch for the "/opt/ros/melodic/share/cv_bridge/cmake/cv_bridgeConfig.cmake" file is read from the "cv_bridge.patch" file and applied.
cd ~/Downloads/opencvDownTo34
sudo patch -p1 /opt/ros/melodic/share/cv_bridge/cmake/cv_bridgeConfig.cmake -p1 < cv_brige.patch
cd ~/catkin_ws

cma
```
## Test Servo Motor
```
sudo apt install python-pip -y
pip2 install adafruit_pca9685

cd ~/catkin_ws
python src/jessiarm/jessiarm_control/src/servokit_test.py
```
![image](https://github.com/server-123/Robot_Arm/assets/73692229/7f9341c7-48f3-4a6e-8324-f79089081031)
**PWM(Pulse Width Modulation)**: The PWM signal represents the percentage of time the signal is in the ON state within the period (T). This signal allows you to generate a signal that corresponds to the analog value you want to represent on average.  
  
**Power(Positive)**: A line for power supply  
**Ground(Negative)**: A line indicating the grounding of the power supply  
**Control**: A line used to control the angle of the servo motor  
  
The servo motor rotates in response to the PWM signal received from the control line.  
The pulse width of the PWM signal (when the waveform is high) controls the movement of the servo motor.  
Typically, the greater the pulse width, the greater the angle the servo motor rotates.  
It usually uses a PWM signal with a period of 20 ms.
If the pulse width is 1 ms, the servo motor rotates at the minimum angle, and if the pulse width is 2 ms, it rotates at the maximum angle.

*Since the range of pulse width used to control the operation of the servo motor is determined according to a specific standard, a pulse width of 1 ms to 2 ms is used.
## Control by Keyboard
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
## Automatic Move
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
According to the **"automove.txt"** file, the robot arm moves.
After it sleeps according to the variable written at the end, it excutes "while loop"
## Verify USB camera
```
ls /dev/video0*
nvgstcapture-1.0 --camsrc=0 --cap-dev-node=/dev/video0
```
## Blob Pick and Place
```
roslaunch jessiarm_control blob_control.launch
```
The content of the **"find_ball.yaml"** file
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
## Yolo4 Pick and Place
### Install Darknet_ros
```
sudo apt-get install -y ros-melodic-image-pipeline

cd ~/catkin_ws/src
git clone --recursive https://github.com/Tossy0423/yolov4-for-darknet_ros.git
```
### Darknet_ros Modification
Modify "yolov4" to "yolov4-tiny" and apply "config" and "weights" corresponding to "custom dataset".
```
cd ~/catkin_ws/src/yolov4-for-darknet_ros/darknet_ros
git clone https://github.com/zeta0707/darknet_ros_custom.git
cp -rf darknet_ros_custom/* darknet_ros/
```
```
cd ~/catkin_ws
cma
```
### Run the Darknet_ros
It consists of two **"launch"** files.  
**Operation Order**
1. camera publish
2. yolo running
3. object x, y → robot move
```
# terminal 1, object detect using Yolo_v4
roslaunch darknet_ros yolo_v4.launch

# terminal 2, camera publish, object x/y -> robot move
roslaunch jessiarm_control yolo_chase.launch
```
The **"yolo_chase.launch"** file reads the Variables in the **"yolo_jessiarm.yaml"** file.
  
Modify the topic to subscribe to to use the USB camera.
```
camera_reading:
  topic: /webcam_image
```
![image](https://github.com/server-123/Robot_Arm/assets/73692229/4b945d30-3e46-4bac-bb43-cbaeccdd1c03)
```
rqt_image_view
```
![image](https://github.com/server-123/Robot_Arm/assets/73692229/7343795d-d352-4c9a-9b4c-b6275b546f4c)
```
rqt_graph
```
![image](https://github.com/server-123/Robot_Arm/assets/73692229/360af69f-807c-4b7f-aeb8-45f3ce0f7e90)
