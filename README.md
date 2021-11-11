# UE4_ClimbSystem
这个工程展示的功能和[UE4 Climb System - Tutorial](https://www.youtube.com/watch?v=BKiSTM-G9pQ)这个系列教程讲的基本完全相同。
它包含以下功能：
- **在空中时自动抓取墙壁边缘并悬挂**
- **悬挂时可以爬上墙壁**（Space键）  
![在这里插入图片描述](https://img-blog.csdnimg.cn/9bfd64ea2b4d4fc88b153d62a2038946.gif#pic_center)
- **悬挂时可以左右移动**（A/D键）  
- **悬挂时在墙壁边缘可以飞跃到相邻的墙壁**（在有墙壁的情况下，A/D键+Space键）  
![在这里插入图片描述](https://img-blog.csdnimg.cn/d36a452ff0e34ab487fe8df76066c69f.gif#pic_center)
- **悬挂时在墙壁边缘时可以左右拐弯**（A/D键）  
![在这里插入图片描述](https://img-blog.csdnimg.cn/c4884a1a58b54474a90fdbab3a804bc9.gif#pic_center)
- **悬挂时可以回头看并取消**（W/S键）  
- **回头看状态时可以退出悬挂状态或者反向跳跃**（Space键反向跳，S键退出悬挂）
![在这里插入图片描述](https://img-blog.csdnimg.cn/fe56c3b20a4c439ab4677e9c3296d90e.gif#pic_center)

本工程和教程作者提供的工程不同的地方在于：
- **重构了蓝图，简化了大量节点**。包括但不限于：
	- 简化了大量的Branch判断节点。
	- 将播放Montage的逻辑全部放到CharacterBP里面。替换原先的CharatcerBP->AnimBP的PlayMontage->AnimBP的Anim_Notify->CharacterBP的复杂流程。![在这里插入图片描述](https://img-blog.csdnimg.cn/aa8c79d98a56443aaa3dcfc205197b77.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5rC05puc5pel6bih,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)![在这里插入图片描述](https://img-blog.csdnimg.cn/c75872cb14d64339ade575e4cc646296.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5rC05puc5pel6bih,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/50eb0b7ce4f4410c862aeab039848603.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5rC05puc5pel6bih,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/b5e9793816564111bbc6c73dbc00f095.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5rC05puc5pel6bih,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)
- **悬挂时的左右移动功能改为用动画曲线来实现，表现更加自然**。原先的做法是只要按下方向键就会移动，缺点在于如果快速的按下抬起方向键，角色动画没有播放完整但是却会快速移动。  
**Before：**  
![在这里插入图片描述](https://img-blog.csdnimg.cn/865a046147844f4885ab4eaeca9a0261.gif#pic_center)  
**After：**  
![在这里插入图片描述](https://img-blog.csdnimg.cn/635964838e0043c6a58222fbf8130d11.gif#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/c8c8b362cdf9446e8e9365966cbcd042.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5rC05puc5pel6bih,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

- **更多更详细的注释。** 对于每段代码的功能、为什么这么写、蓝图使用技巧都进行了注释。
	
 ***
### 学习资料：
- [UE4 Climb System - Tutorial - Part 1 - Grab & Climb-Up](https://www.youtube.com/watch?v=BKiSTM-G9pQ)
- [UE4 Climb System - Tutorial - Part 2 - Side Movement](https://www.youtube.com/watch?v=ClplpwPGrt4)
- [UE4 Climb System - Tutorial - Part 3 - Side Jumps & Corner Detection](https://www.youtube.com/watch?v=g_vj61gbUIg&t=1s)
- [UE4 Climb System - Tutorial - Part 4 - Jump Up & Turn back](https://www.youtube.com/watch?v=AihuFDuvCw0)
- [UE4 Ledge Climbing Tutorial Part1](https://www.youtube.com/watch?v=4yjcwZLQqlE&t=42s)
- [UE4 Ledge Climbing Tutorial Part2](https://www.youtube.com/watch?v=H2xqW7lKkyw&t=891s)
***
### 开发过程中的笔记和总结
#### 物理
- 碰撞检测结果的**HitLocation**和**HitImpactPoint**区别：
**HitImpactPoint指的是两个碰撞体交叉的点，HitLocation是发生碰撞时当前碰撞体的位置**。如图的球形连续检测的例子，HitLocation比HitImpactPoint在水平方向上差了一个球形半径的大小。在视频教程里的例子使用的是HitLocation，所以在计算一些位置的时候都要多加上半径。在我这个工程里统一使用的都是HitImpactPoint。
![在这里插入图片描述](https://img-blog.csdnimg.cn/49cea051b6a44d45a9a1a670c9dbce1b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5rC05puc5pel6bih,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)
#### 蓝图
- 如何简化**多重Branch的蓝图逻辑**？
在视频教程里，作者使用了大量的Branch判断。比如当变量A为true时调用方法1，为false时检查变量B，变量B为true时调用方法2，以此类推，最后嵌套的Branch非常多，蓝图看起来很乱，而且很容易漏掉一个情况或者重复了相同的节点，当再增加新的状态时就要考虑多种可能性很麻烦。下面以跳跃键的逻辑为例，说明一下我是如何优化大量Branch的情况的。
**Before：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/148066ced83b4959a7c3283c2ea74072.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5rC05puc5pel6bih,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)  
**After：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/2549ec48aeb54cf48fe733827f9f5832.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5rC05puc5pel6bih,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)
	- 首先把跳跃的逻辑包装成方法
	- 用Sequence罗列多种可能
	- 满足情况A时走对应逻辑，然后return，这样就不会走下面所有逻辑
	- 将来如果添加新的状态，只需要Sequence再加一个输出引脚就可以了 

- 方法**Function** vs 宏**Marcos** vs 自定义事件**CustomEvent**
	- 宏的输入输出引脚添加方法：**将变量类型改为Exec**
	![在这里插入图片描述](https://img-blog.csdnimg.cn/962ebcaaaf084ba5b1075c1f4ff15095.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5rC05puc5pel6bih,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)
	
|    | Function  | Marcos   | CustomEvent  |
|  ----  | ----  | ---- | ---- |
|    局部变量| 支持  | 不支持   | 不支持  |
|    返回值| 支持  | 支持   | 不支持  |
|    外部调用| 支持 | 不支持   | 支持  |
|   多输入输出引脚| 不支持  | 支持   | 不支持  |
|   LatentAction（Delay型节点）| 不支持  | 支持   | 支持  |
#### 动画
- 动画事件的触发类型**Queued** vs **Branching Points**  
![在这里插入图片描述](https://img-blog.csdnimg.cn/dff9d66de67a49c3a7e2453d98605be1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5rC05puc5pel6bih,size_17,color_FFFFFF,t_70,g_se,x_16#pic_center)

	- Branching Points更加精确，开销更高。
	- 当对动画事件的触发时机要求很精确时使用Branching Points。比如第几帧开枪，第几帧产生攻击判定。要求不精确时使用Queued（实际触发时会差1到几帧），比如走路产生走路烟。 
- **Montage** vs **StateMachine**  
参考：[When to use State Machine or Anim Montage？](https://forums.unrealengine.com/t/when-to-use-state-machine-or-anim-montage/138686/2)
	- StateMachine的优点：
		- 复杂的动画混合
		- 复杂的动画状态切换
	- Montage的优点：
		- 复杂的动画组合
		- 使用方便，可以接受回调，不需要添加动画事件
	- 如果有需求是只能用StateMachine或者只能用Montage才能实现，比如**复杂的混合动画**（StateMachine），**一次播两个动画序列**（Montage），或者**不定次数的Loop动画**，比如这个工程里的保持悬挂状态（StateMachine）。这个时候没有犹豫的必要。
	- 如果两种方案都能实现需求，如何选择？**看这个动画和其他动画的耦合性，耦合性小就用Montage实现**。还是以这个工程为例，比如说TurnBack状态（悬挂时回头看），MoveLeft和MoveRight以及Hanging都可能进入TurnBack状态，同时TurnBack状态可以退出Hanging以及重新进入TurnBack，它和其他动画的耦合性很强，必须做成StateMachine。  
	![在这里插入图片描述](https://img-blog.csdnimg.cn/fbcca96f8b08499eaa906580dfd0b7be.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5rC05puc5pel6bih,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)  
再比如这个工程里的飞跃状态，飞跃状态只能由Hanging状态下才会触发，同时飞跃完成后必定进入Hanging，用Montage就比较合适。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/d36a452ff0e34ab487fe8df76066c69f.gif#pic_center)
- **Play Montage**(CharacterBP) vs **Play Anim Montage**(CharacterBP) vs **Montage Play**(AnimBP)  
这三个函数都能播放Montage，区别在于：
	- 前面两个是角色蓝图里的节点。**Play Montage**是**LatentAction**不能在方法里使用，同时有多个输出节点作为回调，还可以设置播放起始位置（StartingPosition填入百分比），比**Play Anim Montage**好用。
![在这里插入图片描述](https://img-blog.csdnimg.cn/63c9466615b1483fa7a792aa42db06d8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5rC05puc5pel6bih,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)
	- **Montage Play**是动画蓝图的节点，当这个Montage和动画蓝图其他逻辑变量有关联时才适合在动画蓝图里播放Montage。
	- 一般情况下，建议在角色蓝图里使用**Play Montage**。
- **如何查看RootMotion？**  
勾选这两个选项，观察红色指示线。
![在这里插入图片描述](https://img-blog.csdnimg.cn/462561d947a649a5a5cc950a3280fcde.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5rC05puc5pel6bih,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center) 
- **AnimSequence启用了RootMotion没有起作用？**  
找到动画蓝图，修改RootMotionMode。**默认RootMotion只会对Montage生效。**
![在这里插入图片描述](https://img-blog.csdnimg.cn/3457a50758d0412395bfbd195aa74aae.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5rC05puc5pel6bih,size_15,color_FFFFFF,t_70,g_se,x_16#pic_center)

