# Galaxy Camera Module

## 简介

大恒银河系列(Galaxy)工业相机驱动模块。
该模块将大恒C语言版本的GalaxySDK的基本控制方法封装为C++库。

## 使用说明

类*CameraDevice*对应一个物理相机设备，提供开启、关闭、设置曝光、增益、白平衡等操作。
应当保证整个系统中只有一个*CameraDevice*对象与单个相机对应；
由于这些*CameraDevice*共享同一个物理设备，故在一个实例中调用关闭方法，会导致所有实例都无法读取图片。

接口*AbstractAcquisitor*提供了基本事件接口，实现该接口的类可以被注册在*CameraDevice*设备中，
当事件发生后，所有注册过的采集器的相应的方法都会被调用。
类*LambdaAcquisitor*使用Lambda表达式转发了*AbstractAcquisitor*中的事件，
从而允许用户使用Lambda表达式而不必选择继承来实现简单的处理操作。

类*RawPicture*描述了图片的基本信息：尺寸、大小、和内存地址。
根据图像的大小即字节数除以图像的像素点个数和单个像素点的字节数即可得到通道数。
其中，应当注意，大恒的官方相机驱动采用交换链的方式存储图片，
这意味着，存储某一阵图像的地址会在数帧后被重复利用，
若对图片的处理时间较长，以至于超过该重复利用周期，则处理图片前应当将图片内存拷贝，
以避免用户线程和相机驱动线程同时读写该内存区域而导致访问冲突。

## 依赖项

- GalaxySDK，大恒银河系列开发工具，来自[大恒图像官网](https://daheng-imagine.com)。
