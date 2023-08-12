# YOLOv5 ROS
本仓库fork自[yolov5_ros](https://github.com/mats-robotics/yolov5_ros)，添加了对V4L2摄像头和Intel Realsense D435设备的支持。

## 安装

### 依赖
本仓库编译和测试的环境如下：

- Jetson Orin NX 8GB
- Ubuntu 20.04 LTS
- ROS Noetic
- Python 3.8

其他设备可能在V4L2调用上存在问题，请自行修改[这行代码](https://github.com/s235784/yolov5_ros/blob/eda0637b6c039e7c36ec3de516ee98063f43e30b/src/detect_v4l2.py#L111)。

1. Clone本仓库至本地的ROS工作空间中，并安装yolov5的依赖：

```bash
cd <ros_workspace>/src
git clone https://github.com/s235784/yolov5_ros.git
git clone --recurse-submodules https://github.com/mats-robotics/yolov5_ros.git 
cd yolov5_ros/src/yolov5
pip install -r requirements.txt # install the requirements for yolov5
```
2. 编译ROS包：

```bash
cd <ros_workspace>
catkin build yolov5_ros # build the ROS package
```
3. 授予Python script权限： 

```bash
cd <ros_workspace>/src/yolov5_ros/src
chmod +x detect.py
```

## 基本用法
- 使用Topic

修改 launch/yolov5.launch中的 `input_image_topic` 至任何类型为 `sensor_msgs/Image` 或 `sensor_msgs/CompressedImage` 的话题。然后启动节点：

```bash
roslaunch yolov5_ros yolov5.launch
```

- 使用D435

首先开启D435的ROS节点：

```bash
roslaunch realsense2_camera rs_camera.launch
```

然后开启Yolo节点：

```bash
roslaunch yolov5_ros yolov5_d435.launch
```

- 使用V4L2

修改 launch/yolov5_camera.launch中的 `camera_src ` 至摄像头文件地址，如`/dev/video0`，然后启动节点：

```bash
roslaunch yolov5_ros yolov5_camera.launch
```

## 使用自定义权重和训练集

* 将 *pt* 和 *yaml* 文件放在 `yolov5_ros/models` 文件夹中
* 更改各个 launch 文件中的 `weights`,  `data`
