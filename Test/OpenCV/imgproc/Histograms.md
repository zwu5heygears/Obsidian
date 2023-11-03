

```cpp
Mat src = imread("1.png", 0);
const int channels[] = { 0 };
Mat hist;
int dims = 1;
const int histSize[] = { 256 };
float pranges[] = { 0, 255 };
const float* ranges[] = { pranges };
calcHist(&src, 1, channels, Mat(), hist, dims, histSize, ranges, true, false);

int scale = 2;
int hist_height = 256;
Mat hist_img = Mat::zeros(hist_height, 256 * scale, CV_8UC3);
double maxVal;
minMaxLoc(hist, 0, &maxVal, 0, 0);
for (int i = 0; i < 256; ++i) {
	float bin_val = hist.at<float>(i);
	int indensity = cvRound(bin_val * hist_height / maxVal);
	rectangle(hist_img, Point(i * scale, hist_height - 1), Point((i + 1) * scale - 1, hist_height - indensity), Scalar(255, 255, 255));
}
imshow("hist", hist_img);
waitKey();
```