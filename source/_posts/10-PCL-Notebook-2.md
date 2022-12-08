---
title: 10-PCL_Notebook_2
tags:
  - PCL
cover: >-
  https://github.com/Monaxi/Mona-study/blob/main/blog/wallhaven-wqgr9x_1920x1080.png?raw=true
top_img: >-
  https://github.com/Monaxi/Mona-study/blob/main/blog/default_top_img.png?raw=true
date: 2022-07-09 15:41:42
description:
---

# 第三章 PCL数据读取与可视化

## PCL点云存储格式

支持的格式有：.pcd .ply .obj .xyz(ascii) .vtk .png .tif

各个格式的头文件信息如下：

```头文件信息
VERTION 0.7								//指定PCD文件版本
FIELDS x y z							//指定一个点的维度和字段的名字，此处表示点云维度是XYZ
SIZE 4 4 4								//用字节数指定每一个维度的大小
TYPE F F F
COUNT 1 1 1
WIDTH 6411
HEIGHT 1
VIEWPOINT 0 0 0 1 0 0 0
POINTS 6411
DATA ascii
```

## PCL编程规范

### 命名规范

#### 文件命名

所有文件名单词之间用下划线隔开，头文件扩展名为.h，模板类实现文件的扩展名为.hpp，源文件的扩展名为.cpp

#### 目录命名

所有目录及其子目录命名，多个单词之间用下划线隔开，头文件应放在源码目录树中的include/下，模板类放在include/impl/下，源文件放在目录树中的src/下。

#### include语句

当文件在同一目录下，include指示语句用`""`，在其他情况下用`<>`

```cpp
#include <pcl/module_name/file_name.h>
#include <pcl/module_name/impl/file_name.hpp>
#include "file_name.cpp"
```

#### 宏定义命名

- 宏定义中字母采用大写格式，为头文件定义的宏后面还要加上下划线，且名称从include下目录开始

🌰  ：pcl/filters/bilateral.h  对应  PCL_FILTERS_BILATERAL_H_

- #ifndef 和 #define 定义放在BSD协议后面，代码前面；
- #endif 定义一直在文件结尾，并且加上一句注释掉的宏对应头文件的宏定义。

```cpp
// theBSD license
#ifndef PCL_MODULE_NAME_IMPL_FILE_NAME_HPP_//为避免重复包含头文件而定义的宏
#define PCL_MODULE_NAME_IMPL_FILE_NAME_HPP_
// the code
#endif // PCL_MODULE_NAME_IMPL_FILE_NAME_HPP_
```

#### 命名空间的命名

命名空间多于一个单词的之间要用下划线连接；

…更多参考《点云库PCL从入门到精通》

## PCL点类型及自定义

### PointT类型

对XYZ数据的简单操作：对带有SSE功能的处理器，最高效的方法是存储三维坐标为浮点型，然后紧跟着一个浮点型数据，作为填补位数以满足存储对齐的要求。

```cpp
struct PointXYZ{
  float x;
  float y;
  float z;
  float padding;
}
```

可用的PointT类型

- PointXYZ——成员变量：float x, y, z

  最常用的点数据类型，只包含xyz三维坐标信息，三个浮点数附加一个浮点数满足存储对齐，可利用`points[i].data[0]`或者`points[i].x`访问点的x坐标值

  代码如下

  ```cpp
  union{
    float data[4]
      struct{
        float x;
        float y;
        float z;
      };
  };
  ```

- PointXYZI——成员变量：float x, y, z, intensity（强度）

  XYZ坐标加intensity的point类型；

  理想情况：四个变量将新建单独一个结构体，满足存储对齐；

  现实情况：point大部分操作会把data[4]的元素设置成0或1（用于变换）不能让intensity和xyz在同一个结构体中，内容有被覆盖的可能性；

  对于兼容存储对齐，用三个额外的浮点数来填补intensity，虽然存储效率低，但是运行效率高且符合存储对齐的要求。

  ```cpp
  union{
    float data[4];
    struct{
      float x;
      float y;
      float z;
    };
  };
  union{
    struct{
      float intensity;
    };
    float datat_c[4]
  };
  ```

- PointXYZRGBA——成员变量：float x, y, z; unit32_t rgba;

  除了rbga信息被包含在一个整型变量中，其他和PointXYZI类似；

  ```cpp
  union{
    float data[4];
    struct{
      float x;
      float y;
      float z;
    };
  };
  union{
    struct{
      unit32_t rgba;
    };
    float data_c[4];
  };
  ```

