[OpenCV Document](https://docs.opencv.org/4.8.0/d7/d1b/group__imgproc__misc.html)

```cpp
Mat map_x_float = Mat::ones(Size(10, 10), CV_32FC1);
Mat map_y_float = Mat::ones(Size(10, 10), CV_32FC1);

Mat dstmap1, dstmap2;
convertMaps(map_x_float, map_y_float, dstmap1, dstmap2, CV_16SC2);
cout << dstmap1 << endl;
cout << dstmap2 << endl;
```


```cpp
	Point2f srcTri[3];
	Point2f dstTri[3];
	srcTri[0] = Point2f(0, 0);
	srcTri[1] = Point2f(src.cols - 1, 0);
	srcTri[2] = Point2f(0, src.rows - 1);
	dstTri[0] = Point2f(src.cols * 0, src.rows * 0.33);
	dstTri[1] = Point2f(src.cols * 0.85, src.rows * 0.23);
	dstTri[2] = Point2f(src.cols * 0.15, src.rows * 0.7);
	Mat affineMat(Size(2, 3), CV_32FC1);
	affineMat = getAffineTransform(srcTri, dstTri);
	cout << affineMat << endl;
```


```cpp
vector<Point2f> corners(4);
corners.at(0) = Point2f(0, 0);
corners.at(1) = Point2f(src.cols - 1, 0);
corners.at(2) = Point2f(0, src.rows - 1);
corners.at(3) = Point2f(src.cols -1, src.rows - 1);

vector<Point2f> corners_trans(4);
corners_trans.at(0) = Point2f(150, 250);
corners_trans.at(1) = Point2f(700, 0);
corners_trans.at(2) = Point2f(0, src.rows - 1);
corners_trans.at(3) = Point2f(600, src.rows - 1);

Mat trans = getPerspectiveTransform(corners, corners_trans);
```

```cpp
Mat src = imread("1.png", 0);
Mat dst;
getRectSubPix(src, Size(src.cols / 5, src.rows / 5), Point2f(src.cols / 2, src.rows / 2), dst);  // INTER_LINEAR
imshow("dst", dst);
waitKey();
```

```cpp
	Mat src = imread("1.png", 0);
	Mat rotationMatrix = getRotationMatrix2D(Point2f(src.cols / 2, src.rows / 2), 45, 1);  // CV_64FC1
	double cos = abs(rotationMatrix.at<double>(0, 0));
	double sin = abs(+rotationMatrix.at<double>(0, 1));
	int new_w = src.cols * cos + src.rows * sin;
	int new_h = src.cols * sin + src.rows * cos;
	rotationMatrix.at<double>(0, 2) += (new_w - src.cols) * 0.5;  // adaptive dst.size
	rotationMatrix.at<double>(1, 2) += (new_h - src.rows) * 0.5;
	warpAffine(src, src, rotationMatrix, Size(new_w, new_h));
	imshow("1", src);
	waitKey();
```


```cpp
	Mat affineMat2;
	invertAffineTransform(affineMat, affineMat2);  
	Mat dst2;
	warpAffine(dst, dst2, affineMat2, dst.size());
	imshow("dst2", dst2);
	waitKey();
```

```cpp
	linearPolar(src, src, Point2f(src.cols / 2, src.rows / 2), 20, 1);  // replaced by warpPolar
	imshow("src", src);
	waitKey();
```


```cpp
	logPolar(src, src, Point2f(src.cols / 2, src.rows / 2), 20, 1);  // replaced by logPolar
	imshow("src", src);
	waitKey();
```


```cpp
	Mat dst;
	Mat map_x(src.size(), CV_32FC1);
	Mat map_y(src.size(), CV_32FC1);

	for (int i = 0; i < src.rows; ++i) {
		for (int j = 0; j < src.cols; ++j) {
			if (j > map_x.cols * 0.25 && j < map_x.cols * 0.75 && i > map_x.rows * 0.25 && i < map_x.rows * 0.75) {
				map_x.at<float>(i, j) = 2 * (j - map_x.cols * 0.25f) + 0.5f;
				map_y.at<float>(i, j) = 2 * (i - map_x.rows * 0.25f) + 0.5f;
			}
			else
			{
				map_x.at<float>(i, j) = 0;
				map_y.at<float>(i, j) = 0;
			}
		}
	}
	for (int i = 0; i < src.rows; ++i) {
		for (int j = 0; j < src.cols; ++j) {
			map_x.at<float>(i, j) = float(map_x.cols - j);
			map_y.at<float>(i, j) = float(i);
		}
	}
	remap(src, dst, map_x, map_y, INTER_LINEAR, 0, Scalar(0));
	imshow("src", dst);
	waitKey();
```

```cpp
	resize(src, src, Size(src.cols / 2, src.rows / 2), 0, 0, INTER_CUBIC);
	resize(src, src, Size(), 0.5, 0.5, INTER_CUBIC);
	resize(src, src, Size(src.cols * 2, src.rows * 2), 0, 0, INTER_AREA);
```

```cpp
	Mat dst;
	warpAffine(src, dst, affineMat, src.size());
	imshow("dst", dst);
	waitKey();
```


```cpp
warpPerspective(src, src, trans, src.size());
```


```cpp
warpPolar(src, dst1, src.size(), Point2f(src.cols / 2, src.rows / 2), 400, INTER_AREA + WARP_POLAR_LOG);

warpPolar(dst1, dst2, src.size(), Point2f(src.cols / 2, src.rows / 2), 400, INTER_AREA + WARP_POLAR_LOG + WARP_INVERSE_MAP);
```