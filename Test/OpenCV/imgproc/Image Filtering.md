[OpenCV Document](https://docs.opencv.org/4.8.0/d4/d86/group__imgproc__filter.html)

[bilaterFilter](https://www.xjx100.cn/news/420481.html?action=onClick)
```cpp
bilaterFilter(Mat src, Mat dst, int d, double  sigmaColor, double sigmaSpace, int borderType)
```


```cpp
	blur(src, src, Size(3, 3), Point(-1, -1), 4);
	boxFilter(src, src, src.depth(), Size(3, 3), Point(-1, -1), true, 4);


	vector<Mat> dst;
	buildPyramid(src, dst, 2, 4);

	Mat src;

	
	Mat kernel = getStructuringElement(MORPH_RECT, Size(3, 3));
	erode(src, src, kernel, Point(-1, -1), 1, 0);
	dilate(src, src, kernel, Point(-1, -1), 1, 0);

	filter2D(src, src, -1, kernel, Point(-1, -1), 0.0, 4);
	GaussianBlur(src, src, Size(3, 3), 0, 0, 4);
	
	Mat kx, ky;
	getDerivKernels(kx, ky, 1, 0, 3);
	kx = kx.reshape(0, 1);
	Mat kxy = ky * kx;

	Mat gaborKernel = getGaborKernel(Size(3, 3), 2.0, 2, 2, 2);
	
	Mat gaussianKernel = getGaussianKernel(3, 1);
	Mat gaussianKernel2 = getGaussianKernel(3, 1);
	gaussianKernel = gaussianKernel.reshape(0, 1);
	Mat g = gaussianKernel2 * gaussianKernel;
	cout << g << endl;

Mat structElement = getStructuringElement(MORPH_ELLIPSE, Size(5, 5), Point(-1, -1));
	cout << structElement << endl;

	Laplacian(src, src, -1, 3);
	medianBlur(src, src, 3);
	cout << src << endl;
	//morphologyEx(src, src, MORPH_BLACKHAT, structElement);
	morphologyEx(src2, src2, MORPH_HITMISS, kernel2, Point(1, 1), 1);
	cout << src2 << endl;

	cout << src << endl;
	pyrUp(src, src);  // https://blog.csdn.net/m0_50317149/article/details/129996609
	pyrDown(src, src);  // https://blog.csdn.net/qq_54185421/article/details/124350723
	

	Mat img = imread("1.png");
	pyrMeanShiftFiltering(img, img, 10, 10);
	/* Scharr */ 
	Mat imgx, imgy;
	Scharr(img, imgx, -1, 1, 0);
	convertScaleAbs(imgx, imgx);
	Scharr(img, imgy, -1, 0, 1);
	convertScaleAbs(imgy, imgy);
	Mat imgxy = imgx + imgy;
	convertScaleAbs(imgxy, imgxy);

	Mat kernelX = (Mat_<uchar>(1, 3) << 1, 1, 0);
	Mat kernelY = (Mat_<uchar>(1, 3) << 1, 1, 0);
	Mat kernelY2 = (Mat_<uchar>(3, 1) << 1, 1, 0);
	sepFilter2D(src, dst, -1, kernelX, kernelY);
	sepFilter2D(src, dst2, -1, kernelX, kernelY2);  
	// sep converge advantage: https://www.guyuehome.com/41277
	cout << !cv::norm(dst, dst2, NORM_L1) << endl;


// Sobel
	Sobel(src, dst, CV_64F, 1, 0, -1);
	Sobel(src, dst2, CV_64F, 0, 1, -1);
	convertScaleAbs(dst, dst);
	convertScaleAbs(dst2, dst2);
	Mat rr = dst2 + dst;

	spatialGradient(src, dst, dst2);

	sqrBoxFilter(A, A, -1, Size(3, 3), Point(), false);
	stackBlur(src, dst, Size(3, 3));
```