- PointXYZRGB——成员变量：float x, y, z, rgb;

  除了rgb信息被包含在一个浮点型变量中，其他的和PointXYZTRGB类似；

- 简单的二维x-y point结构

  ```cpp
  struct{
    float x;
    float y;
  }
  ```

- InterestPoint——成员变量：float x, y, z, strength(表示关键点的强度测量值)

- Normal——：float normal[3], curvature;

  Normal结构体表示给定点所在的样本曲面上的发现方向，以及对应曲率的测量值——通过曲面块特征值之间的关系获得；可以通过`points[i].data_n[0]`、`points[i].normal[0]`或者`points[i].normal_x`访问法向量的第一个坐标；NOTE：曲率不能存在同一个结构体中，会被普通数据操作覆盖掉；

  ```cpp
  union{
    float data_n[4];
    float normal[3];
    struct{
      float normal_x;
      float normal_y;
      float normal_z;
    };
  };
  union{
    struct{
      float curvature;
    };
    float data_c[4];
  };
  ```

  

- PointNormal——成员变量：float x, y, z; float normal[3], curvature;

- PointXYZRGBNormal——成员变量：float x, y, z, rgb, normal[3], curvature;

- PointXYZINornal——成员变量：float x, y, z, intensity, normal[3], curvature;

- PointWithRange——成员变量：float x, y, z, (union with float point[4]), range;

- PointWithViewpoint——成员变量：float x, y, z, vp_x, vp_y, vp_z;

- MomentInvariants——float j1, j2, j3;

  MomentInvariants 是一个包含采样曲面上面片的三个不变矩的point类型，描述面片上质量的分布情况；

  ```cpp
  struct{
    float j1, j2, j3;
  }
  ```

…更多内容参考《PCL从入门到精通》P50

## 使用点云创建深度图像（range image）

深度图像：指将从图像采集器到场景中各点的距离值作为像素值的图像，直接反映了景物课间表面的几何形状。

NOTE：深度图像可以经过点云变换计算为点云数据，点云数据也可以反算为深度图像数据

### PCL中模块RangeImage相关类介绍

