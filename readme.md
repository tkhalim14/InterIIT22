# Installation 
## ROS
You can find these installation instructions [here](http://wiki.ros.org/melodic/Installation/Ubuntu).
#### Setup your sources.list
```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```
#### Set up your keys
```
sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
```
#### Update packages and install ROS
```
sudo apt update
sudo apt install -y ros-melodic-desktop-full
```
#### Setup the environment
```
echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```
#### Dependencies
```
sudo apt install -y python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential
```
#### Rosdep
```
sudo apt install -y python-rosdep
sudo rosdep init
rosdep update
```

## Ardupilot
### Installing Ardupilot and MAVProxy
#### Clone ArduPilot
In home directory:
```
cd ~
sudo apt install git
git clone https://github.com/ArduPilot/ardupilot.git
cd ardupilot
git checkout Copter-3.6
git submodule update --init --recursive
```

#### Install dependencies:
```
sudo apt install python-matplotlib python-serial python-wxgtk3.0 python-wxtools python-lxml python-scipy python-opencv ccache gawk python-pip python-pexpect
```

#### Use pip (Python package installer) to install mavproxy:
```
sudo pip install future pymavlink MAVProxy
```

Open `~/.bashrc` for editing:
```
gedit ~/.bashrc
```

Add these lines to end of `~/.bashrc` (the file open in the text editor):
```
export PATH=$PATH:$HOME/ardupilot/Tools/autotest
export PATH=/usr/lib/ccache:$PATH
```

Save and close the text editor.

Reload `~/.bashrc`:
```
. ~/.bashrc
```

Run SITL (Software In The Loop) once to set params:
```
cd ~/ardupilot/ArduCopter
sim_vehicle.py -w
```
## Gazebo and Plugins
#### Gazebo

Setup your computer to accept software from http://packages.osrfoundation.org:
```
sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'
```

Setup keys:
```
wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -
```

Reload software list:
```
sudo apt update
```
Install Gazebo:
```
sudo apt install gazebo9 libgazebo9-dev
```
### Install Gazebo plugin for APM (ArduPilot Master) :
```
cd ~
git clone https://github.com/khancyr/ardupilot_gazebo.git
cd ardupilot_gazebo
git checkout dev
```
build and install plugin
```
mkdir build
cd build
cmake ..
make -j4
sudo make install
```
```
echo 'source /usr/share/gazebo/setup.sh' >> ~/.bashrc
```
Set paths for models:
```
echo 'export GAZEBO_MODEL_PATH=~/ardupilot_gazebo/models' >> ~/.bashrc
. ~/.bashrc
```

## Gazebo models
```
cd ~/.gazebo/
git clone https://github.com/Avi241/models
cd models 
sudo rm -r .git
```
#### Run Simulator
In one Terminal (Terminal 1), run Gazebo:
```
gazebo --verbose ~/ardupilot_gazebo/worlds/iris_arducopter_runway.world
```

In another Terminal (Terminal 2), run SITL:
```
cd ~/ardupilot/ArduCopter/
sim_vehicle.py -v ArduCopter -f gazebo-iris --console

```
## If you face gazebo error then Solving Gazebo Error

Open ~/.ignition/fuel/config.yaml, replace

api.ignitionfuel.org
with
fuel.ignitionrobotics.org

## Creating Catkin Workspace

mkdir catkin_ws
cd catkin_ws
mkdir -p src
cd src
catkin_init_workspace
echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
cd ~/catkin_ws
catkin_make
source ~/catkin_ws/devel/setup.bash

### Compiling package they provided 
## Download the zip file in your Downloads folder 
## Open terminal

```
cd ~/Downloads
unzip InterIIT_DRDO.zip
cp -r prius_msgs ~/catkin_ws/src/
cd ~/catkin_ws
catkin_make 

cd ~/catkin_ws/src/
sudo rm -r prius_msgs
cd  ~/Downloads
cp -r prius_description interiit22 ~/catkin_ws/src/
cd ~/catkin_ws
catkin_make

``` 

## To run the world

```
roslaunch interiit22 drdo_world1.launch

```



