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
