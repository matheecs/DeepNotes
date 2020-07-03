# RVIZ

内置显示组件
- Axes
- Effort
  - `sensor_msgs/JointState`
- Camera
  - `sensor_msgs/Image`
  - `sensor_msgs/CameraInfo`
- Grid
  - `nav_msgs/GridCells`
- Grid Cells
- Image
- InteractiveMarker
  - `visualization_msgs/InteractiveMarker`
- Laser Scan
  - `sensor_msgs/LaserScan`
- Map
  - `nav_msgs/OccupancyGrid`
- Markers
  - `visualization_msgs/Marker`, `visualization_msgs/MarkerArray`
- Path
  - `nav_msgs/Path`
- Point
  - `geometry_msgs/PointStamped`
- Pose
  - `geometry_msgs/PoseStamped`
- Pose Array
- Point Cloud(2)
  - `sensor_msgs/PointCloud`, `sensor_msgs/PointCloud2`
- Polygon
  - `geometry_msgs/Polygon`
- Odometry
  - nav_msgs/Odometry
- Range
  - sensor_msgs/Range
- RobotModel
  - URDF?
- TF
- Wrench
  - geometry_msgs/WrenchStamped

Views 类型
- Orbital Camera
- FPS (first-person) Camera
- Top-down Orthographic
- XY Orbit
- Third Person Follower

坐标系
- **固定坐标系（Fixed Frame）**
  - 世界坐标系
  - `map`
  - `world`
- 目标坐标系（Target Frame）
  - > The reference frame for the camera view.
  - 传感器的视角？

时间
- ROS Time
- Wall Time
- 注：时间信息在**仿真**时很重要！

插件
- 定制插件


[rviz_visual_tools](https://github.com/PickNikRobotics/rviz_visual_tools)