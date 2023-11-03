[OpenCV Document](https://docs.opencv.org/4.8.0/d7/d1b/group__imgproc__misc.html)


```cpp
adaptiveThreshold(InputArray src
				  OutputArray dst,
				  double maxVal, 
				  int adaptiveMethod = ASAPTIVE_THRESH_MEAN_C ||ADAPTIVE_THRESH_GAUSSIAN_C,
				  int thresholdType = THRESH_BINARY,
				  int blockSize, 
				  double C);
```

自适应阈值（用于二值化处理图像，对于对比大的图像有较好效果，相对于opencv中固定阈值化操作（threshold），自适应阈值中图像中每一个像素点的阈值是不同的，该阈值由其领域中图像像素带点加权平均决定。这样做的好处：

- 每个像素位置处的二值化阈值不是固定不变的，而是由其周围邻域像素的分布来决定的。
- 亮度较高的图像区域的二值化阈值通常会较高，而亮度较低的图像区域的二值化阈值则会相适应地变小。
- 不同亮度、对比度、纹理的局部图像区域将会拥有相对应的局部二值化阈值。
```cpp
adaptiveThreshold(src, src, 200, ADAPTIVE_THRESH_GAUSSIAN_C, THRESH_BINARY, 3, 0);
```
---

```cpp
Mat weights1(src.size(), CV_32FC1, Scalar(0.5));
Mat weights2(src.size(), CV_32FC1, Scalar(0.5));
blendLinear(src, src, weights1, weights2, src);
imshow("src", src);
waitKey();
```
	type requirements:

---



```cpp
Mat weights1(src.size(), CV_32FC1, Scalar(0.5));
Mat weights2(src.size(), CV_32FC1, Scalar(0.5));
blendLinear(src, src, weights1, weights2, src);  // cv::addWeighted()
```

---

[[Geometric Image Transformations]]

