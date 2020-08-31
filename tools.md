# Tools

数据曲线绘图可视化工具：
- PlotJuggler
  - 支持订阅 [[ROS.md]] 消息绘制
  - 支持 CSV、rosbags……


系统资源可视化监控
- htop

删除 `._*` 开头的所有文件，或者 .DS_Store 文件夹
```bash
$ find . -type f -name '._*' -delete
$ find . -name '.DS_Store' -type f -delete
```

macOS 禁止 `.DS_store` 生成:
```bash
$ defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool TRUE
```
使能：
```bash
$ defaults delete com.apple.desktopservices DSDontWriteNetworkStores
```

**Doxygen 生成文档**
```bash
RECURSIVE              = YES
```

**C++调试工具：backward-cpp**