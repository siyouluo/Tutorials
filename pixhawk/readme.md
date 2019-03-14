# pixhawk
 
## 连接方式
[Connecting to a Vehicle](http://python.dronekit.io/automodule.html#dronekit.connect)  
[Connection string](http://python.dronekit.io/guide/connecting_vehicle.html#get-started-connecting)  

## 飞行模式
[ArduPilot Tutorial(PDF版)及ArduPilot飞行模式介绍 - xiaoshuai537的博客 - CSDN博客](https://blog.csdn.net/xiaoshuai537/article/details/60465851)  
以下内容转自[PIXHAWK 飞行模式简易说明 - 多旋翼](http://www.moz8.com/thread-36543-1-105.html)  
<table>
<tr>
    <td colspan="4" align="center">PIXHAWK 飞行模式简易说明</td>
</tr>
<tr>
    <td colspan="3" align="center">飞行模式</td>
    <td rowspan="2">是否需GPS定位</td>
</tr>  
<tr>
    <td>中文名</td>
    <td>英文名</td>
    <td>MP简称</td>
</tr>

<tr>
    <td rowspan="2" width="106">自稳模式</td>
    <td width="150">Stabilize Mode</td>
    <td width="106">Stabilize</td>
    <td width="107"><div align="center">否</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：自稳模式会自动保持多轴直升机的水平并且维持目前的朝向。<br />
&nbsp; &nbsp; 相关资料：<a href="http://copter.ardupilot.cn/wiki/stabilize-mode/" target="_blank">http://copter.ardupilot.cn/wiki/stabilize-mode/</a></td>
</tr>

<tr>
    <td rowspan="2" width="106">定高模式</td>
    <td width="150">Altitude Hold</td>
    <td width="106">AltHold</td>
    <td width="107"><div align="center">否</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：此模式不需要GPS支持，pixhawk会根据气压传感器的数据保持当前高度，但不会定点，飞行器依然会漂移，您可以遥控来移动或保持位置。<br />
&nbsp; &nbsp; 定高时飞控通过控制油门来保持高度。<br />
&nbsp; &nbsp; 定高时但仍可用遥控油门来调整高度，但不可以用来降落，因为油门不会降到0。<br />
相关资料：<a href="http://copter.ardupilot.cn/wiki/altitude-hold/" target="_blank">http://copter.ardupilot.cn/wiki/altitude-hold/</a></td>
</tr>

<tr>
    <td rowspan="2" width="106">悬停模式</td>
    <td width="150">Loiter Mode</td>
    <td width="106">Loiter</td>
    <td width="107"><div align="center">是</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：悬停模式就是GPS定点模式。应该在起飞前先让GPS定点，避免在空中突然定位发生问题。其他方面跟定高模式基本相同。<br />
相关资料：<a href="http://copter.ardupilot.cn/wiki/loiter-mode/" target="_blank">http://copter.ardupilot.cn/wiki/loiter-mode/</a></td>
</tr>

<tr>
    <td rowspan="2" width="106">返航模式</td>
    <td width="150">RTL Mode</td>
    <td width="106">RTL</td>
    <td width="107"><div align="center">是</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：返航模式需要GPS定位。当切换到返航模式时，飞行器会返回家的位置。默认情况下，在返航之前，飞行器会首先飞到至少15米的高度（如果当前高度＞15米，就会保持当前高度）。<br />
&nbsp; &nbsp; 家的位置：解锁时后，GPS首次定位的位置。<br />
相关资料：<a href="http://copter.ardupilot.cn/wiki/rtl-mode/" target="_blank">http://copter.ardupilot.cn/wiki/rtl-mode/</a></td>
</tr>

<tr>
    <td rowspan="2" width="106">自动模式</td>
    <td width="150">Auto Mode</td>
    <td width="106">Auto</td>
    <td width="107"><div align="center">是</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：此模式下飞行器会自动执行地面站Mission Planner设定好的任务，例如起飞、按顺序飞向多个航点、旋转、拍照等。<br />
相关资料：<a href="http://copter.ardupilot.cn/wiki/auto-mode/" target="_blank">http://copter.ardupilot.cn/wiki/auto-mode/</a></td>
</tr>

<tr>
    <td rowspan="2" width="106">特技模式</td>
    <td width="150">Acro Mode</td>
    <td width="106">Acro</td>
    <td width="107"><div align="center">否</div></td></tr><tr><td colspan="3" width="406">&nbsp; &nbsp; 模式说明：特技模式是仅基于速率控制的模式。<br />
特技模式提供了遥控器摇杆到飞行器电机之间的最直接的控制关系。<br />
&nbsp; &nbsp; 在特技模式下飞行，就像是不装飞控的遥控直升机一样，需要持续不断的手工摇杆操作。<br />
相关资料：<a href="http://copter.ardupilot.cn/wiki/acro-mode/" target="_blank">http://copter.ardupilot.cn/wiki/acro-mode/</a></td>
</tr>
<tr>
    <td rowspan="2" width="106">运动模式</td>
    <td width="150">Sport Mode</td>
    <td width="106">Sport</td>
    <td width="107"><div align="center">否</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：运动模式也可以说是“速率控制的自稳”加定高。它的设计目的是用于飞行FPV和拍摄移动镜头或者是飞越。<br />
相关资料:<a href="http://copter.ardupilot.cn/wiki/sport-mode/" target="_blank">http://copter.ardupilot.cn/wiki/sport-mode/</a></td>
</tr>
<tr>
    <td rowspan="2" width="106">飘移模式</td>
    <td width="150">Drift Mode</td>
    <td width="106">Drift</td>
    <td width="107"><div align="center">否</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：飘移模式能让用户就像飞行安装有“自动协调转弯”的飞机一样飞行多旋翼飞行器。<br />
相关资料：<a href="http://copter.ardupilot.cn/wiki/drift-mode/" target="_blank">http://copter.ardupilot.cn/wiki/drift-mode/</a></td>
</tr>
<tr>
    <td rowspan="2" width="106">引导模式</td>
    <td width="150">Guided Mode</td>
    <td width="106">Guided</td>
    <td width="107"><div align="center">是</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：此模式需要地面站软件和飞行器之间通信。连接后，在任务规划器Mission Planner软件地图界面上，在地图上任意位置点鼠标右键，选弹出菜单中的“Fly to here”（飞到这里），软件会让你输入一个高度，然后飞行器会飞到指定位置和高度并保持悬停。<br />
相关资料：<a href="http://copter.ardupilot.cn/wiki/guided-mode" target="_blank">http://copter.ardupilot.cn/wiki/guided-mode</a></td>
</tr>
<tr>
    <td rowspan="2" width="106">绕圈模式</td>
    <td width="150">Circle Mode</td>
    <td width="106">Circle</td>
    <td width="107"><div align="center">是</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：当切入绕圈模式时，飞行器会以当前位置为圆心绕圈飞行。而且此时机头会不受遥控器方向舵的控制，始终指向圆心。如果遥控器给出横滚和俯仰方向上的指令，将会移动圆心。与定高模式相同，可以通过油门来调整飞行器高度，但是不能降落。 圆的半径可以通过高级参数设置调整。<br />
相关资料：<a href="http://copter.ardupilot.cn/wiki/circle-mode/" target="_blank">http://copter.ardupilot.cn/wiki/circle-mode/</a></td>
</tr>
<tr>
    <td rowspan="2" width="106">定点模式</td>
    <td width="150">Position Mode</td>
    <td width="106">OF_Loiter</td>
    <td width="107"><div align="center">是</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：定点模式是依赖于GPS的，其余基本和悬停模式相同，在定点模式下，飞行器会保持位置和头的方向不变，同时允许操作者手动控制油门。<br />
相关资料：<a href="http://copter.ardupilot.cn/wiki/position-mode/" target="_blank">http://copter.ardupilot.cn/wiki/position-mode/</a></td>
</tr>
<tr>
    <td rowspan="2" width="106">降落模式</td>
    <td width="150">Land mode</td>
    <td width="106">Land</td>
    <td width="107"><div align="center">否</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：下降至10m的过程中（或是直到声呐检测到了飞行器下面有东西之前）使用常规定高控制器，通过WPNAV_SPEED_DN参数限制下降速度，可通过Mission Planner修改，在10m内，飞行器会以LAND_SPEED参数规定的速率下降，默认为50cm/s。一到达地面，如果飞手的油门位于最低，飞行器就会自动关闭电机并锁定飞行器。<br />
相关资料：<a href="http://copter.ardupilot.cn/wiki/land-mode/" target="_blank">http://copter.ardupilot.cn/wiki/land-mode/</a></td>
</tr>
<tr>
    <td rowspan="2" width="106">跟着我！模式</td>
    <td width="150">Follow Me! Mode</td>
    <td width="106">地面站模式</td>
    <td width="107"><div align="center">是</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：操作者地面站设备带有GPS，此GPS会将位置信息通过地面站和数传电台随时发给飞行器，飞行器实际执行的是“飞到这里”的指令。其结果就是飞行器跟随操作者移动。<br />
相关资料：<a href="http://copter.ardupilot.cn/wiki/follow-me-mode/" target="_blank">http://copter.ardupilot.cn/wiki/follow-me-mode/</a></td>
</tr>
<tr>
    <td rowspan="2" width="106">简单模式</td>
    <td width="150">Simple Modes</td>
    <td width="106">可用7/8通道切换</td>
    <td width="107"><div align="center">否</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：即无头模式，不用管机头朝向，可以将飞行器看成一个点，如果升降舵给出俯冲指令，飞行器就会飞得远离操作者；反之如果给出拉杆指令，飞行器会飞回操作者；给出向左滚转的指令，飞行器会向左飞，反之亦然。<br />
&nbsp; &nbsp; 简单模式可以让你用起飞时的头的方向控制飞行器，仅需要较好的罗盘指向。<br />
相关资料：<a href="http://copter.ardupilot.cn/wiki/simpleandsuper-simple-modes/" target="_blank">http://copter.ardupilot.cn/wiki/simpleandsuper-simple-modes/</a></td>
</tr>
<tr>
    <td rowspan="2" width="106">超简单模式</td>
    <td width="150">Super Simple Modes</td>
    <td width="106">可用7/8通道切换</td>
    <td width="107"><div align="center">是</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：超简单模式可以让你以飞行器朝向家——解锁位置的方向控制飞行器，但需要较好的GPS定位。<br />
&nbsp; &nbsp; 模型在家10m以内时，方向是不会更新的，所以要避免在家附近飞<br />
&nbsp; &nbsp; 在起飞时要确保控制是正确的，和简单模式一样，你应该在解锁时站在模型后面，飞手和模型所朝方向也应是一样的。<br />
相关资料：<a href="http://copter.ardupilot.cn/wiki/simpleandsuper-simple-modes/" target="_blank">http://copter.ardupilot.cn/wiki/simpleandsuper-simple-modes/</a></td>
</tr>

</table>

## 查看飞控日志  
[APM和PIX飞控日志分析入门贴 - 闲来无事搞搞机 - CSDN博客](https://blog.csdn.net/xazzh/article/details/72814567)

# websites
[Welcome to DroneKit-Python’s documentation!](http://python.dronekit.io/)  
[使用DroneKit控制无人机 - DarrenChan陈驰 - 博客园](https://www.cnblogs.com/DarrenChan/p/7847199.html)  
