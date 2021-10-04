---
layout: post
title:  "写给零基础者的 GNU/Linux 安装教程"
date:   2021-10-04 04:00 
---

> [![Creative Commons License](https://i.creativecommons.org/l/by-sa/4.0/88x31.png)](http://creativecommons.org/licenses/by-sa/4.0/)
>
> This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).
>
> 这份文档以 [CC-BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 进行许可。简单地说，您可以在合适的署名下以相同的许可证，自由地共享、演绎该作品（衍生品需要标明对原作进行了修改），无论用于商业或非商业目的，具体细节以许可证文本为准。

## 导读

该文档的安装步骤基于 Windows 上的虚拟机软件 VirtualBox 及 GNU/Linux 发行版 Ubuntu。

最终效果（参考）：

![](install-neofetch-7.png.jpg)

### 组成部分及阅读顺序

1. [下载并安装 VirtualBox](#下载并安装-virtualbox)
2. [下载 Ubuntu 安装镜像](#下载-ubuntu-安装镜像)
3. [配置 VirtualBox 虚拟机并启动 Ubuntu 安装程序](#配置-virtualbox-虚拟机并启动-ubuntu-安装程序)
4. [安装 Ubuntu](#安装-ubuntu)

- [附录 1：分辨率过低导致安装界面显示不全的解决方法](#附录-1分辨率过低导致安装界面显示不全的解决方法)

其中，1、2 步相互独立，第 3 步依赖于第 1、2 步；第 4 步依赖于第 3 步，但也可单独作为使用其他方式安装 Ubuntu 的参考。

```
 1     2
 |    /
 |---/
\|/
 3
 |
\|/
 4
```

### 关于截图

没有特殊操作的截图都已折叠起来，可按需单击展开，其中

- 对应步骤的文字说明（如果有）一般会位于截图上方
- 需要点击的步骤中，截图上的鼠标一般会位于需要点击处。

截图仅供参考，以实际情况为准。

## 下载并安装 VirtualBox

1. 用浏览器打开 mirrorz（<https://mirrorz.org/app/VirtualBox>）选择一个标明 `Win` 的 VirtualBox 镜像（`latest(Win)`）下载。

    
    点击其中一处 `latest(Win)`
    
    <details markdown="1" ><summary>截图</summary>

    ![mirrorz-virtualbox](mirrorz-virtualbox.png.jpg)
    
    </details>

2. 下载完成后，运行下载下来的安装包文件并安装。
    
    <details markdown="1" ><summary>点击展开详细步骤截图</summary>

    > 点击下一步：
    >
    > ![](install-virtualbox-1.png.jpg)
    >
    > 此处可以自定义安装的组件以及安装目录，没有特殊需要可以保留默认设置。点击下一步：
    >
    > ![](install-virtualbox-2.png.jpg)
    >
    > 此处选项无特殊需要可保留默认值。点击下一步：
    >
    > ![](install-virtualbox-3.png.jpg)
    >
    > 点击“是”后将会加入安装状态：
    >
    > ![](install-virtualbox-4.png.jpg)
    >
    > 进入安装状态时可能会弹出请求赋予权限的提示框，点击“是”：（您的安装过程中不一定会有此提示框，如果有，您的提示框也可能无需输入密码）
    >
    > ![](install-virtualbox-6.png.jpg)
    >
    > 正在安装，请稍等片刻：
    >
    > ![](install-virtualbox-7.png.jpg)
    >
    > 安装过程中可能会弹出安装驱动程序的提示，点击“安装”：
    >
    > ![](install-virtualbox-8.png.jpg)
    >
    > 安装完成的界面：
    >
    > ![](install-virtualbox-9.png.jpg)
    </details>


## 下载 Ubuntu 安装镜像

此处我们使用 20.04.3 版[^ubuntu-release]的桌面版（`Desktop`），推荐重庆大学的同学从重庆大学开源软件镜像站下载 Ubuntu 镜像：<https://mirrors.cqu.edu.cn/ubuntu-releases/20.04.3/ubuntu-20.04.3-desktop-amd64.iso>，此外也可以从 mirrorz <https://mirrorz.org/os/ubuntu> 选择合适的镜像站进行下载。

## 配置 VirtualBox 虚拟机并启动 Ubuntu 安装程序

### 新建虚拟机

1. 打开 VirtualBox

    <details markdown="1" ><summary>截图</summary>

    ![](gui.png.jpg)

    </details>

2. 点击新建按钮，设置虚拟机名称（自己定的名称），`类型` 设为 `Linux`，点击`下一步`

    <details markdown="1" ><summary>截图</summary>

    ![](setup-vm-2.png.jpg)

    </details>

3. 内存设置为至少 4096MB[^ubuntu-require]，点击下一步

    <details markdown="1" ><summary>截图</summary>

    ![](setup-vm-3.png.jpg)

    </details>

4. 是否创建虚拟硬盘，无特殊需求保持默认选项（`现在创建虚拟硬盘`）即可，点击`创建`；如果您只想试用 Ubuntu 而不想安装，此处选择`不添加虚拟硬盘`，然后该教程跳到[虚拟机的一些可选配置](#虚拟机的一些可选配置)

    <details markdown="1" ><summary>截图</summary>

    ![](setup-vm-4.png.jpg)

    </details>

5. 虚拟硬盘文件类型保持默认选项即可，点击`下一步`

    <details markdown="1" ><summary>截图</summary>

    ![](setup-vm-5.png.jpg)

    </details>

6. 虚拟硬盘存储方式，若不打算重度使用 Ubuntu，那么可保持默认选项（`动态分配`），点击`下一步`

    <details markdown="1" ><summary>截图</summary>

    ![](setup-vm-6.png.jpg)
    
    </details>

7. 虚拟硬盘文件位置请视需要自定义或保持默认，大小请设为不低于 9GB，推荐 24GB 及以上[^ubuntu-require]，之后点击`创建`，虚拟机就可以创建好了

    <details markdown="1" ><summary>截图</summary>

    ![](setup-vm-7.png.jpg)

    创建好的虚拟机：

    ![](config-vm-1.png.jpg)

    </details>


### 虚拟机的一些可选配置[^ubuntu-require]

1. 打开 VirtualBox，如图所示，点击`设置`

    <details markdown="1" ><summary>截图</summary>

    ![](config-vm-1.png.jpg)

    </details>

2. 点击窗口左侧的`系统`，勾选`启用 EFI`[^ubuntu-require][^efi]

    <details markdown="1" ><summary>截图</summary>

    ![](config-vm-2.png.jpg)

    </details>

3. 点击`处理器`，将`处理器数量`调至至少为 2[^ubuntu-require]

    <details markdown="1" ><summary>截图</summary>

    ![](config-vm-3.png.jpg)

    </details>

### 引导 Ubuntu 安装程序

1. 做完前述操作后，点击`启动`

    <details markdown="1" ><summary>截图</summary>

    ![](load-linux-vm-1.png.jpg)

    </details>

2. 片刻过后，应该会看到`选择启动盘`的界面，如图所示，点击黄色文件夹图标（`选择一个虚拟光盘文件`）

    <details markdown="1" ><summary>截图</summary>

    ![](load-linux-vm-1.png.jpg)

    </details>

3. 在弹出来的窗口中点击`注册`，选择之前下载好的 Ubuntu 安装镜像 `ubuntu-20.04.3-desktop-amd64.iso`后，点击`打开`

    <details markdown="1" ><summary>截图</summary>

    ![](load-linux-vm-2.png.jpg)

    ![](load-linux-vm-3.png.jpg)

    ![](load-linux-vm-4.png.jpg)

    </details>

4. 选中刚刚添加的安装镜像，并点击`选择`，之后可以看到`ubuntu-20.04.3-desktop-amd64.iso`的字样已经出现在`选择启动盘`的界面，点击`启动`

    <details markdown="1" ><summary>截图</summary>

    ![](load-linux-vm-5.png.jpg)

    ![](load-linux-vm-6.png.jpg)

    </details>

5. **注意，之后要进入 VirtualBox 的虚拟系统中，您的鼠标和键盘会被虚拟系统占用，被用于操作虚拟系统，按右 Ctrl 键（键盘右侧的 Ctrl 键）可以解除这种占用状态，您之后可能会需要多次使用此键来解除占用翻阅资料。** 点击虚拟机窗口的虚拟区域，出现占用鼠标、键盘的提示，按`捕获`

    <details markdown="1" ><summary>截图</summary>

    ![](load-linux-vm-7.png.jpg)

    </details>

## 安装 Ubuntu

1. 接上一个步骤，我们会看到如下界面短暂地出现

    <details markdown="1" ><summary>截图</summary>

    ![](install-ubuntu-1.png.jpg)

    ![](install-ubuntu-2.png.jpg)

    </details>

2. 然后进入如下界面，此时系统在校验安装镜像的完整性（即，镜像的数据是否正确），可以按 `Ctrl+C` 跳过该步骤（生产环境中不建议这么做）

    <details markdown="1" ><summary>截图</summary>

    ![](install-ubuntu-3.png.jpg)

    </details>    

3. 接下来可能会依次在如下界面停留片刻，请耐心等待

    <details markdown="1" ><summary>截图</summary>

    转圈

    ![](install-ubuntu-4.png.jpg)

    黑屏

    ![](install-ubuntu-5.png.jpg)

    进入图形界面

    ![](install-ubuntu-6.png.jpg)

    </details>    

4. 之后可以看到安装程序自动启动

    <details markdown="1" ><summary>截图</summary>

    ![](install-ubuntu-7.png.jpg)

    </details>

5. 在窗口左侧的语言选择框里向下滚动，选择`中文(简体)`

    <details markdown="1" ><summary>截图</summary>

    ![](install-ubuntu-8.png.jpg)

    </details>

6. 点击`安装 Ubuntu`；而如果您不打算安装 Ubuntu 而仅仅想试用，请点击`试用 Ubuntu`，请跳到下面 `试用 Ubuntu` 的折叠内容


    <details markdown="1" ><summary>截图</summary>

    ![](install-ubuntu-9.png.jpg)

    </details>

    <details markdown="1" ><summary>试用 Ubuntu</summary>

    点击`使用 Ubuntu`后虚拟机会自动重启，一会儿后进入 Ubuntu 的桌面。

    此时，按 `Ctrl+Alt+T` 打开终端，输入
    ``` bash
    deb http://mirrors.cqu.edu.cn/ubuntu/ focal universe | sudo tee -a /etc/apt/sources.list
    ```
    注意以下几点：
    - `universe` 和 `sudo` 之间的一竖不是字母 `l`，而是 `\` 键上符号 `|`。
    - **单词不要拼错，符号不要打错，空格不要漏了**

    按回车键，之后跳到该教程的[最终效果](#最终效果)部分。
    </details>

7. 键盘布局，一般保持默认即可，点击`继续`

    <details markdown="1" ><summary>截图</summary>

    ![](install-ubuntu-9.png.jpg)

    </details>

8. 无特殊需要保持默认即可（时间不充裕的话可以取消`安装Ubuntu时下载更新`的勾选）点击`继续`

    <details markdown="1" ><summary>截图</summary>

    ![](install-ubuntu-10.png.jpg)

    </details>

9. 如果使用虚拟机安装，无特殊需要保持默认即可；如果在真机上安装，请根据实际情况进行配置，点击`现在安装`

    <details markdown="1" ><summary>截图</summary>

    ![](install-ubuntu-11.png.jpg)

    </details>

10. 弹出分区修改的确认对话框，点击`继续`

    <details markdown="1" ><summary>截图</summary>

    ![](install-ubuntu-12.png.jpg)

    </details>

11. 选择时区，用鼠标在中国的版图内点一下（如果您在北京时区），地图下面出现 `Shanghai` 字样即为北京时区。之后点击`继续`。

    PS: 这个地图上标注的是各地区的时区划分，而**不是国界**

    <details markdown="1" ><summary>截图</summary>

    ![](install-ubuntu-13.png.jpg)

    ![](install-ubuntu-14.png.jpg)

    </details>

12. 填写信息，其中填写`姓名`（请使用英文字母、数字、下划线等组合，不要是用空格和中文）后`计算机名`、`用户名`会自动填充，无特殊需要这几项可以不改，之后填写密码，以后会使用这个密码来登陆。设置完后点击`继续`

    <details markdown="1" ><summary>截图</summary>

    ![](install-ubuntu-15.png.jpg)

    </details>

13. 程序进入安装过程，此处可能需要一些时间，请耐心等待

    <details markdown="1" ><summary>截图</summary>

    ![](install-ubuntu-16.png.jpg)

    ![](install-ubuntu-17.png.jpg)

    </details>

14. 安装完成，点击`现在重启`以重启虚拟机
    
    <details markdown="1" ><summary>截图</summary>

    ![](install-ubuntu-18.png.jpg)

    </details>

15. 片刻之后出现 `Please remove the installation medium, then press ENTER` 字样的提示，按回车键（Enter）

    <details markdown="1" ><summary>截图</summary>

    ![](install-ubuntu-19.png.jpg)

    </details>

16. 之后虚拟机会重启，一会儿可以见到登陆界面

    <details markdown="1" ><summary>截图</summary>

    ![](install-ubuntu-20.png.jpg)

    </details>

17. 单击自己的用户名，输入密码并按回车键以登陆，之后可见`连接帐号`的向导，无相关需求点击`跳过`即可

    <details markdown="1" ><summary>截图</summary>

    ![](install-ubuntu-21.png.jpg)

    </details>

18. Livepatch 配置，无需要直接点击`前进`即可

    <details markdown="1" ><summary>截图</summary>

    ![](install-ubuntu-22.png.jpg)

    </details>

19. 选择是否发送信息给 Cononical（Ubuntu 是该公司的项目），之后点击`前进`

    <details markdown="1" ><summary>截图</summary>

    ![](install-ubuntu-23.png.jpg)

    </details>

20. 准备就绪，点击`完成`
    
    <details markdown="1" ><summary>截图</summary>

    ![](install-ubuntu-24.png.jpg)

    </details>

21. 若出现提醒更新的窗口，点击`稍后提醒`即可

    <details markdown="1" ><summary>截图</summary>

    ![](install-ubuntu-25.png.jpg)

    </details>

## 最终效果

1. 按 `Ctrl+Alt+T`，屏幕上会出现一个终端

    <details markdown="1" ><summary>截图</summary>

    ![](install-neofetch-1.png.jpg)

    </details>

2. 输入 `sudo apt update`（注意**其中的空格不可省略，单词不要拼错**）并按回车键，之后若看到输入密码的提示请输入之前设定的密码（注意输入过程中密码不会回显出来，即输入过程中看不到任何变化）并按回车键
   
    <details markdown="1" ><summary>截图</summary>

    ![](install-neofetch-2.png.jpg)

    ![](install-neofetch-3.png.jpg)

    </details>

3. 等上个指令运行完毕（绿色的字符重新出现，如截图中的 `hagb@hagb-VirtualBox` 字样）后，输入 `sudo apt install neofetch` 并回车，看到`您希望继续执行吗`的询问后按 `y` 并回车以确认

    <details markdown="1" ><summary>截图</summary>

    ![](install-neofetch-4.png.jpg)

    </details>

4. 等上个指令执行完后，输入 `neofetch` 并回车，即可见到文章开头的效果：

    ![](install-neofetch-7.png.jpg)

## 附录 1：分辨率过低导致安装界面显示不全的解决方法

TODO

## 参考文献

[^ubuntu-release]: 上面推荐的 20.04 是从中选择了最近的一个 LTS（长期支持）版本 https://ubuntu.com/about/release-cycle

[^ubuntu-require]: Ubuntu 的系统要求 https://help.ubuntu.com/community/Installation/SystemRequirements

[^efi]: legacy 模式下屏幕分辨率可能需要手动调节以便将安装界面显示完全 https://stackoverflow.com/questions/57115746/virtualbox-screen-resolution-too-small-during-installation