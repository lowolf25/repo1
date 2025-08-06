# ROS1-ROS2 Turtlesim Bridge Demo 🐢🤖

This project demonstrates how to control a **ROS 1 turtlesim node** using **ROS 2 commands** via the `ros1_bridge`, all running in **Docker**.

## 🔧 What This Does

- Run `roscore` + `turtlesim_node` in a ROS 1 (Noetic) container
- Run ROS 2 (Foxy or Humble) in a separate container
- Use `ros1_bridge` in a third container to forward topics
- Send a velocity command from ROS 2 → ROS 1


## 🐳 Docker Setup

Create a shared Docker network:

```bash
docker network create ros_net

