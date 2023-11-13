# Robot_Arm
Reference : https://zeta7.notion.site/zeta7/JessiArm-be431f54912b472fb7f8977e5499612d
## Hardware (A Circuit Diagram)
![image](https://github.com/server-123/Robot_Arm/assets/73692229/263003c9-ebbb-4816-bfb2-d8b9b06ef14e)
Connect four servo motors.
## Software
### Jetpack & ROS install
Reference : https://github.com/server-123/AI_RC, "Jetpack & ROS install"
### Jessiarm install
```
cd ~/catkin_ws/src
git clone https://github.com/zeta0707/jessiarm.git

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
j l: Robot Arm Left, Right  
i , : motor1  
o . : motor2  
u m : motor3  
t b: gripper close, open

```
rqt_graph
```
![image](https://github.com/server-123/Robot_Arm/assets/73692229/8c05987b-0f0b-4cc8-be66-ac7518fdfded)
