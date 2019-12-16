采取的实验数据格式为VOC的格式,标签存储在xml文件中.代码中涉及一些对.xml文件的基本操作
文件树：
----
  Annotation
    ----xx.xml
    ...
    ----xxx.xml
  JPEGImages
   ----xx.jpg
    ...
    ----xxx.jpg
  
# DataAugmentation_ForObjectDetect
本仓库主要包含了针对目标检测数据集的增强手段和源码：图像的旋转，镜像，裁剪，亮度/对比度的变换等
## rotated.py包含了旋转图像以及对应的标签的旋转代码
对图像的旋转可以调用cv2的库函数，但是对标签的操作需要搞清楚一个点绕着另一个点旋转后的坐标与原坐标的对应关系，计算公式见代码！

## padd_rotated_crop.py
> 由于图像的旋转会使得一部分信息损失，而且不是很容易的判断旋转后的图像是否还包含我们的完整目标。
因此可以尝试把原图像先安装长短边的长度填充为一个正方形，这样可以确保填充后的图像在旋转过程中，原图中的目标信息不会丢失。
然后可以旋转作一个裁剪（根据目标的标签坐标，只要避开旋转后的标签坐标就能很好的裁剪出图像），当然也可以不裁剪，但是这样会降低模型
训练的效率

## color.py 目前只是对图像的亮度/对比度的改变
> 该部分代码还应该有对图像的颜色变换，下次更新！对亮度和对比度的调节利用的是像素的简单线性变换。

## mirror.py
> 该部分代码主要是对图像进行水平，竖直，对角的镜像变换
对图像的操作cv2.flip()；对标签的变化其实就是利用原图的长宽来减去原像素，三种情况有所不同，但很好理解，只是有些细节需要注意，见代码！
注：对原图的对角镜像其实等价于对原图旋转180°
