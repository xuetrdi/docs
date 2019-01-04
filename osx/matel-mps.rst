=========================
Metal Performance Shaders
=========================

Metal Performance Shaders framework supports functionality
----------------------------------------------------------

- 将高性能的过滤器应用于图像从图像中提取统计和直方图数据
- 机器学习神经网络的训练和推断
- 求解方程组,分解矩阵,矩阵和向量乘法
- 高性能光线几何相交测试加速光线追踪

Fundamentals(基本原理)
----------------------

  MPSKernel Class是所有Matel Performance Shaders kernels的基类：定义了所有内核类的基线行为，声明设备运行内核,一些调试选项和用户友好标签。
  
  派生子类MPSUnaryImageKernel和MPSBinaryImageKernel子类：定义了大多数图像处理内核(过滤器)的共享行为，例如边缘模式，剪切和平铺支持，用于消耗一个或两个源纹理的图像操作。
  这些类都不是直接使用的。它们只提供抽象API，在某些情况下可能允许图像内核对象进行某种程度的多态操作。

  MPSUnaryImageKernel和MPSBinaryImageKernel类的子类提供专门初始化和编码功能,以将各种图像处理原语编码到命令缓冲区。

  许多图像过滤器可用：

  - Convolution filters(Sobel, Gaussian)
  - Morphological operators(dilate, erode):形态学运算操作(扩张,腐蚀)
  - Histogram operators(equalization,specification)：直方图操作(均衡,规范)

  所有的Texture(纹理)和buffer objects(缓冲区对象)都直接运行在GPU上。

  MPSKernel，MPSUnaryImageKernel，MPSBinaryImageKernel类都是用于将多种图像操作统一为简单一致的接口和调用序列以应用在图像过滤器，，子类实现了与规范不同的细节。

  使用内核子类的整体顺序保持不变。

  1. 查询硬件设备是否支持Metal Performance Shaders框架: `MPSSupportsMTLDevice(_:)`
  2. 分配常用的Metal对象来驱动Metal计算管道:MTLDevice,MTLCommandQueue,MTLCommandBuffer
  3. 创建适当的内核操作，例如如果要进行高斯模糊，则创建MPSImageGanssianBlur对象，不能同时被多个线程使用，如果需要多个线程的Metal请创建多个线程
  4. 调用内核的编码方法`endEncoding()`，编码调用的参数因内核类型而已，但操作类似。它们创建命令编码器，编写命令以将内核运行到命令缓冲区，然后结束命令编码器必须在当前命令编码器上调用endEncoding()管理内存
  5. 如果希望在命令缓冲区编码自己的其它命令，则必须创建一个新的命令编码器来执行此操作。
  6. 完成命令缓冲区后,使用`commit()`方法提交给设备。


  每个内核都针对特定的设备进行分配。单个内核不能与多个设备一起使用。这是必要的,因为`init(device:)`方法有时会分配缓冲区和纹理保存作为参数传递给初始化的数据，并且设备需要分配它们。
  内核提供了一个`copy(with:device)`方法允许它们复制到新设备中。

  内核对象不完全是线程安全的。虽然它们可能在多线程上下文中使用，如果尝试在多个内核对象同时写入同一缓冲区。
  在这方面，它们与命令编码器共享限制。在有限情况下，可以使用相同的内核同时写入多个命令缓冲区。但是只有将内核视为不可变对象时才有效。
  也就是说，如果共享内核属性发生了更改，那么更改可以反映在另一个线程上，而另一个线程正在对其工作编码，从而导致未定义的行为。

Device Support
--------------

.. code-block:: swift

    func MPSSupportsMTLDevice(_ device: MTLDevice?) -> Bool

    // args: device 
    // return: bool (ture, false)

Image Filters
-------------

  MPSUnaryImageKernel和MPSBinaryImageKernel基类定义了所有图像内核公有的几个属性：

  1. clipRect：剪辑矩形可用于写入目标纹理的所有图像内核。
  2. offset：所有使用源纹理的图像内核都可以使用偏移量,从中读取像素数据。
  3. edgeMode：边缘模式描述了偏离源图像边缘的纹理读取的行为。

  Supported Pixel Formats for Image Kernel：

  1. 1个8位无符号整数格式：MTLPixelFormat.r8Unorm,MTLPixelFormat.r8Unorm_srgb
  2. 2个8位无符号整数格式：MTLPixelFormat.rg8Unorm,MTLPixelFormat.rg8Unorm_srgb
  3. 4个8位无符号整数格式：MTLPixelFormat.rgba8Unorm,MTLPixelFormat.rgba8Unorm_srgb,MTLPixelFormat.bgra8Unorm,MTLPixelForm.bgra8Unorm_srgb
  4. 1个16位浮点格式：    MTLPixelFormat.r16Float,MTLPixelFormat.rg16Float,MTLPixelFormat.rgba16Float
  5. 1个32位浮点格式：    MTLPixelFormat.r32Float,MTLPixelFormat.rg32Float,MTLPixelFormat.rgba32Float
  6. 1个16位无符号整数格式：MTLPixelFormat.r16Unorm,MTLPixelFormat.rg16Unorm,MTLPixelFormat.rgba16Unorm
  7. 1个16位无符号整数颜色分量: MTLPixelFormat.b5g6r5Unorm,MTLPixelFormat.a1bgr5Unorm,MTLPixelFormat.abgr4Unorm,MTLPixelFormat.bgr5A1Unorm
  8. 1个32位无符号整数颜色分量：MTLPixelFormat.rgb10a2Unorm
  9. 1个32位浮点颜色分量：MTLPixelFormat.rg11b10Float,MTLPixelFormat.rgb9e5Float

  Metal Performance Shaders图像内核还支持具有普通符号和无符号整数像素格式的源和目标
  
  - MPSImageTranspose
  - MPSImageIntegral
  - MPSImageIntegralOfSquares

Neural Networks
---------------

  训练深度神经网络

  - MPSImage：在卷积神经网络中可能具有四个以上通道纹理
  - MPSTemporaryImage：用于卷积神经网络的纹理，用于存储要立即使用和丢弃的瞬态数据。
  - Objects that Simplify the Creation of Neural Networks：简化神经网络创建对象
  - Convolution Neural Network kernels：构建深度网络层
  - Recurrent Neural Networks：循环神经网络

MPSImage
""""""""
  
  .. code-block:: swift
  
      class MPSImage : NSObject

  MTLTexture对象不能具有多于4个通道。因此附加通道存储在2D纹理阵列的切片中，使得4个通道存储在该阵列的每个切片中。
  如果特征通道的数量是N,则所需的阵列切片的数量是：(N+3)/4，例如宽度为3,高度为2的9通道CNN图像将如下方式存储。

  MPSImage对象可以包含多个CNN图像进行批处理。要创建包含N个图像的MPSImage对象,请创建一个numberOfImages属性设置为N的MPSImageDescriptor对象。

  2D纹理阵列的长度(切片的数量) = ((featureChannels + 3) \* 4) \* numberOfImages，其中(featureChannels + 3) / 4切片表示一个图像

  MPSImage对象可以包含多个图像，但是MPSCNNKernel对象处理的图像中的实际图像数量由clipRect属性的z维度控制。n = clipRect.size.depth

  

Matrices and Vectors
--------------------


Ray Tracing
-----------


KernelBase Classes
------------------


Keyed Archivers
---------------


Classes
-------
