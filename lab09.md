# 介绍
>模块（module）是Verilog的基本描述单位，用于描述某个设计的功能或结构及与其他模块通信的外部端口。
模块内容是嵌在module和endmodule两个语句之间。每个模块实现特定的功能，模块可进行层次的嵌套，因此可以将大型的数字电路设计分割成大小不一的小模块来实现特定的功能，最后通过由顶层模块调用子模块来实现整体功能，这就是Top-Down的设计思想。

![](https://gss2.bdstatic.com/-fo3dSag_xI4khGkpoWK1HF6hhy/baike/w%3D268%3Bg%3D0/sign=64329eeebc7eca8012053ee1a918f0e0/ac4bd11373f082028c8ca9f247fbfbedaa641be4.jpg)  
简单来说，自顶向下代表着一种解决问题的策略方法，它让我们在面对庞大复杂的问题时能够有解决问题的思路：即，将一个大的问题分解为许多个具有逻辑关联的，有先后顺序的小部分，并不断这样分解直至分得的每一个小部分都足够简单以至于能够轻易地解决。此时解决每一个容易解决的基础单元，然后将得到的各部分答案整合汇总即得到解决整个大问题的综合答案。
关于top-down还有更为详细的解释：  
[baike.baidu](https://baike.baidu.com/item/%E8%87%AA%E9%A1%B6%E5%90%91%E4%B8%8B/9827072?fr=aladdin)  
[wikipedia](https://en.wikipedia.org/wiki/Top-down_and_bottom-up_design)
# 具体案例：洗衣机
本次作业已经写出了洗衣机的执行步骤的伪代码：
[示例](hw08)  
```
1）请使用伪代码分解“正常洗衣”程序的大步骤。包括注水、浸泡等

* 用户输入洗衣配置（水量、过程）
* 注水
* 浸泡
* 洗涤
* 脱水
* 结束

2）进一步用基本操作、控制语句（IF、FOR、WHILE等）、变量与表达式，写出每个步骤的伪代码
~~~
set struct time{
    soak_time
    wash_time
}
halt(success)
procedure config_setdown
    set water_volume , clothes , struct time and steps_given
end procedure
procedure  add_water
    water_in_switch(open)
    set real_water_volume to 0
    while get_water_volume(real_water_volume<water_volume)
        increment real_water_volume by a integer per second
    end while
    water_in_switch(close)
end procedure
procedure soak(soak_time)
    set real_time to 0
    if(time_counter(real_time)<soak_time)
        do nothing 
        set real_time to time_counter(real_time)
    end if 
end procedure
procedure wash(wash_time)
    set real_time to 0
    if(time_counter(real_time)<wash_time)
        motor_run(left)
        motor_run(right)
        motor_run(stop)
        set real_time to time_counter(real_time)
    end if
end procedure
procedure dehydrate
    water_out_switch(open)
    set real_water_volume to get_water_volume()
    while(real_water_volume>0)
        set real_water_volume to get_water_volume()
    end while
    water_out_switch(close)
end procedure
halt(failure)
~~~
3）根据你的实践，请分析“正常洗衣”与“快速洗衣”在用户目标和程序上的异同。你认为是否存在改进（创新）空间，简单说明你的改进意见？

同：都要经历以上的基本程序，且一般都能把衣服洗干净  
异：procedure_config_setdown不同  
改进措施：1）注水浸泡共同完成 2）脱水速度加快 3）洗涤阶段当衣服干净时就停止  

4）通过步骤3），提取一些共性功能模块（函数），简化“正常洗衣”程序，使程序变得更利于人类理解和修改维护。例如：  
wait(time) //等待指定的时间；  
注水(volume,timeout) //在指定时间内完成注水，否则停机；  
排水(timeout)。等子程序
~~~
set procedure wait (time,thing)
    while (time_counter()<time)
        do thing
    end while
end procedure
halt(success)
procedure add_water(volume,timeout)
    wait(time, add_water)
    if(get_water_volume()<volume)
        halt(failure)
    end if
end procedure
procedure wash(time)
    wait(time,wash_clothes)
end procedure
procedure dehydrate
    water_out_switch(open)
    set real_water_volume to get_water_volume()
    while(real_water_volume>0)
        set real_water_volume to get_water_volume()
    end while
    water_out_switch(close)
end procedure
halt(failure)
~~~
```
关于更接近真实情况的演绎详见：[实验报告](http://www.51hei.com/bbs/dpj-125837-1.html)
![](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1542811210&di=76a7f04a9ae9adb5c352b43dd932da98&imgtype=jpg&er=1&src=http%3A%2F%2Fc.51hei.com%2Fa%2Fhuq%2Fa%2Fa%2F9%2F62%2F62.003.jpg)