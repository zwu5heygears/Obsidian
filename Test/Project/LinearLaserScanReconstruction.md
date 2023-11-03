# 选型
- 激光选型
1. 激光图案：线性
2. 亮度可调：是
3. 工作距离：
4. 激光线长：100mm
5. 颜色波段：405nm蓝紫光（[鲍威尔棱镜](https://blog.csdn.net/lzy750750/article/details/126479670)）
- 相机选型
1. 相机接口：USB/以太网
2. 像素精度：FOV除以精度乘3倍
3. 黑白彩色：黑白
	USB HKvision 黑白工业相机 2500w 5120x5120 像元4.5umpixel CMOS 35mm对角线
> 选型经验：[CSDN](https://blog.csdn.net/lzy750750/article/details/126479670)
# 相机标定
## 小孔成像坐标系转换：
Lucida
 $(X_w,Y_w,Z_w)\leftarrow (X_c,Y_c,Z_c)\leftarrow (X_p,Y_p)\leftarrow (u,v)$
```c++
void calibCamera(string director, int imageCount, Mat& cameraMatrix, Mat& distCoeffs, vector<Mat>& R, vector<Mat>& T, Mat& Rw, Mat& Tw){
    /* 获取标定图片角点 */
    Mat grayImage;
    for(int i = 0; i < imageCount; i++){
        Mat image = imread(director + to_string(i) + ".jpg");
        cvtColor(image, grayImage, COLOR_BGR2GRAY);
        vector<Point2f> targetPoint;
        cout << "begin" << endl;
        if(0 == findCirclesGrid(grayImage, patternSize, targetPoint)){ // 圆心角点检测
            cout << i << "picture is wrong" << endl;
        }
        else{
            // find4QuadCornerSubpix(image, targetPoint, Size(5,5));
            calibImagePoint.push_back(targetPoint);
            drawChessboardCorners(image, patternSize, targetPoint, true);
            // imwrite("Corner" + to_string(i) + ".jpg", image);
            // imshow("1", image);
            // waitKey();
            cout << i << "picture is good" << endl;
        }
    }
    /* 获取相机对应点 */
    for(int i = 0; i < imageCount; i++){
        vector<Point3f> tempPoint;
        for(int j = 0; j < patternSize.height; j++){
            for(int k = 0; k < patternSize.width; k++){
                Point3f temp;
                temp.x = k * patternLength.width;
                temp.y = j * patternLength.height;
                temp.z = 0;
                tempPoint.push_back(temp);
            }
        }
        calibBoardPoint.push_back(tempPoint);
        tempPoint.clear();
    }
    /* 获取相机内参和外参 畸变系数 */
    double rms = calibrateCamera(calibBoardPoint, calibImagePoint, imageSize, cameraMatrix, distCoeffs, R, T);
    cout << "opencv RMS error" << rms << endl;
    Rw = R[0], Tw = T[0];
    /* 重投影获取标定误差 */
    double err = 0.0;
    double meanerr = 0.0;
    double totalerr = 0.0;
    double reProjectionError; // 误差值
    vector<Point2f> reProjectionPoint;
    for(int i = 0; i < calibBoardPoint.size(); i++){
        vector<Point3f> tempPointSet = calibBoardPoint[i];
        projectPoints(tempPointSet, R[i], T[i], cameraMatrix, distCoeffs, reProjectionPoint);
        vector<Point2f> tempImagePoint = calibImagePoint[i];
        Mat tempImagePointMat = Mat(1, tempImagePoint.size(), CV_32FC2);
        Mat imagePointMat = Mat(1, reProjectionPoint.size(), CV_32FC2);
        for(int j = 0; j < tempImagePoint.size(); j++){
            imagePointMat.at<Vec2f>(0, j) = Vec2f(reProjectionPoint[j].x, reProjectionPoint[j].y);
            tempImagePointMat.at<Vec2f>(0, j) = Vec2f(tempImagePoint[j].x, tempImagePoint[j].y);
        }
        err = norm(imagePointMat, tempImagePointMat, NORM_L2);
        meanerr = err / (patternSize.width * patternSize.height);
        totalerr += meanerr;
    }

    reProjectionError = totalerr / calibBoardPoint.size();  // 此误差为非开方误差，与Openc自带的RMS error不同
    cout << "Rw:" << Rw.at<int>(0,0) << endl;
    cout << "Tw:" << Tw.at<int>(0,0) << endl;
    cout << "error:" << reProjectionError << endl;
}
```
# 中心线提取
[1]([https://blog.csdn.net/taifyang/article/details/123869047](https://blog.csdn.net/taifyang/article/details/123869047))
[2]([https://blog.csdn.net/AAAA202012/article/details/126056366](https://blog.csdn.net/AAAA202012/article/details/126056366))
[3]([https://blog.csdn.net/jiangxing11/article/details/120056536](https://blog.csdn.net/jiangxing11/article/details/120056536))
```C++
void stegerLine(string director, int imageCount, )
```

# 激光标定（光平面拟合）
$$Ax+By+Cz+D=0$$
[URL]([https://blog.csdn.net/weixin_39904522/article/details/116515525](https://blog.csdn.net/weixin_39904522/article/details/116515525))
```cpp
void lightPlanFitting(int imageCount, vector<vector<Point2f>> lightPlanSubPixelImagePoint){

}
```

# 点云计算和显示
[[PCL]]
```cpp
void calcWordPoint(){
	FileStorge fs;
	fs.open("D/VSCodeProjects/result")
}
```
## 点云图
![[Untitled.png]]
# 完整代码
