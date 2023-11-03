- 多边形逼近拟合
- 弧线长度
- 最小左上角外接非旋转矩形
- 旋转矩形角点
- 连通域处理（标签、外接矩形、面积、中心点）
- 轮廓面积
- 点击返回凸包轮廓
- 凸包缺陷检测
- 查找轮廓
- 拟合椭圆
- 拟合直线
- Hu矩
- 检测凸包和凸包交集
- 是否轮廓为凸包
- 形状匹配
- 最小面积外接旋转矩形
- 最小外接圆形
- 最小外接三角形
- 图像矩
- 点与轮廓位置关系或距离
- 旋转矩形交集关系
```cpp
1 approxPolyDP(InputArray curve,
			   OutPutArray approxCurve,
			   double epsilon,
			   bool closed)
```
[Douglas-peucker](https://blog.csdn.net/u013925378/article/details/86075230)

详细应用：(https://blog.csdn.net/qq_27487739/article/details/131320356)
```cpp
Mat srcImg = imread("1.png");
Mat dstImg(srcImg.size(), CV_8UC3, Scalar::all(0));
cvtColor(srcImg, srcImg, COLOR_BGR2GRAY);
threshold(srcImg, srcImg, 220, 255, THRESH_BINARY_INV);
vector<vector<Point>> contours;
vector<Vec4i> hierarcy;
findContours(srcImg, contours, hierarcy, 0, CHAIN_APPROX_NONE);
vector<vector<Point>> contours_poly(contours.size());
for (int i = 0; i < contours.size(); i++)
{
	approxPolyDP(Mat(contours[i]), contours_poly[i], 100, false);
	drawContours(dstImg, contours_poly, i, Scalar(0, 255, 255), 2, 8);
}
imshow("approx", dstImg);
waitKey();
```
---

```cpp
2 double arcLength(InputArray curve,
				   bool closed);
```
计算弧线（轮廓）长度
```cpp
	length = arcLength(contours.at(i), true);
	cout << i << "th:" << length << endl;
```

---
```cpp
3 Rect boundingRect(InputArray array)
```
计算点集的最小左上角外接矩形
```cpp
Rect rect = boundingRect(contours.at(maxID));   
```
---
```cpp
4 void boxPoint(RotatedRect box,
				OutPutArray points)
```
计算旋转矩形的4个点
	= RotatedRect.point(p[4])
```cpp
Point2f* vertices = new cv::Point2f[4];
RotatedRect ellipseRect;
ellipseRect.points(vertices);
```
---
```cpp
5 int connectedComponents(InputArray image,
						  OutputArray labels,
						  int connnectivity,
						  int ltype,
						  int ccltype)
```

https://mp.weixin.qq.com/s?__biz=MzU0NjgzMDIxMQ==&mid=2247486901&idx=2&sn=e8e5a0ef034e79c558fe99b66e282b9b&chksm=fb56ef59cc21664f66f4a3776bc665ea1837e86ad48bf90f351f70befaee8e45552c30d08817&scene=27&poc_token=HJND7GSjH3tAx0ymeij4rmt_BQ5ByKw8vant0siV
连通域标记

---
```cpp
6 int connectedComponentsWithStats(InputArray image,
								 OutputArray labels,
								 OutputArray stats,
								 OutputArray centroids,
								 int connectivity,
								 int ltype,
								 int ccltype)
```
连通域标记并返回外接框、面积、中心点

---
```cpp
7 double cv::contourArea(InputArray contour,
					   bool oriented = false)
```
计算轮廓面积
```cpp
cv::contourArea(contours.at(i), false) << endl
```
---

```cpp
8 void convexHull(InputArray points,
				OutpuArray hull,
				bool clockwise = false,
				bool returnPoints = true)
```
计算点集的凸包
https://blog.csdn.net/michaelshare/article/details/85133071
https://blog.csdn.net/qq_40784418/article/details/105999175

利用凸包检测可以得到进一步的轮廓信息，**可进行边界检测**，结合凸缺陷**可以实现手势识别和物体识别**。
	clockwise：true：凸包顺时针输出为正，否则逆时针
	returnPoints：true：返回凸包Point,否则返回Index
```cpp
vector<vector<Point>> hull(contours.size());
vector<vector<int>> intHull(contours.size());
vector<vector<Vec4i>> hullDefects(contours.size());
for (int i = 0; i < contours.size(); ++i) {
	// 轮廓中的凸包
	convexHull(contours.at(i), hull.at(i), false);  
	// 轮廓凸包的Index
	convexHull(contours.at(i), intHull.at(i), false);  
	convexityDefects(contours.at(i), intHull.at(i),
	hullDefects.at(i));
}
```
---
```cpp
8 void convexityDefects(inputArray contour,
					    InputArray convexhull,
					    OutputArray convexityDefects)
```
凸包曲线检测
https://zhuanlan.zhihu.com/p/58696677?utm_id=0
---
```cpp
9 Ptr<GeneralizedHoughBallard> createGeneralizedHoughBallard()
```
---
```cpp
10 Ptr<GeneralizedHoughGuil> createGeneralizedHoughGuil()
```
---
```cpp
11 void findContours(InputArray image,
					 OutputArrayOfArrays contours,
					 [OutputArray hierarchy],
				     int mode,
				     int method,
				     Point offset = Point())
```
计算图像中的轮廓
	image：CV_8UC1二值化后图片
	contours:vector vector Point>>
	hierarchy:vector Vec4i>

---
```cpp
12 RotatedRect fitEllipse(InputArray points)
```
点集拟合椭圆外接旋转矩形
>point size  >= 5;
---
```cpp
13 RoatatedRect fitEllipseAMS(InputArray points)
```
---
```cpp
14 RotatedRect fitEllipseDirect(InputArray points)
```
---
```cpp
15 void fitLine(InputArray points,
			 OutPutArray line,
			 int distType,
			 double param,
			 double reps,
			 double aeps)
```
拟合曲线
```cpp
vector<Point> ellipsePoint;
ellipsePoint.push_back(Point(200, 50));
ellipsePoint.push_back(Point(500, 50));
ellipsePoint.push_back(Point(230, 50));
ellipsePoint.push_back(Point(330, 50));
Vec4f line_para;
fitLine(ellipsePoint, line_para, DIST_L2, 0, 1e-2, 1e-2);
cout << "line_para = " << line_para << endl;
double k = line_para[1] / line_para[0];
Point p1, p2;
p1.x = 0;
p1.y = k * (0 - line_para[2]) + line_para[3];
p1.x = 20;
p1.y = k * (20 - line_para[2]) + line_para[3];
line(canvas, p1, p2, Scalar(255));
imshow("src", canvas);
waitKey();
```
---
```cpp
16 void HuMoments(const Moments &moments
			      double hu[7] || OutputArray hu)
```
jisuan Hu矩

---
```cpp
17 float intersectConvexConvex(InputArray p1,
							   InputArray p2,
							   OutPutArray p12,
							   bool handleNested = true)
```
计算凸包是否相交
	handleNested=true 完全封闭另一个算相交并有一个交点，否则没有交集
```cpp
	intersectConvexConvex(hull.at(i), hull.at(i + 1), temp, false);
```
---
```cpp
18 bool isContourConvex(InputArray contour)
```
判断轮廓是否为凸包
---
```cpp
19 double matchShapes(InputArray contour1,
					  InputArray contour2,
					  int method,
					  double parameter)
```
- parameter not support now(set 0)
	InputArray is Mat or grayscale image
轮廓完全相似，返回值为0
---
```cpp
20 RotatedRect minAreaRect(InputArray points)
```
包含点集的最小旋转正方形

---
```cpp
21 void minEnclosingCircle(InputArray points,
					    Poin2f& center,
					    float& radius)
```
包含点集的最小圆形

---
```cpp
22 double minEnclosingTriangle(InputArray points,
							   OutputArray triangle)
```
> triangle size:(3,2)
包含点集的最小三角形

---
```cpp
23 Moments moments(InputArray array,
				   bool binaryIamge = false)
```
图像矩：空间矩、中心矩、中心归一化矩

---
```cpp
24 double pointPolygonTest(InputArray contour,
						   Point2f pt,
						   bool measureDist)
```
measuredist =false  返回 点在多边形内（1）外（-1）边上（0），否则返回距离

---
```cpp
25 int rotatedRectangleIntersection(const RotatedRect& rect1,
								    const RotatedRect& rect2,
								    OutputArray intersectingRegion)
```
判断2个旋转矩形交集，返回0(不相交)1(部分相交)2(包含)，返回相交点
>intersectingRegion size(4,2)