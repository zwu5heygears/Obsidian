
```cpp
imread(const string &filename, int flags = 0) // 0:gray 1:RGB <0:origian
Mat src = imread("src.png", IMREAD_REDUCED_GRAYSCALE_4);  
// Gray = 0.2989*R + 0.5870*G + 0.1140*B
cvtColor(src, src, COLOR_GRAY2BGR);
namedWindow("s", WINDOW_NORMAL);
imshow("s", src);
waitKey(0);
destroyAllWindows();
imwrite("src.jpg", src);

	vector<int> compression_params;
	compression_params.push_back(cv::IMWRITE_JPEG_RST_INTERVAL);
	compression_params.push_back(90); // default::95
	imwrite("src.jpg", src, compression_params);

	cout << (int)saturate_cast<uchar> (288) << endl;
	cout << (int)*src.ptr<uchar>(10, 0) << endl;
	cout << (int)src.at<uchar>(10, 0) << endl;
	Mat mask;
	Mat img(src);	
	src.copyTo(src, mask);  // mask = 1/255
	src.convertTo(src, CV_8UC1);
	src.convertTo(src, CV_16FC1, 2, 20); //  src * alhpa + beta;
	src = src.clone();
	cout << src.channels() << src.rows << src.cols << src.depth() << src.empty();

```
> Mat create and copy
```cpp

```











>Learn from Refelx/Projector/CIL.h

Mat converto vector.
```cpp
template<typename T>
std::vector<T> MatToVec(const cv::Mat& mat) {
	if (mat.empty()) {
		return {};
	}
	return (std::vector<T>)(mat.reshape(1, 1));
}
//template std::vector<uchar> MatToVec(const cv::Mat& mat);
//template std::vector<int> MatToVec(const cv::Mat& mat);
//template std::vector<float> MatToVec(const cv::Mat& mat);
//template std::vector<double> MatToVec(const cv::Mat& mat);
```
Vector converTo Mat
```cpp
template<typename T>
cv::Mat VecToMat(const std::vector<T>& vec, const cv::Size& size) {
	if (vec.empty()) {
		return {};
	}
	Mat mat = Mat(vec).clone();
	Mat dst = mat.reshape(1, size.height());
	return dst;
}
//template cv::Mat VecToMat(const std::vector<uchar>& vec, const cv::Size& size);
//template cv::Mat VecToMat(const std::vector<int>& vec, const cv::Size& size);
//template cv::Mat VecToMat(const std::vector<float>& vec, const cv::Size& size);
//template cv::Mat VecToMat(const std::vector<double>& vec, const cv::Size& size);
```

