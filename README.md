# ROS 1 + ROS 2 Bridge with Docker (Turtlesim Example)

This repository shows how to set up a Docker-based environment to bridge communication between **ROS 1 (Noetic)** and **ROS 2 (Foxy/Humble)** using the official `ros1_bridge`. The goal is to control the `turtlesim_node` running in ROS 1 from ROS 2.
# ROS1-ROS2 Turtlesim Bridge Demo 🐢🤖

## 🔧 What This Does

- Run `roscore` + `turtlesim_node` in a ROS 1 (Noetic) container
- Run ROS 2 (Foxy or Humble) in a separate container
- Use `ros1_bridge` in a third container to forward topics
- Send a velocity command from ROS 2 → ROS 1


## 🐳 Docker Setup

Create a shared Docker network:

```bash
docker network create ros_net
````

---


## 🐢 Run ROS 1 Container (Noetic)

```bash
docker run -it --rm --name ros1_container --network ros_net \
  -e DISPLAY=host.docker.internal:0.0 \
  osrf/ros:noetic-desktop bash
```

Inside the container:

```bash
roscore
```

Open another terminal or shell tab:

```bash
docker exec -it ros1_container bash
rosrun turtlesim turtlesim_node
```

---

## 🧠 Run ROS 1 Bridge Container (Foxy or Humble)

```bash
docker run -it --rm --name bridge_container --network ros_net \
  osrf/ros:foxy-ros1-bridge bash
```

Inside the container:

```bash
export ROS_MASTER_URI=http://ros1_container:11311
source /opt/ros/noetic/setup.bash
source /opt/ros/foxy/setup.bash
ros2 run ros1_bridge dynamic_bridge
```

If using ROS 2 Humble, replace `foxy` with `humble`.

---

## 🛰️ Run ROS 2 Publisher

In a third terminal:

```bash
docker run -it --rm --name ros2_container --network ros_net \
  osrf/ros:foxy-desktop bash
```

Inside:

```bash
source /opt/ros/foxy/setup.bash
ros2 topic pub /turtle1/cmd_vel geometry_msgs/msg/Twist \
  '{linear: {x: 2.0}, angular: {z: 1.8}}'
```

You should see the turtle move in the ROS 1 `turtlesim` window.

---

## ✅ Tested With:

* ROS 1: Noetic
* ROS 2: Foxy / Humble
* Docker: 24.x
* Host: Windows 10/11 with WSL2 backend
