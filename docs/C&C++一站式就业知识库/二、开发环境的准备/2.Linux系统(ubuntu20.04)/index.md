# 2.Linux系统(ubuntu20.04)

<font style="color:#117CEE;">老廖提供主流C/C++就业课程服务：</font>

1. <font style="color:#117CEE;">C/C++通用后台开发</font>
2. <font style="color:#117CEE;">音视频开发</font>
3. <font style="color:#117CEE;">QT开发</font>
4. <font style="color:#117CEE;">游戏开发</font>

<font style="color:#117CEE;">等课程，包括视频教学、资料代码、课程答疑、简历指导、面试复盘等服务，详情咨询</font>**<font style="color:#117CEE;">微信laoliao6668</font>**





参考文章：[https://zhuanlan.zhihu.com/p/355314438](https://zhuanlan.zhihu.com/p/355314438)

老廖在原文链接上有修改。



对于Linux初学者老廖建议安装桌面版本的Ubuntu，并且不建议追求最新的版本，<font style="color:#DF2A3F;">20.04是比较合适的一个版本。</font>



<font style="color:#DF2A3F;">大家不要设置静态IP，千万不要设置静态IP。</font>



# <font style="color:rgb(25, 27, 31);">1 安装虚拟机</font>
 参考：[1.VMware安装](https://www.yuque.com/linuxer/gscfv1/gtcf7x1t5f3zg2fl)

<font style="color:rgb(25, 27, 31);">  
</font>

# <font style="color:rgb(25, 27, 31);">2 Ubuntu镜像下载</font>
<font style="color:rgb(25, 27, 31);">Linux有很多发行版，选择较为友好的Ubantu。登录清华镜像，下载20.04版本的Ubantu。镜像链接如下：</font>

[Tsinghua Open Source Mirrormirrors.tuna.tsinghua.edu.cn/](https://link.zhihu.com/?target=https%3A//mirrors.tuna.tsinghua.edu.cn/)

**<font style="color:#DF2A3F;">建议直接点击下载</font>**：[https://mirrors.tuna.tsinghua.edu.cn/ubuntu-releases/20.04/ubuntu-20.04.6-desktop-amd64.iso](https://mirrors.tuna.tsinghua.edu.cn/ubuntu-releases/20.04/ubuntu-20.04.6-desktop-amd64.iso)<font style="color:rgb(25, 27, 31);">  
</font>![1718095220156-d07170be-9331-4a33-b5a0-9d159ba5d126.png](./img/GrlM7ByGkhLGvDTj/1718095220156-d07170be-9331-4a33-b5a0-9d159ba5d126-827363.png)![1718095271714-627ab8e2-838b-4a59-9add-adfdb8fb0c37.png](./img/GrlM7ByGkhLGvDTj/1718095271714-627ab8e2-838b-4a59-9add-adfdb8fb0c37-738463.png)

<font style="color:rgb(25, 27, 31);">  
</font>

# <font style="color:rgb(25, 27, 31);">3 虚拟机配置</font>
## 3.1 Ubuntu配置和安装
<font style="color:rgb(25, 27, 31);">step1：打开第一步所下载的VMware Workstation。在主界面中，选择【创建新的虚拟机】。</font>

![1718095336498-d8cb3710-b220-4f4a-8fff-0021f95b63b1.png](./img/GrlM7ByGkhLGvDTj/1718095336498-d8cb3710-b220-4f4a-8fff-0021f95b63b1-381907.png)

<font style="color:rgb(25, 27, 31);">step2：如图，会自动弹出【新建虚拟向导】，选择【自定义（高级）】后，点击【下一步】。</font>![1718095510702-7c4b6bb9-56a4-431b-9944-5e744d5853b5.png](./img/GrlM7ByGkhLGvDTj/1718095510702-7c4b6bb9-56a4-431b-9944-5e744d5853b5-044400.png)

<font style="color:rgb(25, 27, 31);">step3：这一步选择默认值 或者 16.x 即可，点击【下一步】。</font>

<font style="color:rgb(25, 27, 31);background-color:#FBDE28;">程序员老廖注：主要我是要发给其他同学使用所以选择了兼容16.x，如果是自己使用使用默认配置即可。</font>

![1718095552746-18e4dc21-8d82-4a33-9909-7c3d4b1cddda.png](./img/GrlM7ByGkhLGvDTj/1718095552746-18e4dc21-8d82-4a33-9909-7c3d4b1cddda-940517.png)

<font style="color:rgb(25, 27, 31);">step4：选择通过【浏览】选择刚才下载的镜像的位置并加载好镜像，自动识别到为ubuntu</font>

<font style="color:rgb(25, 27, 31);">点击【下一步】</font>

![1718095707253-98cc2229-8e01-481a-b18b-6647527bf3dd.png](./img/GrlM7ByGkhLGvDTj/1718095707253-98cc2229-8e01-481a-b18b-6647527bf3dd-088102.png)

<font style="color:rgb(25, 27, 31);">step5：填写【全名】 【用户名】 【密码】</font>

![1718095951901-50af54c3-a4ac-4303-8991-d34db4ba6ab0.png](./img/GrlM7ByGkhLGvDTj/1718095951901-50af54c3-a4ac-4303-8991-d34db4ba6ab0-114343.png)

<font style="color:rgb(25, 27, 31);">step6：【虚拟机名称】填写，以及选择安装到哪个【位置】，点击【下一步】。</font>

![1718096075728-b3129911-fd3e-4892-8e55-5a44c5b00ca1.png](./img/GrlM7ByGkhLGvDTj/1718096075728-b3129911-fd3e-4892-8e55-5a44c5b00ca1-979038.png)



<font style="color:rgb(25, 27, 31);">step7：【处理器数量】与【每个处理器的内核数量】，我是设置为4，2，点击【下一步】。</font>

![1718096239666-8c69375a-7ec0-413e-983f-9ddef5ae5a66.png](./img/GrlM7ByGkhLGvDTj/1718096239666-8c69375a-7ec0-413e-983f-9ddef5ae5a66-483127.png)

<font style="color:rgb(25, 27, 31);">step8：选择【此虚拟机的内存】，此值应依据电脑本身情况酌情调整。常用值为1G（1024MB）、2G（2048MB）、4G（4096MB）等等。</font>**<font style="color:rgb(25, 27, 31);">在这里选择4G</font>**<font style="color:rgb(25, 27, 31);">。</font>

![1718096298755-ff7e3eb9-408d-4879-8492-31431869cdce.png](./img/GrlM7ByGkhLGvDTj/1718096298755-ff7e3eb9-408d-4879-8492-31431869cdce-734872.png)

<font style="color:rgb(25, 27, 31);">step9：选择【使用网络地址转换】，点击【下一步】。</font>

<font style="color:rgb(25, 27, 31);">桥接模式是 安装的Ubuntu和主机是同样的ip网段。</font>

![1718096333866-67ac14c0-8606-4e79-b373-27da17b19a47.png](./img/GrlM7ByGkhLGvDTj/1718096333866-67ac14c0-8606-4e79-b373-27da17b19a47-544891.png)

<font style="color:rgb(25, 27, 31);">step10：选择推荐的【LSI Logic】即可，点击【下一步】。</font>

![1718096439373-1388e4bb-5407-47b5-8705-bfa929c43533.png](./img/GrlM7ByGkhLGvDTj/1718096439373-1388e4bb-5407-47b5-8705-bfa929c43533-382237.png)

<font style="color:rgb(25, 27, 31);">step11：选择推荐的【SCSI】即可，点击【下一步】。</font>

![1718096476602-78ca7779-5a69-4adf-8fdc-8d8e1c58ec9e.png](./img/GrlM7ByGkhLGvDTj/1718096476602-78ca7779-5a69-4adf-8fdc-8d8e1c58ec9e-532502.png)

<font style="color:rgb(25, 27, 31);">step12：选择【创建新虚拟磁盘】，点击【下一步】。</font>

![1713520894237-8a315d93-d5e5-444f-b89d-cab3291a421d.webp](./img/GrlM7ByGkhLGvDTj/1713520894237-8a315d93-d5e5-444f-b89d-cab3291a421d-847947.webp)

<font style="color:rgb(25, 27, 31);">step13：设置【最大磁盘大小】为40，并选择【将虚拟磁盘拆分成多个文件】后，点击【下一步】。最大磁盘大小默认为20GB，磁盘剩余空间较大的话该数值可以往上加，</font><font style="color:#DF2A3F;"> 这里设置的磁盘大小不会马上占用对应的磁盘，设置个120GB也没有关系的（老廖补充）</font><font style="color:rgb(25, 27, 31);">。</font>

![1718096560021-e69c195b-539d-45d1-82c0-a7b3dee76865.png](./img/GrlM7ByGkhLGvDTj/1718096560021-e69c195b-539d-45d1-82c0-a7b3dee76865-008091.png)

<font style="color:rgb(25, 27, 31);">step14：点击【下一步】。</font>

![1718096580917-876a5405-80a7-49eb-9581-879af4dba21b.png](./img/GrlM7ByGkhLGvDTj/1718096580917-876a5405-80a7-49eb-9581-879af4dba21b-264020.png)

<font style="color:rgb(25, 27, 31);">step15：点击【完成】。</font>

![1718096597162-4b374f95-2ec2-4f13-82e5-5cd7539d4785.png](./img/GrlM7ByGkhLGvDTj/1718096597162-4b374f95-2ec2-4f13-82e5-5cd7539d4785-596992.png)



<font style="color:rgb(25, 27, 31);">step16：</font>在上一步骤点击完成后，开始自动安装Ubuntu。

![1718096685235-88215c9f-0dd1-4f4d-8903-52e27d23f267.png](./img/GrlM7ByGkhLGvDTj/1718096685235-88215c9f-0dd1-4f4d-8903-52e27d23f267-669228.png)

自动安装成功的速度和电脑配置有一定的关系，

![1718096985335-2d063245-d318-4590-a361-a2a734ebc01d.png](./img/GrlM7ByGkhLGvDTj/1718096985335-2d063245-d318-4590-a361-a2a734ebc01d-861531.png)

![1718098440282-f9c9cb7d-bef7-4bd1-b0d3-dd6c3b177872.png](./img/GrlM7ByGkhLGvDTj/1718098440282-f9c9cb7d-bef7-4bd1-b0d3-dd6c3b177872-900902.png)

![1718098459392-234d0c8e-8e6e-42ba-81ce-32e0e57728d0.png](./img/GrlM7ByGkhLGvDTj/1718098459392-234d0c8e-8e6e-42ba-81ce-32e0e57728d0-969000.png)

![1718097470239-d9ffbe0c-8336-46b0-a15f-a94a782efa12.png](./img/GrlM7ByGkhLGvDTj/1718097470239-d9ffbe0c-8336-46b0-a15f-a94a782efa12-957323.png)



![1718097541981-5a2643d9-5434-483d-9f67-52247ed2e026.png](./img/GrlM7ByGkhLGvDTj/1718097541981-5a2643d9-5434-483d-9f67-52247ed2e026-013432.png)



![1718097572765-01785f2b-b0b7-4481-929e-b097cf9a7608.png](./img/GrlM7ByGkhLGvDTj/1718097572765-01785f2b-b0b7-4481-929e-b097cf9a7608-777693.png)



![1718097617604-43a1447f-a704-4c92-a2c4-46ec78aa78c1.png](./img/GrlM7ByGkhLGvDTj/1718097617604-43a1447f-a704-4c92-a2c4-46ec78aa78c1-942802.png)



![1718097661387-0d814f62-9f3c-42c3-9f9a-ae8550fbbdcd.png](./img/GrlM7ByGkhLGvDTj/1718097661387-0d814f62-9f3c-42c3-9f9a-ae8550fbbdcd-357149.png)



![1718097688823-8803d33c-badc-4806-ba2a-48dc4de962c4.png](./img/GrlM7ByGkhLGvDTj/1718097688823-8803d33c-badc-4806-ba2a-48dc4de962c4-116535.png)



## 3.2 打开终端安装ifconfig命令
CTRL + Alt + T弹出终端

![1718098808352-0d469d82-cd27-42b2-9c5c-785fea9b3367.png](./img/GrlM7ByGkhLGvDTj/1718098808352-0d469d82-cd27-42b2-9c5c-785fea9b3367-511686.png)



在终端输入

```bash
sudo apt-get update
sudo apt-get install net-tools
```



然后再ifconfig即可看到当前 ubuntu的ip

![1718098982208-89e1b1d6-359b-4789-b07d-95969ffda445.png](./img/GrlM7ByGkhLGvDTj/1718098982208-89e1b1d6-359b-4789-b07d-95969ffda445-921331.png)



## 3.4 如何操作虚拟机
### 如果要重新编辑虚拟机
先关机

![1718098205618-6612c9ea-0501-463e-8a64-6378a195ae43.png](./img/GrlM7ByGkhLGvDTj/1718098205618-6612c9ea-0501-463e-8a64-6378a195ae43-690650.png)



<font style="color:rgb(25, 27, 31);">回到VMware Station界面，单击界面左侧【编辑虚拟机位置】。</font>

 ![1718098276987-3338e2a4-d70d-4d90-bd1c-6f25df4af702.png](./img/GrlM7ByGkhLGvDTj/1718098276987-3338e2a4-d70d-4d90-bd1c-6f25df4af702-706990.png)



## <font style="color:rgb(34, 34, 38);">虚拟机电源类选项说明</font>
1. 启动客户机和开机: 前者是正常开机。后者相当于物理上电。打开电源时进入固件：开机自动进入boss页面。

![1718107183025-9a948f64-0dfc-49f5-9277-21797f991a9a.png](./img/GrlM7ByGkhLGvDTj/1718107183025-9a948f64-0dfc-49f5-9277-21797f991a9a-894352.png)



2. 关闭客户机和关机：前者是正常关机后者是强制断电。

![1718107189101-09f15bdf-9651-4f8c-a2b1-8d6e2927f63e.png](./img/GrlM7ByGkhLGvDTj/1718107189101-09f15bdf-9651-4f8c-a2b1-8d6e2927f63e-102598.png)



3. 挂起客户机和挂起：前者是休眠(释放cpu和内存)，将系统里的数据进程由内存写入到磁盘。后者是强制性休眠。

![1718107204382-028525a4-3e2d-4ff7-b788-c90043017b63.png](./img/GrlM7ByGkhLGvDTj/1718107204382-028525a4-3e2d-4ff7-b788-c90043017b63-713793.png)



4. 重新启动客户机和重置：前者是正常重启。后者是强制重启；

![1718107213163-b32b0f32-6460-48cc-bd7d-cdc9979029e6.png](./img/GrlM7ByGkhLGvDTj/1718107213163-b32b0f32-6460-48cc-bd7d-cdc9979029e6-646967.png)



注意：凡是强制的都有数据丢失的可能性。



参考链接：[https://blog.csdn.net/m0_66741725/article/details/127683163](https://blog.csdn.net/m0_66741725/article/details/127683163)

### 重新开启虚拟机
![1718098498342-ae39dcde-777f-4492-a179-ca11435e9c60.png](./img/GrlM7ByGkhLGvDTj/1718098498342-ae39dcde-777f-4492-a179-ca11435e9c60-990585.png)

![1718098355852-ef5f8fb5-14c2-4662-ac1d-c3e337b0c221.png](./img/GrlM7ByGkhLGvDTj/1718098355852-ef5f8fb5-14c2-4662-ac1d-c3e337b0c221-679808.png)



### 如果想下次开机继续保留ubuntu的状态
![1718098606353-12f1cc1b-46ec-4d91-be21-861f1bbaaeea.png](./img/GrlM7ByGkhLGvDTj/1718098606353-12f1cc1b-46ec-4d91-be21-861f1bbaaeea-207483.png)

挂起后要启动也是点击 开启此虚拟机：

![1718098498342-ae39dcde-777f-4492-a179-ca11435e9c60.png](./img/GrlM7ByGkhLGvDTj/1718098498342-ae39dcde-777f-4492-a179-ca11435e9c60-814564.webp)





### 保存快照
VMware虚拟机的一个很重要的功能就是快照, 简而言之就是一种快速的系统备份与还原的功能, 就像时光机, 可以倒退到某个点时系统状态, 比如, 当你装某个软件, 或做某项测试时, 如果把系统搞坏了, 那么你可能就要重装系统以及装相当软件及配置,每次出问题,都要这样做, 很费时间与精力, 解决这个问题, 可以在进行测试前先做一个快照, 如果发生意外, 只需要回到那个快照即可, 不需要重头再装相关软件及配置

要使用VMware的快照功能, 需要执行 "虚拟机"->"快照"->"拍摄快照"



后续系统出问题了可以恢复之前保存的快照。



但要强调的是，**<font style="color:#DF2A3F;">快照是比较占用磁盘空间的。</font>**

 

## 3.5 注意事项
### <font style="color:#DF2A3F;">此主机支持Intel VT-x，但Intel VT-x处于禁用状态</font>
<font style="color:rgb(25, 27, 31);">【注】：此时若出现【</font>**<font style="color:#DF2A3F;">此主机支持Intel VT-x，但Intel VT-x处于禁用状态】的提示</font>**<font style="color:rgb(25, 27, 31);">。请进入BIOS，将【Intel Virtual Technology】设为【Enabled】即可，具体方法可百度   </font><font style="color:#DF2A3F;">（老廖补充，可以搜索自己对应型号电脑设置bios的方法）。</font>

![1713520894795-14e2c525-bfb0-4128-983d-1ae3897539b3.webp](./img/GrlM7ByGkhLGvDTj/1713520894795-14e2c525-bfb0-4128-983d-1ae3897539b3-945970.webp)



### 要不要设置静态IP
1. <font style="color:#DF2A3F;">新手不建议配置静态ip，不要跟风网上配置静态ip的方法，很容易导致网络异常。 </font>

当动态ip发生变化时，在ubuntu里通过ifconfig命令获取新的ip。



# 4 更换国内源
Ubuntu后续包的下载默认是使用国外的源，容易出现下载慢或者不能正常下载的情况，建议换成国内的源，比如清华的源。

## Ubuntu20.04更换apt清华镜像源
这里要特别注意，不同的Ubuntu版本的源是不一样的，<font style="color:#DF2A3F;">这里配置的源只适合Ubuntu20.04</font>

先备份原有的源： 

```bash
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak 
```



 <font style="color:rgb(77, 77, 77);">编辑上面的文件，可以用vim工具，如果安装的是桌面版本有</font>[图形界面](https://so.csdn.net/so/search?q=%E5%9B%BE%E5%BD%A2%E7%95%8C%E9%9D%A2&spm=1001.2101.3001.7020)<font style="color:rgb(77, 77, 77);">，可以用gedit</font>

```bash
sudo gedit /etc/apt/sources.list
```



 

把下面的内容替换文件里内容（删除原有的内容），保存

```bash
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
```



 然后做更新

```bash
sudo apt-get update
sudo apt-get upgrade
```

 





> 更新: 2025-04-02 14:24:17  
> 原文: <https://www.yuque.com/linuxer/gscfv1/tamrudtblo15hznl>