---
tags:
  - github
date: 2025/12/14
---
## 步骤
### 1.进入指定目录
- cd xxx;
- ls xxx;
### 2.克隆相关仓库
- git clone xxx
- cd xxx
### 3.建立虚拟环境
- python -m venv venv
- venv\Scripts\activate
- pip install -r requirements.txt
### 4.运行程序
- python main.py
### 5.建立以后的快捷方式
- 进入和main.py同级的目录
- 新建bat文件
- 输入：
@echo off
cd /d 项目目录
call venv\Scripts\activate
python main.py
pause