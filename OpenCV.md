布局上，我认为那个4h视频讲的很好，于是就将代码融入实操来理解

在编写源代码之前

```plain
//cmake_minimum_required(VERSION 3.10.0)
//project(vscode_opencv VERSION 0.1.0 LANGUAGES C CXX)

//add_executable(vscode_opencv main.cpp)
find_package(OpenCV REQUIRED)
target_link_libraries(vscode_opencv ${OpenCV_LIBS})
//include(CTest)
//enable_testing()
//set(CPACK_PROJECT_NAME ${PROJECT_NAME})
//set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
//include(CPack)

```

我先用vscode上的cmake：quick start 快速写了个cmakelists.txt

之后就是增加代码链接opencv库（不带//的）





```plain
#include <iostream>
#include <opencv2/imgcodecs.hpp>
#include <opencv2/highgui.hpp>
#include <opencv2/imgproc.hpp>
using namespace cv;
using namespace std;


//////图片生成////////

 int main(){
     string path = "./picture/num3.jpeg";
     Mat img = imread(path);
     imshow("Image",img);
     waitKey(0);
     return 0;
 }

/////////////////视频//////////////////

 int main(){
     string path = "picture/num5.mp4";
     VideoCapture cap(path);
     Mat img;
    
    while(true){
         cap.read(img);

   
         imshow("Image",img);
         waitKey(100);
     }
     return 0;

 }


/////////////////摄像头//////////////////

 int main(){
     
     VideoCapture cap(0);
     Mat img;
     bool isrunning = true;
     while(isrunning){
         cap.read(img);

   
         imshow("Image",img);
         if(waitKey(1) == 27){
            isrunning = false;
         }
     }
     return 0;
}
```

为了方便，我暂时先用了全局命名空间cv

上面的代码是我从视频教程中学的三个基本方式

打开图片-----------打开视频--------打开摄像头

在这里我们需要读取图片或视频时，需要有一个途径

而     string path = "picture/num5.mp4";（我把图片文件直接放到目录下了）

Mat就是初始化定义的类型了imread读取路径（可加上额外操作，后续分析代码时再说），imshow显示出名字为Image的num3的图片

对于视频，则是额外加一个  cap.read(img);（相当于读取每一帧来显示）

而摄像头就不需要路径了    VideoCapture cap(0);直接获取

