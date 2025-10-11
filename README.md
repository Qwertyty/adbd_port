文件：study_record-master.zip

## 配置交叉编译工具

export ARCH=arm64
export CROSS_COMPILE=aarch64-linux-gnu-
export PATH=$PATH:/opt/gcc-linaro-7.5.0/bin



## 交叉编译zlib

tar -zxvf zlib-1.2.11.tar.gz

cd zlib-1.2.11 && mkdir build

CC=aarch64-linux-gnu-gcc ./configure --prefix=/home/qwerty/Desktop/study_record-master/zlib-1.2.11/build

make -j8

make install



## 交叉编译openssl

tar -xvf openssl-1.1.1f.tar.gz

cd openssl-1.1.1f/ && mkdir build

./Configure linux-aarch64   shared no-asm --prefix=$(pwd)/build --cross-compile-prefix=aarch64-linux-gnu-

make -j8

make install



## 交叉编译adbd

cd ./study_record-master/android-tools-4.2.2/core/adbd

vi Makefile

#设置交叉编译工具

CC:=aarch64-linux-gnu-gcc

#设置zlib openssl 头文件与库搜索路径

CPPFLAGS+= -I/home/qwerty/Desktop/study_record-master/openssl-1.1.1f/build/include
CPPFLAGS+= -I/home/qwerty/Desktop/study_record-master/zlib-1.2.11/build/include

LDFLAGS+= -L/home/qwerty/Desktop/study_record-master/zlib-1.2.11/build/lib
LDFLAGS+= -L/home/qwerty/Desktop/study_record-master/openssl-1.1.1f/build/lib





将交叉编译的zlib与openssl的库拷贝到设备的/usr/lib，或者在设备端设置环境变量export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/mnt/flash/arm_soft/zlib/lib
/mnt/flash/arm_soft/zlib/lib为你动态库实际想要放置设备的路径。



## 设备树修改

usb_dwc3_0 {
        dr_mode = "peripheral";

​	......

}
