![Darknet Logo](http://pjreddie.com/media/files/darknet-black-small.png)

# Darknet #
Darknet is an open source neural network framework written in C and CUDA. It is fast, easy to install, and supports CPU and GPU computation.

For more information see the [Darknet project website](http://pjreddie.com/darknet).

For questions or issues please use the [Google Group](https://groups.google.com/forum/#!forum/darknet).

***
## Darknet with NNPACK for Raspberry Pi3

### digitalbrain79版の Darknet with NNPACKの NNPACK処理を最新版の Darknetに適用
http://www.neko.ne.jp/~freewing/raspberry_pi/raspberry_pi_update_darknet_nnpack_neural_network_framework/

FREEWING-JP/NNPACK  
https://github.com/FREEWING-JP/NNPACK/tree/FREEWING_20180827

FREEWING-JP/darknet  
https://github.com/FREEWING-JP/darknet/tree/FREEWING_20180828

pi@raspberrypi:~/pytorch $ uname -a  
Linux raspberrypi 4.14.50-v7+ #1122 SMP Tue Jun 19 12:26:26 BST 2018 armv7l GNU/Linux

***
## # Build
###### # お決まりの sudo apt-get updateで最新状態に更新する
sudo apt-get update

###### # #
sudo apt-get -y install ninja-build

sudo pip install --upgrade git+https://github.com/Maratyszcza/PeachPy  
sudo pip install --upgrade git+https://github.com/Maratyszcza/confu

###### # NNPACK-darknet
cd  
mkdir fw  
cd fw  
###### # Patch digitalbrain79 Add bias when bias is not NULL
git clone https://github.com/FREEWING-JP/NNPACK.git  
cd NNPACK

###### # #
confu setup  
python ./configure.py --backend auto

###### # 2コアでビルドで 10分
ninja -j2

###### # #
sudo cp -a lib/* /usr/lib/
###### # */
sudo cp include/nnpack.h /usr/include/  
sudo cp deps/pthreadpool/include/pthreadpool.h /usr/include/  

###### # darknet-nnpack
cd  
cd fw  
###### # Patch digitalbrain79 Darknet with NNPACK diff
git clone https://github.com/FREEWING-JP/darknet.git  
cd darknet  

###### # 2コアでビルドで 1分 12秒
make -j2  

***
#### # Nightmare 悪夢
###### # jnet-conv.weights (72MB)
wget http://pjreddie.com/media/files/jnet-conv.weights  
###### # wget https://raw.githubusercontent.com/pjreddie/darknet/master/cfg/jnet-conv.cfg
###### # mv jnet-conv.cfg ./cfg/

./darknet nightmare cfg/jnet-conv.cfg jnet-conv.weights data/horses.jpg 11 -rounds 4 -range 3

./darknet nightmare cfg/jnet-conv.cfg jnet-conv.weights data/dog.jpg 11 -rounds 4 -range 3

./darknet nightmare cfg/jnet-conv.cfg jnet-conv.weights data/eagle.jpg 11 -rounds 4 -range 3

###### # vgg-conv.weights (56M)
wget http://pjreddie.com/media/files/vgg-conv.weights  
./darknet nightmare cfg/vgg-conv.cfg vgg-conv.weights data/scream.jpg 10  

***
#### # Object Detection 物体検出
###### # yolov2.weights (194MB)
wget https://pjreddie.com/media/files/yolov2.weights  
./darknet detector test cfg/coco.data cfg/yolov2.cfg yolov2.weights data/dog.jpg  

###### # yolov2-tiny-voc.weights (61MB)
wget https://pjreddie.com/media/files/yolov2-tiny-voc.weights  
./darknet detector test cfg/voc.data cfg/yolov2-tiny-voc.cfg yolov2-tiny-voc.weights data/dog.jpg  
