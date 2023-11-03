[OpenCV Document](https://docs.opencv.org/4.8.0/dc/d84/group__core__basic.html)

```cpp
1 class _InputArray{}
2 class _InpuOutputArray{}
3 class _OutputArray{}
4 class Algorithm{}
5 class Complex<_Tp>

cv::Complexf c1(1, 2);
cv::Complexd c2(2, 3);
cout << c1.re << c1.im << c1.conj() << endl;

6 class DataDepth<_Tp>
7 class DataType<_Tp>  // help for CV_8UC1 ...
8 class Dmatch // class for mathcing keypoint descriptors
9 class Formattded
10 class Formatter // format()
11 class KeyPoint(Point2f pt || float x, floaty,  
				 float size,
				 float angle = -1,
				 float response = 0,
				 int octave = 0,
				 int class_id = -1)  // use for keypoint detectors
```

```cpp
12 class Mat{
	// constructor
	Mat();
	Mat(int rows, int cols, int type);
	Mat(Size size, int type);
	Mat(int rows, int cols, int type, const Scalar &s);
	Mat(int ndims, const int *size, int type);  // size.shape = ndims
	Mat(int ndims, const int *size, int type, const Scalar &s);
	Mat(const vector<int> &size, int type);
	Mat(const vector<int> &size, int type, Scalar &s);  // size.size()
	Mat(const Mat& m);
	Mat(int rows, int cols, int type, void *data, size_t step = AUTO_STEP);
	Mat(Size size, int type, void *data, size_t step= AUTO_STEP);
	
}

```