# 项目个人报告
---
**By 16340096-lianghw001, SYSU**  
**On June 27rd**  

---
## 自我总结

在本项目中，我作为客户端Android开发成员，主要负责客户端的网络访问（使用后端api）和部分页面UI以及编写部分文档（ui设计文档、状态模型文档）

在安卓端的开发过程中，遇到了很多bug和难题，但在我和kevinli36的共同努力下，这些问题都顺利解决了。

下面分项介绍我在项目完成的内容

### 分析

- 每周开会总结上周的项目目标，讨论下一周的实现目标，确保进度
- 完成项目仓库的搭建(pages/index.md)
- 完成UI设计文档

### 设计

- UI设计(https://ssad2019.github.io/pages/doc/07-01-01-Android-UI-design)

### 开发

- 学习并使用Android Studio创建项目
- 完成Network、Food、Restaurant类的实现
- 与kevinli36共同研究UI实现

### 管理

- 与网页前端积极合作,进行测试和功能实现方法
- 与kevinli36共同研究UI实现、共同解决bug

## PSP 2.1 统计

PSP2.1       | Personal Software Process Stages| Time (%) Senior Student |
------------ | ------------------------------- | ----------------------- |
**Planning** | **计划** | 5 |
Estimate  | 估计这个任务需要多少时间 | 5 |
**Development**  | **开发** |  75 |
Analysis   | 需求分析 (包括学习新技术) | 10 |
Design Spec| 生成设计文档 | 5 |
Design Review| 设计复审 | 5 |
Design|具体设计| 5 |
Coding|具体编码| 30 |
Code Review| 代码复审| 10 |
Test|测试（自我测试，修改代码，提交修改）| 10 |
**Reporting** | **报告** | 25 |
Domain Model | 状态模型 | 5 |
Database Design| UI设计文档 | 15 |

---
## 在项目相关仓库中的贡献，仅需要截图

- 文档git贡献截图  
![Document](../pic/Final-Report-lianghw001/lianghw001-01.jpg)  
- android客户端git贡献截图  
![WebServerSide](../pic/Final-Report-lianghw001/lianghw001-02.jpg)  

## 主要工作清单
* **最得意:** Network类的编写，所有的网络访问都通过这个类完成了，并且将其设置为单例类，只在网络访问时需要的属性，
座位号number等数据一直保存在这类中，不需要在不同的页面传递。
* **最有价值:** app首页的设计和实现，首页看似简单，只有一个标题和二维码扫描按钮。标题实际是生成的艺术字，二维码扫描按钮的图片非常难找，】
保持标题、按钮、背景的颜色一直也很重要。跳转的二维码扫描页面也是找到的尽可能简单的二维码扫码页面。
* **最有苦劳:** 项目仓库的创建和规划，这部分显然简单，但需要找到合适的框架并编写出合理、简介的readme方便项目其他成员遵循。


## 特别致谢

- 团队队长、安卓端负责人 kevinli36，规划了项目开发的整体方向，认真完成了各次小组会议，保证了项目的进度，使得项目可以顺利推进。
- 后端成员 mikualpha，较早地完成了后端框架的搭建以及服务器的部署，后端的开发任务变得顺利。而后端的顺利实现，有利于web和android的顺利开发。
- 网页端成员 lp-githu，在零基础并且自学的情况下按时完成了前端的开发任务，并帮助前端和android端相互沟通。
