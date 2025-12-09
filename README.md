# MutiFingerHandControl - ROS2 Version

[![License](https://img.shields.io/badge/License-Apache%202.0-green.svg)](https://opensource.org/licenses/Apache-2.0)
[![Platform](https://img.shields.io/badge/ROS-Humble-blue.svg)](<>)
[![Status](https://img.shields.io/badge/Status-ROS2_Ready-success.svg)](<>)

多指手机器人控制系统 - ROS2 Humble版本

使用 `ROS2 Humble`、`MoveIt2` 和 `Gazebo Classic` 进行多指手的运动规划与仿真。

## 系统要求

- **操作系统**: Ubuntu 22.04 LTS
- **ROS版本**: ROS2 Humble Hawksbill
- **Python**: Python 3.10+
- **依赖**:
  - MoveIt2
  - Gazebo Classic (ros-humble-gazebo-ros-pkgs)
  - RViz2

## 安装

### 1. 安装ROS2 Humble

如果尚未安装ROS2 Humble,请参考[官方安装指南](https://docs.ros.org/en/humble/Installation.html)。

```bash
# 确保已安装ROS2 Humble
source /opt/ros/humble/setup.bash
```

### 2. 安装依赖包

```bash
sudo apt update
sudo apt install -y \
    ros-humble-moveit \
    ros-humble-gazebo-ros-pkgs \
    ros-humble-gazebo-ros2-control \
    ros-humble-ros2-control \
    ros-humble-ros2-controllers \
    ros-humble-joint-state-publisher \
    ros-humble-joint-state-publisher-gui \
    ros-humble-robot-state-publisher \
    ros-humble-xacro \
    ros-humble-rviz2
```

### 3. 克隆并编译工作空间

```bash
# 创建工作空间(如果尚未创建)
cd ~/MutiFingerHandControl

# 编译工作空间
colcon build

# Source工作空间
source install/setup.bash
```

## 使用方法

本项目提供三步递进式使用流程:

### 步骤1: RViz2可视化与关节调试

在RViz2中启动机器人可视化,使用图形界面调试关节运动:

```bash
source install/setup.bash
ros2 launch mutifingerhand_moveit_config step1_rviz.launch.py
```

**功能说明**:
- 启动RViz2显示机器人模型
- 启动Joint State Publisher GUI,可通过滑块控制所有关节
- 实时查看关节运动效果

### 步骤2: Gazebo动力学仿真

在Gazebo中进行物理仿真:

```bash
source install/setup.bash
ros2 launch mutifingerhand_moveit_config step2_gazebo.launch.py
```

**功能说明**:
- 在Gazebo Classic中加载多指手模型
- 支持物理碰撞和动力学仿真
- 可以添加外力和交互

**可选参数**:
```bash
# 不启动GUI(无头模式)
ros2 launch mutifingerhand_moveit_config step2_gazebo.launch.py gui:=false
```

### 步骤3: MoveIt2轨迹规划

使用MoveIt2进行运动规划和轨迹执行:

```bash
source install/setup.bash
ros2 launch mutifingerhand_moveit_config step3_moveit.launch.py
```

**功能说明**:
- 启动MoveIt2 move_group节点
- 在RViz2中显示MoveIt2规划插件
- 可以拖动交互式标记进行运动规划
- 支持多个规划组(finger1-5, fingertip1-5)

**可选参数**:
```bash
# 与Gazebo仿真结合使用
ros2 launch mutifingerhand_moveit_config step3_moveit.launch.py use_sim_time:=true fake_execution:=false

# 仅使用假执行(不需要Gazebo)
ros2 launch mutifingerhand_moveit_config step3_moveit.launch.py fake_execution:=true
```

## 项目结构

```
src/
├── mutifingerhand_qi_urdf/          # 机器人URDF描述
│   ├── urdf/                        # URDF文件
│   ├── meshes/                      # 3D网格模型
│   ├── config/                      # RViz配置
│   └── launch/                      # Launch文件
├── gazebocontrolsim/                # Gazebo仿真控制
│   ├── config/                      # 控制器配置
│   ├── launch/                      # Gazebo launch文件
│   └── src/                         # 控制节点源码
└── mutifingerhand_moveit_config/    # MoveIt2配置
    ├── config/                      # MoveIt配置文件
    │   ├── mutifingerhand.srdf      # 语义机器人描述
    │   ├── kinematics.yaml          # 运动学配置
    │   ├── joint_limits.yaml        # 关节限制
    │   └── ompl_planning.yaml       # OMPL规划器配置
    └── launch/                      # MoveIt launch文件
```

## 机器人配置

### 规划组 (Planning Groups)

- **finger1-5**: 5个手指,每个包含多个关节
- **fingertip1-5**: 5个指尖链接

### 预定义姿态

- **homefinger1-5**: 所有手指的初始"home"姿态(所有关节角度为0)

## 常见问题

### Q: 编译出错?
**A**: 确保已安装所有依赖包,并且ROS2 Humble环境已正确source。

### Q: RViz中看不到机器人模型?
**A**: 检查Fixed Frame是否设置为"world",并确保添加了RobotModel显示。

### Q: MoveIt规划失败?
**A**: 检查起始状态和目标状态是否在关节限制范围内,并调整运动学求解器超时时间。

### Q: Gazebo中机器人掉落?
**A**: URDF中的world-Plam关节是固定关节,应该不会掉落。检查URDF加载是否正确。

## 开发计划

- [x] ROS2 Humble基础迁移
- [x] RViz2可视化
- [x] Gazebo Classic仿真支持
- [x] MoveIt2集成
- [ ] ros2_control集成完善
- [ ] 添加力/力矩传感器
- [ ] 抓取规划示例
- [ ] Python API封装

## 联系方式

如有问题,请联系:
- **Email**: qi.shield95@foxmail.com
- **知乎专栏**: https://zhuanlan.zhihu.com/QIQI-HIT

## 许可证

本项目采用 Apache 2.0 许可证。详见 [LICENSE](LICENSE) 文件。

---

**注意**: 这是从ROS1迁移到ROS2的版本。原ROS1版本请切换到`ros1`分支查看。
