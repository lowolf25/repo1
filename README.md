
### 📄 `README.md`

````markdown
# ROS 1 + ROS 2 Bridge with Docker (Turtlesim Example)

This repository shows how to set up a Docker-based environment to bridge communication between **ROS 1 (Noetic)** and **ROS 2 (Foxy/Humble)** using the official `ros1_bridge`. 

An example demonstrated showing how to control the `turtlesim_node` running in ROS 1 from ROS 2.

## 🚀 Prerequisites

- Docker (Windows/Linux/macOS)
- X server for GUI output (e.g., [VcXsrv](https://sourceforge.net/projects/vcxsrv/) for Windows)
- A shared Docker network

````



## 🔧 What This Does

- Run `roscore` + `turtlesim_node` in a ROS 1 (Noetic) container
- Run ROS 2 (Foxy or Humble) in a separate container
- Use `ros1_bridge` in a third container to forward topics
- Send a velocity command from ROS 2 → ROS 1


## 🐳 Docker Setup

Create a shared Docker network:

```bash
docker network create ros_net

Here’s the full content you can include in a **GitHub repository** (e.g., `ros1_ros2_bridge_turtlesim`) that **documents all the steps** for setting up a ROS 1 + ROS 2 + `ros1_bridge` system using Docker — **excluding the script**.

---

### 📁 Directory Structure

```
ros1_ros2_bridge_turtlesim/
├── README.md
├── docker/
│   ├── ros1.dockerfile
│   ├── ros2.dockerfile
│   └── bridge.dockerfile
├── .dockerignore
└── docker-compose.yml (optional)
```

---



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

## 📜 Custom Script (Optional)

You can write custom Python scripts in the ROS 2 container to simulate drawing shapes or writing text using `/turtle1/cmd_vel`.

> The script for drawing a heart with the name **HAGAR** is not included here.

---

## 🧼 Cleanup

```bash
docker stop ros1_container bridge_container ros2_container
docker network rm ros_net
```

---

## ✅ Tested With:

* ROS 1: Noetic
* ROS 2: Foxy / Humble
* Docker: 24.x
* Host: Windows 10/11 with WSL2 backend

---

## 📄 License

MIT License. See [LICENSE](LICENSE).

```

---

Let me know if you'd like me to generate the Dockerfiles or a `docker-compose.yml` to fully automate this setup.
```