参考[blog](https://www.cnblogs.com/li-yao7758258/p/6474699.html)

从点云创建深度图实例如下：

```cpp
/*
 * @Description: 如何从点云创建深度图，
 * https://www.cnblogs.com/li-yao7758258/p/6474699.html
 * http://robot.czxy.com/docs/pcl/chapter02/range_image/#_1  推荐
 * @Author: HCQ
 * @Company(School): UCAS
 * @Email: 1756260160@qq.com
 * @Date: 2020-10-21 15:08:41
 * @LastEditTime: 2020-10-21 15:52:15
 * @FilePath: /pcl-learning/06range-images深度图像/1从点云创建深度图/range_image_creation.cpp
 */


#include <pcl/range_image/range_image.h> // //关于深度图像的头文件
#include <pcl/io/pcd_io.h>
#include <pcl/visualization/pcl_visualizer.h> //  //PCL可视化的头文件
#include <pcl/visualization/range_image_visualizer.h> // //深度图可视化的头文件

int main(int argc, char **argv)
{
	pcl::PointCloud<pcl::PointXYZ>::Ptr pointCloudPtr(new pcl::PointCloud<pcl::PointXYZ>);
	pcl::PointCloud<pcl::PointXYZ> &pointCloud = *pointCloudPtr;

	// 创建一个矩形形状的点云
	// 生成数据，x从1.5f到2.5f，y从-0.5f到0.5f，z从-0.5f到0.5f
	for (float y = -0.5f; y <= 0.5f; y += 0.01f)
	{
		for (float z = -0.5f; z <= 0.5f; z += 0.01f)
		{
			pcl::PointXYZ point;
			point.x = 2.0f - y;
			point.y = y;
			point.z = z;
			pointCloud.points.push_back(point);     // 在点的后面插入
		}
	}
	pointCloud.width = (uint32_t)pointCloud.points.size();
	pointCloud.height = 1;

	//pcl::io::loadPCDFile("../bunny.pcd", pointCloud);
	// pcl::io::loadPCDFile("../table_scene_lms400_downsampled.pcd", pointCloud);

	// 实现一个呈矩形形状的点云
	// 根据之前得到的点云图，通过1deg的分辨率生成深度图。
	// angular_resolution为模拟的深度传感器的角度分辨率，即深度图像中一个像素对应的角度大小
	float angularResolution = (float)(1.0f * (M_PI / 180.0f)); //  弧度1°
    //max_angle_width为模拟的深度传感器的水平最大采样角度，
	float maxAngleWidth = (float)(360.0f * (M_PI / 180.0f));   //  弧度360°
	//max_angle_height为模拟传感器的垂直方向最大采样角度  都转为弧度
	float maxAngleHeight = (float)(180.0f * (M_PI / 180.0f)); // 弧度180°
	//传感器的采集位置
	Eigen::Affine3f sensorPose = (Eigen::Affine3f)Eigen::Translation3f(0.0f, 0.0f, 0.0f);
	//深度图像遵循坐标系统
	pcl::RangeImage::CoordinateFrame coordinate_frame = pcl::RangeImage::CAMERA_FRAME;
	float noiseLevel = 0.00; //noise_level获取深度图像深度时，近邻点对查询点距离值的影响水平
	float minRange = 0.0f;   //min_range设置最小的获取距离，小于最小获取距离的位置为传感器的盲区
	int borderSize = 1;      //border_size获得深度图像的边缘的宽度

    //boost在较低版本的CMake中可以处理，但是CMake3.几的版本就无法处理了，报错版本问题，需要把boost改成const std
	boost::shared_ptr<pcl::RangeImage> range_image_ptr(new pcl::RangeImage); // 用于可视化，共享指针
	pcl::RangeImage &rangeImage = *range_image_ptr;

	rangeImage.createFromPointCloud(pointCloud, angularResolution, maxAngleWidth, maxAngleHeight,
		sensorPose, coordinate_frame, noiseLevel, minRange, borderSize);
	/*
	关于range_image.createFromPointCloud（）参数的解释 （涉及的角度都为弧度为单位） ：
	point_cloud为创建深度图像所需要的点云
	angular_resolution_x深度传感器X方向的角度分辨率
	angular_resolution_y深度传感器Y方向的角度分辨率
	pcl::deg2rad (360.0f)深度传感器的水平最大采样角度
	pcl::deg2rad (180.0f)垂直最大采样角度
	scene_sensor_pose设置的模拟传感器的位姿是一个仿射变换矩阵，默认为4*4的单位矩阵变换
	coordinate_frame定义按照那种坐标系统的习惯  默认为CAMERA_FRAME
	noise_level  获取深度图像深度时，邻近点对查询点距离值的影响水平
	min_range 设置最小的获取距离，小于最小的获取距离的位置为传感器的盲区
	border_size  设置获取深度图像边缘的宽度 默认为0
	*/
	std::cout << rangeImage << "\n";
	// --------------------------------------------
	// -----Open 3D viewer and add point cloud-----
	// --------------------------------------------
    // 打开3D viewer
	pcl::visualization::PCLVisualizer viewer("3D Viewer");
    // 设置背景颜色
	viewer.setBackgroundColor(1, 1, 1);
	// 添加深度图点云
	pcl::visualization::PointCloudColorHandlerCustom<pcl::PointWithRange> range_image_color_handler(range_image_ptr, 0, 0, 0);
	viewer.addPointCloud(range_image_ptr, range_image_color_handler, "range image");
	viewer.setPointCloudRenderingProperties(pcl::visualization::PCL_VISUALIZER_POINT_SIZE, 4, "range image");

	// 添加原始点云
	pcl::visualization::PointCloudColorHandlerCustom<pcl::PointXYZ> org_image_color_handler(pointCloudPtr, 255, 100, 0);
	viewer.addPointCloud(pointCloudPtr, org_image_color_handler, "orginal image");
	viewer.setPointCloudRenderingProperties(pcl::visualization::PCL_VISUALIZER_POINT_SIZE, 2, "orginal image");

	viewer.initCameraParameters();
	viewer.addCoordinateSystem(1.0);

	//--------------------
	// -----Main loop-----
	//--------------------
	while (!viewer.wasStopped())
	{
		viewer.spinOnce();
		pcl_sleep(0.01);
	}
	return (0);
}
```

## 贪婪投影算法原理GreedyProjectionAlgorithm
