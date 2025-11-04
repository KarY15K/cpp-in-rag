# 11-Ubuntu 20.04安装Docker

安装环境

安装该教程的Docker之前，请自行安装好Ubuntu 22.04 系统环境。



开始安装

1、检查卸载老版本Docker

```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```

 

2、更新软件包

 

```bash
sudo apt-get update
sudo apt-get upgrade
```



 

3、安装docker依赖

```bash
sudo apt-get install ca-certificates curl gnupg lsb-release
```



4、添加docker密钥

```bash
curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
```





5、添加阿里云docker软件源

```bash
sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
```



报错：ModuleNotFoundError: No module named 'apt_pkg'

<font style="color:rgb(77, 77, 77);">在使用apt命令时出现ModuleNotFoundError: No module named ‘apt_pkg’。</font>  
<font style="color:rgb(77, 77, 77);">出现这个问题是在升级了Python版本之后，如果在更改了Python软连接后出现这个问题可以参考这个解决方法：</font>

cd /usr/lib/python3/dist-packages

lqf@ubuntu:/usr/lib/python3/dist-packages$ sudo cp apt_pkg.cpython-38-x86_64-linux-gnu.so apt_pkg.so



```bash
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.8  2
```



6、安装docker

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.i
```





7、配置用户组(非必须操作，此操作目的是为了以后执行docker命令时无需输入sudo密码，避免这些重复操作而已。)

```bash
sudo usermod -aG docker $USER
sudo reboot
```



 

8、执行完第七步命令的话你电脑会立刻黑屏进行重启，等待重启即可。

检验docker是否安装成功

```bash
sudo systemctl start docker
sudo apt-get -y install apt-transport-https ca-certificates curl software-properties-common
sudo service docker restart
sudo docker run hello-world
```

 

 

执行完hello-world等待一会，会在信息栏打印出该字眼则表示docker安装成功



查看docker版本

```bash
sudo docker version
```



 

查看docker镜像

```bash
sudo docker images
```



 

如果镜像还没的话，查看镜像只会有一行字。大于两行字就是有镜像了，看英文ID之类的可以清晰知道哪个镜像的。

 



> 更新: 2024-08-28 13:51:22  
> 原文: <https://www.yuque.com/linuxer/gscfv1/bbvgvfta3sxqprca>