# vivo
HTC Incredible S CM12.1

主要使用szezso移植的工程

此处主要说明从环境搭建到最终编译的详细步骤

1、ubuntu编译环境搭建<br>
卸载默认安装的java版本:<br>
sudo apt-get purge openjdk-\* icedtea-\* icedtea6-\*

安装JDK1.7:<br>
sudo apt-get install openjdk-7-jdk

安装依赖包：<br>
sudo apt-get install git gnupg ccache lzop flex bison gperf build-essential zip curl zlib1g-dev zlib1g-dev:i386 libc6-dev lib32ncurses5-dev x11proto-core-dev libx11-dev:i386 libreadline6-dev:i386 lib32z1-dev libgl1-mesa-glx:i386 libgl1-mesa-dev g++-multilib  tofrodos python-markdown libxml2-utils xsltproc libreadline6-dev lib32readline-gplv2-dev libncurses5-dev bzip2 libbz2-dev libbz2-1.0 libghc-bzlib-dev squashfs-tools pngcrush schedtool dpkg-dev

2、下载代码<br>
curl https://storage.googleapis.com/git-repo-downloads/repo > bin/repo<br>
chmod a+x bin/repo<br>
mkdir -p cm12.1<br>
cd cm12.1<br>
repo init -u git://github.com/CyanogenMod/android.git -b cm-12.1<br>
mkdir -p .repo/local_manifests<br>
git clone https://github.com/zhangs0211/vivo.git .repo/local_manifests<br>
repo sync -j8<br>


编辑repo文件，替换默认的代码源<br>
REPO_URL = ‘https://gerrit.googlesource.com/git-repo‘<br>
修改为<br>
REPO_URL = ‘git://aosp.tuna.tsinghua.edu.cn/android/git-repo‘<br>

替换.repo/manifest.xml中已有的AOSP源代码的remote<br>
<remote name="aosp" <br>
fetch=".." <br>
review="https://android-review.googlesource.com/" /><br>
修改为<br>
<remote name="aosp" <br>
fetch="git://aosp.tuna.tsinghua.edu.cn/android/" <br>
review="https://android-review.googlesource.com/" /><br>

下载repo失败的话，可以使用国内的路径<br>
git://aosp.tuna.tsinghua.edu.cn/android/git-repo.git/

同样的如果需要更像Android SDK可以修改使用代理<br>
- 启动 Android SDK Manager ，打开主界面，依次选择「Tools」、「Options...」，弹出『Android SDK Manager - Settings』窗口；
- 在『Android SDK Manager - Settings』窗口中，在「HTTP Proxy Server」和「HTTP Proxy Port」输入框内填入mirrors.neusoft.edu.cn和80，并且选中「Force https://... sources to be fetched using http://...」复选框。设置完成后单击「Close」按钮关闭『Android SDK Manager - Settings』窗口返回到主界面；
- 依次选择「Packages」、「Reload」。

3、编译代码<br>
. build/envsetup.sh && brunch vivo



编译环境相关配置<br>
<html>
    <title>#!/bin/bash<br></title>
    <title>export PATH=/cyanogenmod/bin:$PATH<br></title>
    <title>export PATH=/cyanogenmod/android-sdk/platform-tools:$PATH<br></title>
    <title>export PATH=/usr/local/bin:$PATH<br></title>
    <title>export LC_CTYPE=C<br></title>
    <title>export LANG=C<br></title>
    <title>export USE_CCACHE=1<br></title>
    <title>export CCACHE_DIR=/cyanogenmod/.ccache<br></title>
</html>
设置ccache大小
prebuilts/misc/linux x86/ccache/ccache -M 20G
使用下面命令可以观察ccache使用情况
watch -n1 -d prebuilts/misc/linux-x86/ccache/ccache -s


4、编译过程中会遇到部分错误，解决方法如下
下载patch文件
git clone https://github.com/zhangs0211/htc_vivo_patch.git
注：由于repo无法直接使用补丁文件，此处需要使用git回复patch，或是手动修改





