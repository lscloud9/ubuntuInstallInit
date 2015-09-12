# 安装ros的Ubuntu脚本

为了方便自己安装ros，我写了一个脚本，这个脚本可以根据用户Ubuntu的版本进行选择性的安装ROS版本，如14.04可以安装indigo或者是jade，但是如果你用德是14.10或者是15.04，你就只能安装jade。当你的Ubuntu版本是14.04时，脚本会提示你请你输入你需要安装的ros版本，你可以输入jade或者是indigo，输入是忽略大小写的。

** 声明：我只针对64位的Ubuntu进行了测试，希望有时间和经历的朋友测试下32位版本的**


** 我的代码如下 **

```
#!/bin/bash
# 14.04 -> indigo or jade
# 14.10 15.04 -> jade
# create by Cloud9

function InstallRos()
{
	sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

	sudo apt-key adv --keyserver hkp://pool.sks-keyservers.net --recv-key 0xB01FA116

	sudo apt-get update

	sudo apt-get install "ros-$RosVersion-desktop-full" -y

	sudo rosdep init
	rosdep update

	echo "source /opt/ros/$RosVersion/setup.bash" >> ~/.bashrc
	source ~/.bashrc

	sudo apt-get install python-rosinstall -y
}
function InstallIndigo ()
{
	sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

	sudo apt-key adv --keyserver hkp://pool.sks-keyservers.net --recv-key 0xB01FA116

	sudo apt-get update

	sudo apt-get install ros-indigo-desktop-full -y

	sudo rosdep init
	rosdep update

	echo "source /opt/ros/indigo/setup.bash" >> ~/.bashrc
	source ~/.bashrc

	sudo apt-get install python-rosinstall
}

function InstallJade ()
{
	sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

	sudo apt-key adv --keyserver hkp://pool.sks-keyservers.net --recv-key 0xB01FA116

	sudo apt-get update

	sudo apt-get install ros-jade-desktop-full -y

	sudo rosdep init
	rosdep update

	echo "source /opt/ros/jade/setup.bash" >> ~/.bashrc
	source ~/.bashrc

	sudo apt-get install python-rosinstall
}


# check os version
if cat /etc/lsb-release | grep -i "\b 14.04 \b" >/dev/null 2>&1
	then
	OsVersion="14.04"
	echo "u Os Version is $OsVersion"
elif cat /etc/lsb-release | grep -i "\b 14.10\b" >/dev/null 2>&1
	then
	OsVersion="14.10"
	echo "u Os Version is $OsVersion"
elif cat /etc/lsb-release | grep -i "\b 15.04\b" >/dev/null 2>&1
	then
	OsVersion="15.04"
	echo "u Os Version is $OsVersion"
else
	OsVersion=OsVersion=$(cat /etc/issue)
	echo "u Os Version is $OsVersion"
	echo "i suggest u to choose your os to Ubuntu 14.04,14.10 or 15.04"
	return 1
fi



# choose ros version
if [ "$OsVersion" == "14.04" ]
	then
	echo -n "Your os version is $OsVersion,please input Jade or Indigo u want to install:"
	read RosVersion
	RosVersion=$(echo "$RosVersion" | tr A-Z a-z);
	echo "Your choose is $RosVersion"
else
	RosVersion="jade"
	echo "Your choose is $RosVersion"
fi

# check ros version
if [ "$RosVersion" == "indigo" ]
	then
	echo "Your choose is $RosVersion"
elif [ "$RosVersion" == "jade" ]
	then
	echo "Your choose is $RosVersion"
else
	echo "Your choose is $RosVersion is not availavle"
	return 1
fi

# install Ros
InstallRos
```

下面是测试的结果图片
** 14.04indigo **
![14.04indigo](https://github.com/lscloud9/ubuntuInstallInit/raw/master/image/14.04indigo.jpg)

** 14.10jade **
![14.10jade](https://github.com/lscloud9/ubuntuInstallInit/raw/master/image/14.10jade.jpg)

** 14.04jade **
![14.04jade](https://github.com/lscloud9/ubuntuInstallInit/raw/master/image/14.04jade.jpg)

** 15.04jade **
![15.04jade](https://github.com/lscloud9/ubuntuInstallInit/raw/master/image/15.04jade.jpg)



![image]()


包宿测试的，希望大家用用，遇到什么问题可以给我留言。
针对Ubuntu安装，我还做了个更全的软件安装脚本，包括我常用的软件。
这是我第一次用markdown进行编辑。如果排版不够精致，请见谅！

by Cloud9