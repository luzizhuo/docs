##Docker and IPv6 on Ubuntu 13.10   Saucy  Salamander
##在Ubuntu 13.10 上的Docker and IPv6  Saucy  Salamander

***

After making myself familiar with Docker I wanted to use it on a more recent Ubuntu 13.10 release. I still didn't install Docker natively on my pc, but use a Vagrant box to play with fresh releases. After downloading a fresh Ubuntu 13.10 image, creating a VirtualBox image and installing the current Docker release 0.8.0, I tried to use my other little toys CouchDB and Elasticsearch in Docker containers. Sadly, I couldn't connect to the exposed ports anymore.

随着我对 Docker 的渐渐熟悉，我想把它用在最新发布的 Ubuntu 13.10 上。现在我仍然没有将 Docker Native  安装在我的个人电脑上，但是我经常用 Vagrant 去尝试最新的版本。在我下载了最新版的 Ubuntu 的镜像文件之后，我创建了一个 VirtualBox 的镜像并且安装了 Docker0.8.0。我试着将我的另一些小玩意儿 CouchDB 和 Elasticsearch 在 Docker containers 上运行，不一会儿外设就再也没法儿用了。

***

  The Docker installation docs for Ubuntu Linux 13.04 and 13.10 mention the configuration of UFW to modify the DEFAULT_FORWARD_POLICY to accept all traffic. Well, the UFW wasn't enabled. So the usual digging began, searching on StackOverflow, GitHub and developer blogs. Most hints mentioned disabling the IPv6 support via sysctl, but a quite clear statement of Jérôme Petazzoni at a StackOverflow answer made me search for alternatives.

  在Linux 13.04和13.10版本上的安装文件提到可以通过修改UFW中DEFAULT_FORWARD_POLICY配置来接受全部流量。不过UFW没法儿用，我照常在 StackOverflow，GitHub还有开发者博客上寻找帮助。多少人提到无法通过IPv6设置系统核心参数，不过一个叫做Jérôme Petazzoni的在StackOverflow上的回答让我找到了一个替代品。

  Instead of disabling IPv6 some people found a way to enable IPv6 inside of containers: Andreas Neuhaus showed some native lxc commands to make containers support IPv6. Marek Goldmann also shows some other use cases for native lxc commands to connect containers on multiple hosts. Well, hopefully a pull request to make Docker natively support IPv6 will merged soon. My problem didn't look like missing IPv6 support inside the container, though.
  卸载IPv6后一些人发现能使其工作的内在组件。Andreas Neuhaus告诉我们一些能使容器支持Ipv6的原始Ixc命令。Marek Goldmann也告诉我们另外一些在多个主机上使容器相连的Ixc命令。希望我的pullrequest 能够让Docker尽快支持Ipv6，尽管我的问题看起来并不像是丢失了支持Ipv6的内部组件。
  Some debugging, bridge configuration, iptables fun and interface setups later I came back to the sysctl config and had a look at the existing entries. You might by surprised how well each default entry is documented and especially one entry looked promising: I enabled IPv6 forwarding on all interfaces by uncommenting the entry with: net.ipv6.conf.all.forwarding=1 in the /etc/sysctl.conf. A quick reboot later finally showed a response from the dockerized CouchDB. Relax! Sometimes it's too easy.

  程序调试，桥式接线，ip信息包过滤系统还有接口在安装，一会儿我回来看系统配置条目。你或许会惊讶每一个非法进入，甚至看起来合法的进入都会被记录：我允许Ipv6使用所有接口用net.ipv6.conf.all.forwarding=1 在 /etc/sysctl.conf。一个快速启动很快就完成了，并通过dockerized CouchDB反馈回来。终于轻松了！有时事情还是很简单的。（感觉这段翻译的很糟糕）

  Now comes the fun part with reconnecting the CouchDB River Plugin for ElasticSearch over different containers again :)

  下面才是最有趣的部分，通过容器再次连接CouchDB River Plugin和ElasticSearch。

  Tobias Gesellchen
  Share this post

  分享人：托拜厄斯  
  Saucy  Salamander 