![](https://cdn.nlark.com/yuque/0/2025/png/61445279/1761436175896-62149e03-eab8-4a81-9109-2a923b329646.png)

![](https://cdn.nlark.com/yuque/0/2025/png/61445279/1761436345353-4ca37021-3104-4999-bfac-a630cf65a4b9.png)

![](https://cdn.nlark.com/yuque/0/2025/png/61445279/1761436478232-0af314f7-4b8f-440f-896c-db35a611f22e.png)

图片、视频、摄像头的效果展示



不过在进行c++运行的时候，程序一直报错，说是检测不到头文件opencv

![](https://cdn.nlark.com/yuque/0/2025/png/61445279/1761436580342-97f647e3-8804-4015-b07e-666da08d2330.png)

经过我的不断调试，我发现还是不行，opencv已经安装，路径也是默认路径，而且cmakelists.txt也链接了，最终，我的室友帮我出谋划策，让我直接打开终端，用cmake .         make       ./vscode_opencv    换个方式运行

果真成功，他说是直接右上角运行c++程序时容易忽略cmakelists.txt



<h3 id="GucTP">基本功能</h3>
先让我把命令呈现出来

```plain
int main(){
    string path = "picture/num6.jpeg";
    Mat img = imread(path);
    Mat imggray, imgblur, imgcanny ,imgdil, imgerode; 
    cvtColor(img, imggray,COLOR_BGR2GRAY);
    GaussianBlur(img,imgblur,Size(3,3),3,0);
    Canny(imgblur,imgcanny,25,75);
 
    erode(imgdil,imgerode,kernel);
    
    imshow("Image",img);
    imshow("Image2",imggray);
    imshow("Image3",imgblur);
    imshow("Image4",imgcanny);
    imshow("Image5",imgdil);
    imshow("Image6",imgerode);
    waitKey(0);
    return 0;
}
```



图片效果如下



![](https://cdn.nlark.com/yuque/0/2025/png/61445279/1761438993289-4a2b5d8f-cd51-40a8-ab5a-9d1eed889258.png)

![](https://cdn.nlark.com/yuque/0/2025/png/61445279/1761439007710-44a970e5-f186-4dbe-b698-c62185eb743b.png)

![](https://cdn.nlark.com/yuque/0/2025/png/61445279/1761439017720-64cab616-45da-4878-951f-a0762a4e4453.png)

![](https://cdn.nlark.com/yuque/0/2025/png/61445279/1761439022870-a5f16be7-c454-4137-84c8-d7c1616ebfe8.png)

![](https://cdn.nlark.com/yuque/0/2025/png/61445279/1761439031911-59919309-6c08-485c-933b-b0e0839c7470.png)

![](https://cdn.nlark.com/yuque/0/2025/png/61445279/1761439041966-e4c18830-b612-4fc8-97f1-3a9a3b8df8b8.png)

ok，这分别是原图、灰度图、模糊图、边缘显示图、强化边缘显示图、弱化边缘显示图（为了更好分辨形状 ，虽然有点好笑）

这些命令是对照片进行不同的操作，

cvtColor（原图，转换的图，COLOR_BGR2GRAY）用于变成灰度图，其他的参数COLOR_BGR2RGB/BGR2HSV/GRAY2BGR分别用于适配库/颜色的提取/灰度转BGR

GaussianBlur(原图，转换的图，Size(奇数，奇数），sigmax,sigmay)在源代码中是用于图像的模糊处理的，而这种滤波算法还可用于图像的增强，特征的提取等效果，也有medianBlur\blur.bilateralFilter等

Canny(原图，转换的图，num1,num2）将边缘信息突出，突出物体轮廓，两个参数指定阈值

dilate（原图，转化的图，结构元素）

erode（同上）

上面这两个须相关结构元素   Mat kernel =getStructuringElement(MORPH_RECT,Size(3,3));后面的size参数即像素块的大小，前者填补目标图像的前景目标，连接断裂边缘，描粗边；后者是消除细小噪音，擦去边缘。

！！！！在这里运行时第一次成功编译却没照片，一定记得waitKey



<h3 id="ipd54">图片调节大小和裁减</h3>


```plain
int main(){
        string path = "picture/num6.jpg";
    Mat img = imread(path);
    Mat imgresize;
    resize(img,imgresize,Size(555,555));
    
    imshow("Image",img);
    imshow("Img",imgresize);
    waitKey(0);

    return 0;

}
```



![](https://cdn.nlark.com/yuque/0/2025/png/61445279/1761441444661-c0af369b-9409-4913-a818-64b5acce2d85.png)

（看起来如果我刚刚把照片的大小弄小一点，上面就不用五张图才能展示了，直接一张就行）

用resize（原图，转换的图，Size（宽度，长度））操作

如果想要等比例收缩或扩张，则Size(),0.5,0.5------缩小一半



```plain
int main(){
        string path = "picture/num6.jpg";
    Mat img = imread(path);

    Mat imgcrop;

    Rect roi(400,400,500,300);
    
    imgcrop = img(roi);

    imshow("Image",img);
    imshow("img",imgcrop);
    waitKey(0);

    return 0;

}
```

这个是裁减

![](https://cdn.nlark.com/yuque/0/2025/png/61445279/1761442501470-8db7e7ef-dd30-426b-9615-b287e4c5ac9a.png)

我也是废了很大的劲才剪到这张嘴（裁减时不能超出范围，否则无法生成）

roi指内部的参数（宽度起始点，长度起始点，矩形宽度，矩形长度）



<h3 id="Gwlqw">绘制形状和文字</h3>
```plain
int main(){
    Mat img(512,512,CV_8UC3,Scalar(255,0,0));
    imshow("img",img);
    waitKey(0);

    return 0;

}
```

![](https://cdn.nlark.com/yuque/0/2025/png/61445279/1761443047794-a0dd2c80-e76d-47ab-8e57-711755e95564.png)

这里是定义一个空白的img并且给定他的大小，CV_8UC3指的是8个二进制与彩色，Scalar是定义成标量，后面三个数字即蓝绿红；

```plain
int main(){
    Mat img(512,512,CV_8UC3,Scalar(255,0,0));
    circle(img,Point(256,256),155,Scalar(0,0,255),FILLED);
    rectangle(img,Point(133,229),Point(388,289),Scalar(0,255,0),FILLED);
    putText(img , "hello,world",Point(137,262),,FONT_HERSHEY_COMPLEX_SMALL,2Scalar(255,255,255),2);
    
    imshow("img",img);
    
    waitKey(0);
    return 0;
}
```

用于生成一个自定义的img以及在其中生成图形和文字

![](https://cdn.nlark.com/yuque/0/2025/png/61445279/1761453954211-a451379a-8a5a-40aa-9dd9-e1b2b6733e47.png)

相关的命令就如上面的代码块所示，简单得加一些参数即可



<h3 id="m9d3V">翘曲透视</h3>
```plain
float w=250,h=350;
int main(){
    string path = "picture/num3.jpeg";
    Mat img = imread(path);
    Mat matrix,imgp;
    Point2f scr[4] = { {229,142},{671,190},{405,395},{674,457} };
    Point2f des[4] = { {0.0f,0.0f},{w,0.0f},{0.0f,h},{w,h} };
    matrix = getPerspectiveTransform(scr,des);
    warpPerspective(img,imgp,matrix,Point(w,h));
    imshow("Img",img);
    imshow("omgp",imgp);
    waitKey(0);
    return 0;
```

![](https://cdn.nlark.com/yuque/0/2025/png/61445279/1761460385927-0217d8aa-d611-4f76-829f-6a37be6370f1.png)

这个确实有点难看，主要是我这个像素点的位置很难调，所以现掌握方法就行了了，就算是把四个倾斜的点围城的矩形转换为其他形式的矩形



<h3 id="UrmO5">颜色检测</h3>
```plain
Mat imghsv,mask;
int hmin =0 ,smin =110,vmin =153;
int hmax =19,smax =240,vmax =255;
int main(){
    string path ="picture/num6.jpg";
    Mat img =imread(path);
    cvtColor(img,imghsv,COLOR_BGR2HSV);
    Scalar lower(hmin,smin,vmin);
    Scalar upper(hmax,smax,vmax);
    inRange(imghsv,lower,upper,mask);
    imshow("img",img);
    imshow("img2",imghsv);
    imshow("imh3",mask);
    waitKey(0);
    return 0;
}
```

提前说一下，由于lower和upper都是我自己随便弄的，所以接下来的图片会有点奇怪

![](https://cdn.nlark.com/yuque/0/2025/png/61445279/1761461940155-6fa9a146-82d0-4e40-b8f0-cfb74063eaa3.png)

就是把原图转换成hsv类型的，以便更好监测，接下来可以用lower和upper进行遮掩，范围内的为白色像素，其余为黑色

总之这是用于寻找颜色的方式



<h3 id="HmoHc">形状/轮廓检测</h3>
```plain
vector<vector<Point>> red_contours, green_contours;
        findContours(red_mask, red_contours, RETR_EXTERNAL, CHAIN_APPROX_SIMPLE);
        findContours(green_mask, green_contours, RETR_EXTERNAL, CHAIN_APPROX_SIMPLE);
```

我直接用任务里的代码来进行概述,不再用熊大了

这就是先声明两个容器,就是用来存储红色区域的轮廓和绿色区域的轮廓的,以及后续的用于判定像素面积范围用到的



整个opencv的大体命令和基本操作类型都在上面的一个个实操中,不用再一个个写出慢慢分析,可以系统化的理解和应用.











