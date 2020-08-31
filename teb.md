# TEB

[使用入门](http://wiki.ros.org/teb_local_planner/Tutorials)

[FAQ](http://wiki.ros.org/teb_local_planner/Tutorials/Frequently%20Asked%20Questions)
- 机器人容易靠近墙/角
  - 增加膨胀半径
  - TEB不考虑膨胀半径
- 执行全局规划的路径
  - 默认是时间最短
  - 降低 `max_global_plan_lookahead_dist`，但是降低了避障能力，不推荐
  - 推荐 [Following the Global Plan (Via-Points)](http://wiki.ros.org/teb_local_planner/Tutorials/Following%20the%20Global%20Plan%20%28Via-Points%29)

[ROS中的这些局部导航算法你用过哪些](https://www.guyuehome.com/5500)

- `feasibility_no_of_poses`
- `min_obstacle_dist`
- `inflation_dist`
  - `min_obstacle_dist` < `inflation_dist`
  - 满足条件 width_of_robot + 2*min_obstacle_dist < corridor width

[Teb prefers forward motion](https://github.com/rst-tu-dortmund/teb_local_planner/issues/137):
> In the first case, you placed the goal inside the local costmap and if `allow_init_with_backwards_motion = true`, the optimizer is likely to find the backward motion directly. However, for goals outside the local costmap, intermediate goal poses are obtained from the global plan either close to the costmap border or as soon as `max_global_plan_lookahead_dist` is exceeded.

> In general, we need to assume that the global planner already provides knowledge on intermediate robot orientations. Just imagine a car park, in which parking lots close to the walls can only be accessed by backward motions. However, guessing this from just the local costmap which does not reveal the existence/shape of the parking lot at first is often impossible. However, since not all global planners provide information on the orientation, `global_plan_overwrite_orientation=true` is just a workaround to guess the orientation. And the most likely guess is to drive forward if not any further information like path lengths, global costmap etc. is considered.
> [In case the goal is inside the local costmap it should work out of the box. Otherwise, it is up to the global planner how **intermediate orientations** are chosen.](http://wiki.ros.org/teb_local_planner/Tutorials/Frequently%20Asked%20Questions#Following_global_plan_backwards)


[TEB turns around multiple times while going towards goal](https://github.com/rst-tu-dortmund/teb_local_planner/issues/115)
> The global planner does not provide any information on the orientation by default. So the planner specifies some heuristics and usually **assumes forward motions** (see the link). This could explain your first scenario. However, in the second scenario, the optimizer might choose this solution as time-optimal because the backward speed is quite large (actually similar to the forward speed). Since your linear acceleration is large as well, switching directions could be faster than steering around the corner with smaller angular velocity.

全局规划(global_planner, ~~navfn~~)方位（**Orientation filter**）输出的模式：
- [Global planner ROS - Orientation filter](https://www.youtube.com/watch?v=NQ2z90i5V2I&feature=youtu.be)
- 动态参数：`~orientation_mode`:
  - None=0 (No orientations added except goal orientation)
  - Forward=1 (Positive x axis points along path, except for the goal orientation)
    - 调整方向后，直行过去
  - Interpolate=2 (Orientations are a linear blend of start and goal pose)
  - ForwardThenInterpolate=3 (Forward orientation until last straightaway, then a linear blend until the goal pose)
  - Backward=4 (Negative x axis points along the path, except for the goal orientation)
  - Leftward=5 (Positive y axis points along the path, except for the goal orientation)
  - Rightward=6 (Negative y axis points along the path, except for the goal orientation)


TEB 全部参数：
- **机器人配置参数**
  - acc_lim_x：最大位移加速度
  - acc_lim_theta：最大角加速度
  - **max_vel_x**
  - **max_vel_x_backwards**
  - **max_vel_theta**：最大角速度
  - 仅用于阿克曼小车的参数
    - min_turning_radius
    - wheelbase
    - cmd_angle_instead_rotvel
  - 仅用于全向机器人的参数
    - max_vel_y：最大侧向速度
    - acc_lim_y：最大侧向加速度
  - footprint_model
    - type
    - radius：仅用于“圆”类型
    - line_start：仅用于“线”类型
    - line_end：仅用于“线”类型
    - front_offset：仅用于“two_circles”类型
    - front_radius：仅用于“two_circles”类型
    - rear_offset：仅用于“two_circles”类型
    - rear_radius：仅用于“two_circles”类型
    - vertices：：仅用于“polygon”类型
  - is_footprint_dynamic
- **目标阈值参数**
  - xy_goal_tolerance
  - yaw_goal_tolerance
  - free_goal_vel
- **轨迹参数**
  - dt_ref：轨迹的时间分辨率
  - dt_hysteresis：时间调整区间，最终时间分辨率的范围调整为 `dt_ref` +- `dt_hysteresis`
  - min_samples：最小采样数
  - global_plan_overwrite_orientation：考虑局部代价地图外的Goal，局部规划结果的orientation用全局规划代替
  - global_plan_viapoint_sep：全局路径跟踪模式
  - max_global_plan_lookahead_dist：每次优化时需考虑的全局规划片段
  - force_reinit_new_goal_dist：？？？
  - feasibility_check_no_poses：预测位姿的数量
  - publish_feedback：用于**调试**
  - shrink_horizon_backup：缩短时间分辨率？
  - allow_init_with_backwards_motion：在局部代价地图范围内允许直接后退达到目标点，用于能感知后方的机器人
  - exact_arc_length
  - shrink_horizon_min_duration
- **障碍物参数**
  - min_obstacle_dist：机器人离障碍物的最短距离
  - include_costmap_obstacles：引入代价地图的障碍物
  - costmap_obstacles_behind_robot_dist：只考虑此范围内的障碍物
  - obstacle_poses_affected：障碍物关联位姿的数量
  - inflation_dist (> min_obstacle_dist)：作为缓冲区
  - include_dynamic_obstacles：考虑动态障碍物（假设障碍物的速度不变）
  - legacy_obstacle_association (bool, default: false)
  - obstacle_association_force_inclusion_factor：图优化时考虑 [value] * min_obstacle_dist 范围内的障碍物
  - obstacle_association_cutoff_factor：图优化时不考虑[value] * min_obstacle_dist 范围外的障碍物
  - 使能 costmap_converter 时：
    - costmap_converter_plugin
    - costmap_converter_spin_thread
    - costmap_converter_rate：频率应小于代价地图的更新频率
- **优化参数**
  - no_inner_iterations：内层迭代次数
  - no_outer_iterations：外层迭代次数
  - penalty_epsilon
  - weight_max_vel_x
  - weight_max_vel_theta
  - weight_acc_lim_x
  - weight_acc_lim_theta
  - weight_kinematics_nh：全向机器人对应数值调小
  - weight_kinematics_forward_drive：数值调小允许后退
  - ~~weight_kinematics_turning_radius~~：只用于阿克曼结构
  - weight_optimaltime
  - weight_obstacle
  - weight_viapoint
  - weight_inflation
  - weight_adapt_factor
- **同论路径的并行规划**
  - enable_homotopy_class_planning
  - enable_multithreading
  - max_number_classes：并行规划路径时的最大条数
  - selection_cost_hysteresis：new_cost < old_cost * [value])
  - selection_obst_cost_scale
  - selection_viapoint_cost_scale
  - selection_alternative_time_cost
  - roadmap_graph_no_samples
  - roadmap_graph_area_width
  - h_signature_prescaler
  - h_signature_threshold
  - obstacle_heading_threshold
  - visualize_hc_graph
  - viapoints_all_candidates
  - switching_blocking_period
- **相关参数**
  - odom_topic
  - map_frame：全局规划的坐标系


源码分析：

- include/teb_local_planner/g2o_types：图优化的节点、边
- include/teb_local_planner/teb_config.h：参数配置
  - src/teb_config.cpp
- include/teb_local_planner/optimal_planner.h
  - src/optimal_planner.cpp