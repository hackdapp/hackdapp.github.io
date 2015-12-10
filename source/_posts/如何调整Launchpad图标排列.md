title: 如何调整Launchpad图标排列
date: 2015-10-27 13:57:10
tags:
- mac

---


### 一、打开命令窗口

### 二、设置行图标个数

        > defaults write com.apple.dock springboard-rows -int 6
        
### 三、设置列图标个数

        > defaults write com.apple.dock springboard-columns -int 7
        
### 四、重置Launchpad并重启Dock

        > defaults write com.apple.dock ResetLaunchPad -bool TRUE;
        > killall Dock