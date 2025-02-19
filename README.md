
![d456_hp_925px](https://github.com/user-attachments/assets/ff672fdc-ad5a-4ef3-b1fb-08998e45143b)


HOW TO INSTALL REALSENSE SDK 2.0 WITH RK3588>>

NOTE: Ive used ubuntu-rockchip 22.04 from joshua riek with a rock5A sbc.



Make Ubuntu up-to-date including the latest stable kernel:

    sudo apt-get update && sudo apt-get upgrade && sudo apt-get dist-upgrade

Install the core packages required to build librealsense binaries and the affected kernel modules:

		sudo apt-get install libssl-dev libusb-1.0-0-dev libudev-dev pkg-config libgtk-3-dev v4l-utils

Cmake Note: certain librealsense CMAKE flags (e.g. CUDA) require version 3.8+ which is currently not made available via apt manager for Ubuntu LTS.
Install build tools

		sudo apt-get install git wget cmake build-essential

Prepare Linux Backend and the Dev. Environment
Unplug any connected Intel RealSense camera and run:

		sudo apt-get install libglfw3-dev libgl1-mesa-dev libglu1-mesa-dev at


Note:

on graphic sub-system utilization:
glfw3, mesa and gtk packages are required if you plan to build the SDK's OpenGL-enabled examples.
The librealsens2e core library and a range of demos/tools are designed for headless environment deployment.

libudev-dev installation is optional but recommended,
when the libudev-dev is installed the SDK will use an event-driven approach for triggering USB detection and enumeration,
if not the SDK will use a timer polling approach which is less sensitive for device detection.


Clone/Download the latest stable version of librealsense2 in one of the following ways:

Clone the librealsense2 repo

    git clone https://github.com/IntelRealSense/librealsense.git

    




Run Intel Realsense permissions script from librealsense2 root directory:

	cd librealsense		
  	./scripts/setup_udev_rules.sh

Notice: You can always remove permissions by running: ./scripts/setup_udev_rules.sh --uninstall



Navigate to librealsense2 root directory and run:

		mkdir build && cd build

Run cmake configure step, here are some cmake configure examples:
The default build is set to produce the core shared object and unit-tests binaries in Debug mode.

		cmake ../ -DBUILD_EXAMPLES=true -DFORCE_RSUSB_BACKEND=true -DCMAKE_BUILD_TYPE=release -DBUILD_GRAPHICAL_EXAMPLES=TRUE


Recompile and install librealsense2 binaries:

		sudo make uninstall && make clean && make && make -j$(($(nproc)-1)) && sudo make install

		sudo reboot



NOTE:
My Realsense Camera IMU only works with FW Version 5.13.0.50
If you have problems with IMU, try to downgrade your camera



ros2 wrapper


Option 2: Install from source

    Create a ROS2 workspace

    mkdir -p ~/ros2_ws/src
    cd ~/ros2_ws/src/

Clone the latest ROS2 Intel® RealSense™ wrapper from here into '~/ros2_ws/src/'

git clone https://github.com/IntelRealSense/realsense-ros.git -b ros2-master
cd ~/ros2_ws

    Install dependencies

sudo apt-get install python3-rosdep -y
sudo rosdep init # "sudo rosdep init --include-eol-distros" for Foxy and earlier
rosdep update # "sudo rosdep update --include-eol-distros" for Foxy and earlier
rosdep install -i --from-path src --rosdistro $ROS_DISTRO --skip-keys=librealsense2 -y

    Build

colcon build

    Source environment

ROS_DISTRO=<YOUR_SYSTEM_ROS_DISTRO>  # set your ROS_DISTRO: iron, humble, foxy
source /opt/ros/$ROS_DISTRO/setup.bash
cd ~/ros2_ws
. install/local_setup.bash







