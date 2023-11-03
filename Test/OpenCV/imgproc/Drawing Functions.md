[OpenCV Document](https://docs.opencv.org/4.8.0/d6/d6e/group__imgproc__draw.html)


```cpp
arrowedLine(src, Point(300, 300), Point(200, 200), Scalar(0, 0, 255), 10, 4, 0, 0.5);

// 2
circle(src, Point(100, 0), 200, Scalar(0, 0, 255), -1);


// 3
Rect rect(30, 30, 100, 100);
Point p1(20, 20);
Point p2(100, 20);
bool isIn = clipLine(rect, p1, p2);
bool isIn2 = clipLine(Size(30, 30), p1, p2);
cout << isIn << isIn2 << endl;

// 4
//cvtColor(src, src, COLOR_BGR2GRAY);
//vector<vector<Point>> contours;
//vector<Vec4i> hierarchy;
//findContours(src, contours, hierarchy, RETR_CCOMP, CHAIN_APPROX_SIMPLE);
//int idx = 0;
//for (; idx >= 0; idx = hierarchy[idx][0]) {
	Scalar color(rand() & 255, rand() & 255, rand() & 255);
```
---


```cpp
	drawContours(src, contours, idx, color, 2, 8, hierarchy);

```

	contorus vector<vector<Point>>


```cpp
// 5
drawMarker(src, Point(200, 200), Scalar(0, 0, 255), 6, 50);  // 0-6


// 6
ellipse(src, RotatedRect(Point2f(200, 200), Size(200, 300), 20.0f), Scalar(0, 0, 255));
ellipse(src, Point(200, 200), Size(200, 100), 30, 0, 180, Scalar(0, 0, 255), 1);


// 7
//vector<Point> pnts;
//Mat ellipse = Mat::zeros(500, 500, CV_8UC3);
ellipse2Poly(Point(200, 200), Size(200, 100), 20, 0, 300, 30, pnts);
//for (int i = 0; i < pnts.size() - 1; ++i) {
//	line(ellipse, pnts[i], pnts[i + 1], Scalar(0, 0, 255));
//}
//imshow("ellipse", ellipse);
//waitKey();


// 8
fillConvexPoly(ellipse, pnts, Scalar(0, 255, 0));  // convex faster polygon
//imshow("filledEllipse", ellipse);
//waitKey();

// 9
fillPoly(ellipse, pnts, Scalar(255, 0, 255));
//imshow("filledEllipse", ellipse);
//waitKey();

// 10
double fontSize = getFontScaleFromHeight(FONT_HERSHEY_COMPLEX, 20, 1);
//cout << fontSize << endl;


// 11
//string test = "hello yy";
//int baseline = 0;
//Size textSize = getTextSize(test, FONT_HERSHEY_SCRIPT_COMPLEX, 2, 3, &baseline);
//baseline += 3;
//Point textOrg((src.cols - textSize.width) / 2, (src.rows + textSize.height) / 2);

// 12
//line(src, textOrg + Point(0, 3), textOrg + Point(textSize.width, 3), Scalar(0, 0, 255));

// 13 polylines()
Mat canvas = Mat::zeros(Size(500, 500), CV_8UC3);
vector<Point> pts;
pts.push_back(Point(200, 200));
pts.push_back(Point(200, 300));
pts.push_back(Point(300, 300));
pts.push_back(Point(300, 200));

const Point* ptsPtr[1] = { &pts.at(0) };
const int npt[] = { 4 };
//polylines(canvas, pts, true, Scalar(0, 0, 255));
polylines(canvas, ptsPtr, npt, 1, true, Scalar(0, 0, 255));
imshow("src", canvas);
waitKey();

LineIterator it(src, Point(20, 20), Point(20, 50));
vector<Vec3b> buf(it.count);
for (int i = 0; i < it.count; i++, ++it) {
	buf[i] = *(const Vec3b*)*it;
}

for (int i = 0; i < it.count; i++, ++it) {
	Vec3b val = src.at<Vec3b>(it.pos());
}


// 14
putText(src, test, textOrg, FONT_HERSHEY_SCRIPT_COMPLEX, 2, Scalar(0, 255, 255));

```

```cpp
15 rectangle(src, 
			 textOrg + Point(0, baseline), 
			 textOrg + Point(textSize.width, -textSize.height), 
			 Scalar(0, 0, 255));
```