```cpp
// power to mask (image)
template<typename T>
cv::Mat VecToMask(const std::vector<T>& vec, const cv::Size& vec_size, const cv::Size &dsize, const int interpolation) {
	if (vec.empty()) {
		return {};
	}
	if (vec_size.empty() || dsize.empty()) {
		return {};
	}
	Mat mat = VecToMat(vec, vec_size);
	if (mat.empty()) {
		return {};
	}
	Mat mask = MatToMask(mat);
	if (mask.empty()) {
		return {};
	}
	auto mask_img = MatToImg(mat, dsize, interpolation);
	if (mask_img.empty()) {
		return {};
	}
	cv::convertScaleAbs(mask_img, mask_img);
	return mask_img;
}
//template cv::Mat VecToMask(const std::vector<uchar>& vec, const cv::Size& vec_size, const cv::Size& dsize, const int interpolation);
//template cv::Mat VecToMask(const std::vector<int>& vec, const cv::Size& vec_size, const cv::Size& dsize, const int interpolation);
//template cv::Mat VecToMask(const std::vector<float>& vec, const cv::Size& vec_size, const cv::Size& dsize, const int interpolation);
//template cv::Mat VecToMask(const std::vector<double>& vec, const cv::Size& vec_size, const cv::Size& dsize, const int interpolation);

// 1. OSTU threshold
cv::Mat OtsuThreshold(const cv::Mat& src, uint8_t minV = 0, uint8_t maxV = 255) {
	// check parameter
	if (src.empty()) {
		return {};
	}

	if (src.channels() != 1) {
		return {};
	}
	
	cv::Mat blur;
	cv::GaussianBlur(src, blur, Size(3, 3), 0);
	
	Mat dst;
	auto threV = threshold(blur, dst, 0, 255, THRESH_BINARY | THRESH_OTSU);
	return dst;
}

// Screen Image to Mat(size = 6x10) for adjust mask
cv::Mat ImgToMat(const Mat& img, const Size& size) {
	// check
	if (img.empty()) {
		return {};
	}
	if (img.channels() != 1) {
		return {};
	}
	if (size.empty()) {
		return {};
	}
	if (size.width == 0 || size.height == 0) {
		return {};
	}
	if (img.rows % size.height != 0 || img.cols % size.width != 0) {
		return {};
	}

	int rGap = img.rows % size.height;
	int cGap = img.cols % size.width;

	Mat dst(size.height, size.width, CV_32FC1);

	for (int r = 0; r < size.height; r++) {
		for (int c = 0; c < size.width; c++) {
			int rEnds = (r + 1) * rGap;
			rEnds = rEnds > img.rows ? img.rows : rEnds;

			int cEnds = (c + 1) * cGap;
			cEnds = cEnds > img.cols ? img.cols : cEnds;

			Mat cell(img, Rect(r * rGap, c * cGap, rGap, cGap));  // rows--cols
			
			dst.at<float>(r, c) = static_cast<float>(mean(cell)[0]);
		}
	}

	return dst;
}

// Mat 6x10 to Screen size
cv::Mat MatToImg(const Mat& mat, const Size& size, int interpolation) {
	if (mat.empty()) {
		return {};
	}
	if (size.empty()) {
		return {};
	}
	if (mat.channels() != 1) {
		return {};
	}
	Mat dst;
	cv::resize(mat, dst, size, interpolation);
	return dst;
}

// Power mat(6x10) to Mat (6x10)
cv::Mat MatToMask(const cv::Mat& mat) {
	if (mat.empty()) {
		return {};
	}

	Mat matCopy;
	mat.convertTo(matCopy, CV_32FC1);

	Mat gray(matCopy.rows, matCopy.cols, CV_32FC1, Scalar(255));

	double minVal;
	cv::minMaxLoc(matCopy, &minVal);

	if (minVal < 0.001) {  // power too small
		return {};
	}
	
	Mat minMat(matCopy.rows, matCopy.cols, CV_32FC1, Scalar(minVal));
	Mat temp;
	cv::divide(matCopy, minMat, temp);
	Mat dst;
	cv::divide(gray, temp, dst);
	
	cv::convertScaleAbs(dst, dst);
	return dst;
}

// BGR to GRAY
cv::Mat CvtBGR2Gray(const Mat& img) {
	if (img.empty()) {
		return {};
	}
	Mat gray;
	if (img.channels() == 3) {
		cv::cvtColor(img, gray, COLOR_BGR2GRAY);
	}
	else if (img.channels() == 1) {
		img.copyTo(gray);
	}
	else {
		return {};
	}
	return gray;
}

float SolveEq(const std::vector<float>& coeffs, float x) {
	if (coeffs.empty()) {
		return 0.0f;
	}
	int numOfCoeffs = coeffs.size();

	// method 1
	float result = 0.0f;
	for (float c : coeffs) {
		result += pow(x, numOfCoeffs - 1) * c;
		numOfCoeffs--;
	}

	// method 2

	return result;
}

int checkMonotonicity(const std::vector<cv::Point2f>& pnts) {
	if (pnts.empty()) {
		return {};
	}
	auto temp = pnts;
	sort(temp.begin(), temp.end(), [](const cv::Point2f& p1, const cv::Point2f& p2) -> bool {return p1.x < p2.x; });
	
	int counter = 0;
	Point2f p1, p2;
	for (std::vector<cv::Point2f>::size_type i = 0; i < temp.size() - 1; ++i) {
		p1 = temp[i];
		p2 = temp[i + 1];
		if (p1.x >= p2.x) {
			return 0;
		}
		if (p1.y < p2.y) {
			++counter;
		}
		else if (p1.y > p2.y) {
			--counter;
		}
	}
	if (abs(counter) != temp.size() - 1) {
		return 0;
	}
	return counter > 0 ? 1 : -1;
}

std::vector<float> polyFit(const std::vector<cv::Point2f>& pnts, int degree) {
	if (pnts.empty()) {
		return {};
	}
	int m = pnts.size();
	int n = degree + 1; // K0
	
	// at least x > equle number
	if (m < n) {
		return {}; 
	}
	Mat X(m, n, CV_32FC1);
	Mat Y(m, 1, CV_32FC1);
	Mat K(n, 1, CV_32FC1);

	Mat temp(1, n, CV_32FC1);
	int r = 0;
	for (auto p : pnts) {
		Y.at<float>(r, 0) = p.y;
		for (int i = 0; i < n; i++) {
			temp.at<float>(0, i) = static_cast<float>(pow(p.x, n - 1 - i));
		}
		temp.copyTo(X.row(r));
		++r;
	}

	cv::solve(X, Y, K, DECOMP_QR);

	auto coeffs = MatToVec<float>(K);
	
	if (coeffs.empty()) {
		return {};
	}
	// K may be NAN (0/0)
	for (float& coeff : coeffs) {
		if (coeff == coeff) continue;
		coeff = 0;
	}
	return coeffs;
}

cv::Mat createBlob(const cv::Size& size, const cv::Point2f& center, const int& inner_radius, 
	const std::vector<int>& outer_radius, const int& thickness) {
	if (size.empty()) {
		return {};
	}
	Mat canvas = Mat(size, CV_8UC1, Scalar(0));
	const Point2f frame_c = Point2f(size.width / 2.0, size.height / 2.0);
	

	// method 1: warpAffine
	//cv::circle(canvas, frame_c, inner_radius, Scalar(255), -1); // filling
	//for (auto r : outer_radius) {
	//	cv::circle(canvas, frame_c, r, Scalar(255), thickness);
	//}
	//float m[2][3] = { {1, 0, center.x - frame_c.x}, {0, 1, center.y - frame_c.y} };
	//Mat affine_mtx(2, 3, CV_32FC1, m);
	//Mat dst;
	//warpAffine(canvas, dst, affine_mtx, size, INTER_LINEAR, BORDER_CONSTANT, 0);  // 边界填充0
	
	// method 2: direct circle
	cv::circle(canvas, center, inner_radius, Scalar(255), -1);
	for (auto r : outer_radius) {
		circle(canvas, center, r, Scalar(255), thickness);
	}
	Mat dst = canvas.clone();
	
	return dst;
}


cv::Mat createWhite(const cv::Size& size) {
	if (size.empty()) {
		return {};
	}
	cv::Mat result(size, CV_8UC1, Scalar(255));
	return result;
}

cv::Mat createBlack(const cv::Size& size) {
	if (size.empty()) {
		return {};
	}
	cv::Mat result(size, CV_8UC1, Scalar(0));
	return result;
}

cv::Mat createCross(const cv::Size& dsize, int size, int thickness) {
	Mat canvas(dsize, CV_8UC1, Scalar(0));
	int cx = dsize.width / 2;
	int cy = dsize.height / 2;
	cv::line(canvas, Point2f(cx - size / 2, cy), Point2f(cx + size / 2, cy), 255, thickness);
	cv::line(canvas, Point2f(cx, cy - size / 2), Point2f(cx, cy + size / 2), 255, thickness);
	return canvas;
}

std::string MatToBase64(const cv::Mat& mat) {
	if (mat.empty()) {
		return {};
	}
	vector<uchar> buf;
	cv::imencode(".png", mat, buf);
	//return base64_encode(buf.data(), buf.size(), false);
	return {};
}

cv::Mat Base64ToMat(const std::string& mat_base64) {
	if (mat_base64.empty() || mat_base64.length() == 0) {
		return {};
	}
	cv::Mat mat;
	//auto mat_s = base64_decode(mat_base64, false);  // string
	//vector<uchar> base64_img(mat_s.begin(), mat_s.end());
	//return cv::imdecode(base64_img, IMREAD_UNCHANGED);
	return {};
}

cv::Mat Masked(const cv::Mat& src, const cv::Mat& mask) {
	if (src.empty()) {
		return {};
	}
	if (mask.empty()) {
		return {};
	}
	double maxVal = 0.0;
	cv::minMaxLoc(src, nullptr, &maxVal);
	if (maxVal > 0) {
		Mat dst;
		dst = src.mul(mask, 1.0 / maxVal);
		if (dst.empty()) {
			return {};
		}
		return dst;
	}
	else {
		return {};
	}
	
}
// 映射0-maxv区间 divate
cv::Mat CvtScaleMat(const cv::Mat& image, float maxV) {
	if (image.empty()) {
		return {};
	}
	double maxVal = 0.0;
	cv::minMaxLoc(image, nullptr, &maxVal);
	if (maxVal > 0) {
		Mat dst;
		convertScaleAbs(image, dst, maxV / maxVal);
		return dst;
	}
	else {
		return {};
	}
}
```

> iterations
```cpp

```