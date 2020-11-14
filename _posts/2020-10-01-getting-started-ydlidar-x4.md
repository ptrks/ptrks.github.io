---
layout: post
title: Overview of the YDLidar X4
---
![](/images/2020-10-01-getting-started-ydlidar-x4/header.jpg)

----

### Overview

The YDLidar X4 is a fantastic entry level triangulating lidar that can be had for less than $100 USD. While it's certainly lacking in a few areas, for the price point I was still thoroughly impressed. If you are wanting to add lidar to your latest project I wouldn't hesitate to pick one up.

----
### Specifications

- **Range Frequency:**  5 kHz
- **Scanning Frequency:** 6 - 12 Hz
- **Laser Range:** 0.12 to 10 m
- **Range Resolution:** < .5 mm (Range < 2.0 m) / < 1.0% distance (Range > 2 m)
- **Angle Resolution:** 0.48 - 0.52 degrees
- **Baud Rate:** Default 128000, but configurable
- **Working Temperature:** 0 - 40 C 

#### Pros 

- [Incredibly cheap!](https://camelcamelcamel.com/product/B07DBYHJVQ?context=search)
- Easy to use SDK
- ROS1 support (tested with Melodic)
- Good documentation (User Manual, Mechanical Drawings, SDK Guide, 3D Model)
- Class I laser (It won't blind you, so it is perfect for indoors)

#### Cons
- Default application is Windows only
- Indoor use only
- Lackluster range


----

### ROS1 Usage Example Using Docker

![](/images/2020-10-01-getting-started-ydlidar-x4/example.gif)


#### Prerequisites
* Assume host is running Ubuntu 18.04 (specified distro for ROS Melodic release)
* [Install docker engine](https://docs.docker.com/engine/install/ubuntu/)
* [ROS Melodic installed](http://wiki.ros.org/melodic/Installation/Ubuntu) or at least RVIZ


#### Install & Run
Clone this repository
```sh
git clone git@github.com:patrick--/ROS-YDLidar-x4-docker.git
```

If you haven't gotten the Lidar working on the host OS prior to now, go ahead and run lidar_init_env.sh to create the `udev` rules and appropriate `/dev` symlinks for the lidar
```sh
cd ROS-YDLidar-x4-docker
sudo chmod +x lidar_init_env.sh
sudo sh +x lidar_init_env.sh
```

Build image from the provided Dockerfile
```sh
cd ROS-YDLidar-x4-docker
docker build --tag ros:ros-ydlidar-x4 .
```

Launch the container on the host network and ensure to share `/dev/ydlidar`
```sh
docker run --device /dev/ydlidar --rm --network host --name ydlidar_x4_docker_test  ros:ros-ydlidar-x4
```
  **Note**: We use the default host network here for ease of use. If you need to isolate network communication between containers and or your host, you will need to [create a network](https://docs.docker.com/engine/reference/commandline/network_create/).


In another tab simply launch rviz with default settings
```sh
rosrun rviz rviz
```

Change the `Fixed Frame` drop down to `laser_frame` and add the `LaserScan` topic to start seeing lidar data in real time.


