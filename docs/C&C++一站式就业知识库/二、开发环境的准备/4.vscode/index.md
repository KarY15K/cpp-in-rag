# 4.vscode

<font style="color:#117CEE;">老廖提供主流C/C++就业课程服务：</font>

1. <font style="color:#117CEE;">C/C++通用后台开发</font>
2. <font style="color:#117CEE;">音视频开发</font>
3. <font style="color:#117CEE;">QT开发</font>
4. <font style="color:#117CEE;">游戏开发</font>

<font style="color:#117CEE;">等课程，包括视频教学、资料代码、课程答疑、简历指导、面试复盘等服务，详情咨询</font>**<font style="color:#117CEE;">微信laoliao6668</font>**

**<font style="color:#117CEE;"></font>**

可以通过vscode连接Linux写代码、调试代码。

# 1 安装vscode
 VS Code官网：[https://code.visualstudio.com/Download](https://code.visualstudio.com/Download)

 可以直接点击下载：[https://vscode.download.prss.microsoft.com/dbazure/download/stable/e170252f762678dec6ca2cc69aba1570769a5d39/VSCodeUserSetup-x64-1.88.1.exe](https://vscode.download.prss.microsoft.com/dbazure/download/stable/e170252f762678dec6ca2cc69aba1570769a5d39/VSCodeUserSetup-x64-1.88.1.exe)

 

双击VSCode安装包，点击“我同意此协议”，然后点击“下一步”。



![1713530711520-ca3cf07c-e4ad-44d3-89fb-61759430f13f.png](./img/KXf8CPiDAr-Q19yr/1713530711520-ca3cf07c-e4ad-44d3-89fb-61759430f13f-468664.png)



如果c盘没有位置，可修改安装路径（建议安装到其他盘，避免c盘太满），修改好后点击“下一步”。

![1713530769546-1a3a08ad-b120-44ae-91ba-95266e2fb9bf.png](./img/KXf8CPiDAr-Q19yr/1713530769546-1a3a08ad-b120-44ae-91ba-95266e2fb9bf-227703.png)





点击“下一步”。

![1713530774106-3e299ee0-45fd-4051-ba64-c02be7bd2db3.png](./img/KXf8CPiDAr-Q19yr/1713530774106-3e299ee0-45fd-4051-ba64-c02be7bd2db3-748698.png)





一定要勾选“添加到Path”，其他的也可以勾选上，然后点击“下一步”

![1713530778684-9d85c554-0b79-484d-b971-bfce77c52d49.png](./img/KXf8CPiDAr-Q19yr/1713530778684-9d85c554-0b79-484d-b971-bfce77c52d49-935470.png)





点击“安装”。

![1713530782792-cdd3fd72-7a38-41b9-862e-4d4e9100680f.png](./img/KXf8CPiDAr-Q19yr/1713530782792-cdd3fd72-7a38-41b9-862e-4d4e9100680f-619961.png)





耐心等待安装结束。安装完成后不用运行VSCode。

![1713530789038-71ce9d9e-d8b2-4c31-ab5b-bd2d1d6eb867.png](./img/KXf8CPiDAr-Q19yr/1713530789038-71ce9d9e-d8b2-4c31-ab5b-bd2d1d6eb867-522717.png)



           

# 2 <font style="color:rgb(34, 34, 38);">VSCode连接linux</font>
## 2.1 安装ssh插件
1.安装remote ssh插件

![1718109494218-51bc4ed0-fe7f-4c50-8bc3-85b397bdbba5.png](./img/KXf8CPiDAr-Q19yr/1718109494218-51bc4ed0-fe7f-4c50-8bc3-85b397bdbba5-211958.png)



2. 安装完毕后左侧多了一个图标

![1718109618173-bb7f16e1-76f8-40b6-9564-05c81dfb22bc.png](./img/KXf8CPiDAr-Q19yr/1718109618173-bb7f16e1-76f8-40b6-9564-05c81dfb22bc-106181.png)



## 2.2 配置服务器
1.安装remote ssh完成后，点击下图示中红色箭头指向图标。

![1718109748584-ff457287-8028-453a-8077-fe81b40ca429.png](./img/KXf8CPiDAr-Q19yr/1718109748584-ff457287-8028-453a-8077-fe81b40ca429-421882.png)

2.点击后在正上方搜索框内会出现提示，点击“连接到主机”，然后再点“配置ssh主机”、接着点第一个文件(这是配置文件)

![1718109819612-3e156097-342b-4e40-b5be-4d81eff2f2cb.png](./img/KXf8CPiDAr-Q19yr/1718109819612-3e156097-342b-4e40-b5be-4d81eff2f2cb-968491.png)

![1718109869405-88696b27-34ee-4c0e-8038-ed0990802c04.png](./img/KXf8CPiDAr-Q19yr/1718109869405-88696b27-34ee-4c0e-8038-ed0990802c04-314087.png)比如 ssh lqf@192.168.1.21 -A

![1718109914109-60c19595-12cc-4f99-bd05-12e9cb50df98.png](./img/KXf8CPiDAr-Q19yr/1718109914109-60c19595-12cc-4f99-bd05-12e9cb50df98-559150.png)

![1718109942310-83cb97e3-d5c7-4596-ad5c-035ade3f366c.png](./img/KXf8CPiDAr-Q19yr/1718109942310-83cb97e3-d5c7-4596-ad5c-035ade3f366c-937025.png)

然后右下角有提示

![1718109982529-8a5cde73-d3a9-4c37-bd54-75ef03f7f106.png](./img/KXf8CPiDAr-Q19yr/1718109982529-8a5cde73-d3a9-4c37-bd54-75ef03f7f106-681727.png)

![1718110047708-c4dbcae7-ee5c-4efc-8e12-9c94773c132e.png](./img/KXf8CPiDAr-Q19yr/1718110047708-c4dbcae7-ee5c-4efc-8e12-9c94773c132e-418384.png)

![1718110082140-03977135-73cf-4b20-89a3-76fa147edd40.png](./img/KXf8CPiDAr-Q19yr/1718110082140-03977135-73cf-4b20-89a3-76fa147edd40-852313.png)



进入初始界面

![1718110154546-b4895ef8-7b48-48fa-8d57-967a4171e7b5.png](./img/KXf8CPiDAr-Q19yr/1718110154546-b4895ef8-7b48-48fa-8d57-967a4171e7b5-194651.png)



![1718110188166-e09674c1-2c3c-4b75-abcb-5ac082b25cb5.png](./img/KXf8CPiDAr-Q19yr/1718110188166-e09674c1-2c3c-4b75-abcb-5ac082b25cb5-293485.png)



![1718110217495-7bda6616-6b07-4720-ada9-32bc5246c4ac.png](./img/KXf8CPiDAr-Q19yr/1718110217495-7bda6616-6b07-4720-ada9-32bc5246c4ac-252132.png)



可以先在终端使用命令创建一个写代码的目录，比如cpp_test

![1718110512087-d061fda6-9423-4c63-bf5a-ccbc7f6c1432.png](./img/KXf8CPiDAr-Q19yr/1718110512087-d061fda6-9423-4c63-bf5a-ccbc7f6c1432-222368.png)



![1718110558091-22281d29-2dee-487c-b5c2-b10e7d30c776.png](./img/KXf8CPiDAr-Q19yr/1718110558091-22281d29-2dee-487c-b5c2-b10e7d30c776-492409.png)

然后 OK  回车

![1718110636139-e9113a39-b8cc-4d54-89aa-edf4ccc64df7.png](./img/KXf8CPiDAr-Q19yr/1718110636139-e9113a39-b8cc-4d54-89aa-edf4ccc64df7-252282.png)



![1718110681681-e09e8895-4680-4b40-8b79-3e517cd821c0.png](./img/KXf8CPiDAr-Q19yr/1718110681681-e09e8895-4680-4b40-8b79-3e517cd821c0-359630.png)



![1718110770240-a6305753-a91b-405a-ba35-4e390c13f572.png](./img/KXf8CPiDAr-Q19yr/1718110770240-a6305753-a91b-405a-ba35-4e390c13f572-910978.png)



![1718110792899-d8f15663-39f4-413c-b523-10cd739cff38.png](./img/KXf8CPiDAr-Q19yr/1718110792899-d8f15663-39f4-413c-b523-10cd739cff38-510983.png)

![1718110806359-bd9c4c43-22ac-4059-a9c0-29dae80ed5d1.png](./img/KXf8CPiDAr-Q19yr/1718110806359-bd9c4c43-22ac-4059-a9c0-29dae80ed5d1-357518.png)

此时写代码还不支持自动补全，那怎么办，需要再安装插件才行。





## 2.3 查找之前的远程会话


 ![1718110925933-bdab49b4-7a4c-49bc-8421-99413b609588.png](./img/KXf8CPiDAr-Q19yr/1718110925933-bdab49b4-7a4c-49bc-8421-99413b609588-301557.png)



![1718110990737-cede1f05-cfb7-492c-80b2-16303417c618.png](./img/KXf8CPiDAr-Q19yr/1718110990737-cede1f05-cfb7-492c-80b2-16303417c618-311830.png)

# 3 配置C/C++自动跳转和补全
<font style="color:#DF2A3F;background-color:#FCE75A;">在远程连接Linux主机的状态下操作。</font>

比如![1718111067704-915d415a-b2f7-4caf-bb17-793f21d1f3c7.png](./img/KXf8CPiDAr-Q19yr/1718111067704-915d415a-b2f7-4caf-bb17-793f21d1f3c7-487516.png)



## 3.1 安装插件
先安装以下插件：

+ **<font style="color:rgb(61, 70, 77);">C/C++</font>**

![1718111153979-49d3f391-f363-47b1-994d-00d961efdc7f.png](./img/KXf8CPiDAr-Q19yr/1718111153979-49d3f391-f363-47b1-994d-00d961efdc7f-336837.png)



安装完毕后 ，就能自动补全头文件和函数了。

![1718111369402-94616ef4-01db-4fe7-8825-6d0f37051f6f.png](./img/KXf8CPiDAr-Q19yr/1718111369402-94616ef4-01db-4fe7-8825-6d0f37051f6f-588475.png)

```c
#include <iostream>
#include <pthread.h>
using namespace std;

void test() {
    cout << "test\n";
}
int main() 
{
    cout << "hello word\n";
    test();
    return 0;
}
```

## 3.2 编译代码
![1718111655801-ec4b9da8-1a5c-4206-ae35-2195cb6ec968.png](./img/KXf8CPiDAr-Q19yr/1718111655801-ec4b9da8-1a5c-4206-ae35-2195cb6ec968-959732.png)

找到 Terminal -> New Terminal。

![1718111747427-3824891d-be98-445d-9400-2d3209bcb1f0.png](./img/KXf8CPiDAr-Q19yr/1718111747427-3824891d-be98-445d-9400-2d3209bcb1f0-176569.png)

![1718111772482-3b0aea74-25d7-4855-a296-c696aa1977f8.png](./img/KXf8CPiDAr-Q19yr/1718111772482-3b0aea74-25d7-4855-a296-c696aa1977f8-447818.png)

就可以用命令编译代码了

![1718111810895-0704f713-c966-43d8-ab54-b3d5fbd3beba.png](./img/KXf8CPiDAr-Q19yr/1718111810895-0704f713-c966-43d8-ab54-b3d5fbd3beba-660682.png)



如果报错

![1718111931254-f2233c44-1c7f-4a41-8a46-eba6e651f00b.png](./img/KXf8CPiDAr-Q19yr/1718111931254-f2233c44-1c7f-4a41-8a46-eba6e651f00b-880916.png)



<font style="color:#DF2A3F;"></font>

<font style="color:#DF2A3F;"></font>

# <font style="color:#DF2A3F;">异常处理</font>
## <font style="color:rgb(34, 34, 38);">环境配置][vscode]vscode上ssh输入密码时候用户不是自己设置的解决方法</font>
原文链接：[https://blog.csdn.net/FL1623863129/article/details/131398964](https://blog.csdn.net/FL1623863129/article/details/131398964)

今天遇到一个奇怪的问题，就是vscode去ssh用户时候怎么都显示密码不对，发现上面提示用户名提示和我设置不一样，我明明设置是abc但是显示是电脑用户名，于是去C:\Users\admin\.ssh\config查看发现没有问题，这就奇怪了。于是把config改成下面格式再次连接就可以了

 

```bash
Host [用户名]@[访问地址]
User [用户名]
HostName [访问地址]
ForwardAgent yes
```

<font style="color:rgb(77, 77, 77);">对照这个我改成</font>

```bash
Host abc@192.168.1.122
User abc
HostName 192.168.1.122
ForwardAgent yes
```



> 更新: 2025-07-20 12:21:44  
> 原文: <https://www.yuque.com/linuxer/gscfv1/mq4cgzt4gyarpadc>