<h1>VectorNav ROS 2 Install</h1>
A ROS2 node for VectorNav INS / GNSS devices. 

This package provides both raw and sensor_msg interfaces for the VN100, 200, & 300 devices. It has been entirely redesigned from the ROS1 package to provide a good basis to build into applications without requiring modification of the node itself. Most of the device configuration settings are exposed as ROS2 parameters that can be modified from a launch file.  

1 Source ROS 2 environment 

Your main ROS 2 installation will be your underlay for this tutorial. (Keep in mind that an underlay does not necessarily have to be the main ROS 2 installation.) Depending on how you installed ROS 2 (from source or binaries), and which platform you’re on, your exact source command will vary: 

source /opt/ros/jazzy/setup.bash 

2 Create a new directory 

The best practice is to create a new directory for every new workspace. The name doesn’t matter, but it is helpful to have it indicate the purpose of the workspace. Let’s choose the directory name ros2_ws, for “development workspace”: 

mkdir -p ~/ros2_ws/src 

cd ~/ros2_ws/src  

3 Clone a sample repo 

In the ros2_ws/src directory, run the following command: 

git clone https://github.com/dawonn/vectornav.git -b ros2 

So far you have populated your workspace with a sample package, but it isn’t a fully functional workspace yet. You need to resolve the dependencies first and then build the workspace. 

4 Resolve dependencies 

Before building the workspace, you need to resolve the package dependencies. You may have all the dependencies already, but the best practice is to check for dependencies every time you clone. You wouldn’t want a build to fail after a long wait only to realize that you have missing dependencies. 

From the root of your workspace (ros2_ws), run the following command: 

# rosdep installation 

sudo apt install python3-rosdep 

sudo rosdep init 

rosdep update 

 

# cd if you're still in the ``src`` directory 

cd .. 

rosdep install -i --from-path src --rosdistro jazzy -y 

5 Build the workspace with Colcon 

From the root of your workspace (ros2_ws), you can now build your packages using the command: 

colcon build 

The console will return the following message: 

Starting >>> vectornav 

Finished <<< vectornav 

Once the build is finished, enter the command in the workspace root (~/ros2_ws): 

ls 

And you will see that Colcon has created new directories: 

build  install  log  src 

  

6 Source the overlay 

Before sourcing the overlay, it is very important that you open a new terminal, separate from the one where you built the workspace. Sourcing an overlay in the same terminal where you built, or likewise building where an overlay is sourced, may create complex issues. In the new terminal, source your main ROS 2 environment as the “underlay”, so you can build the overlay “on top of” it: 

source /opt/ros/jazzy/setup.bash 

Go into the root of your workspace: 

cd ~/ros2_ws 

In the root, source your overlay: 

source install/local_setup.bash 

You can also add this sourcing command to your .bashrc file for automatic sourcing in future terminals. 

echo "source ~/ros2_ws/install/setup.bash" >> ~/.bashrc 

source ~/.bashrc 

7.Check Permissions 

Make sure that your user has the necessary permissions to access the /dev/ttyUSB0 port. Typically, you need to be in the dialout group to access serial ports without sudo. 

Check if your user is part of the dialout group:  

groups 

If you don't see dialout, add your user to the group with the following command: 

sudo usermod -a -G dialout $USER 

After doing this, log out and log back in (or restart your machine) to apply the changes. 

  

Now you will be able to run vectornav from the overlay 

Run with ros2 run (Option 1): 

 (Terminal 1) ros2 run vectornav vectornav 

(Terminal 2) ros2 topic echo /vectornav/raw/common 

(Terminal 3) ros2 run vectornav vn_sensor_msgs 

(Terminal 4) ros2 topic echo /vectornav/imu 

Run with ros2 launch (Option 2, uses parameters from vectornav.yaml): 

(Terminal 1) ros2 launch vectornav vectornav.launch.py 

(Terminal 2) ros2 topic echo /vectornav/raw/common 

(Terminal 3) ros2 topic echo /vectornav/imu 

  

  

  

  

 

 
