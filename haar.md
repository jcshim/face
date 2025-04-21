# 얼굴추출

1. '윈도우 데스크톱 마법사'   Project1
2. 데스크톱 애플리케이션(.exe)
3. [체크] 빈 프로젝트(E) [확인]
4. 메뉴 [프로젝트]>아래 [속성]>링커>고급>진입점: mainCRTStartup [확인]
5. 오른쪽 솔루션 탐색기: 
6. 소스파일 > 마우스 오른버튼>추가[D]>새항목>
7. FileName.cpp을 main.cpp [추가(A)]
8. 메뉴 프로젝트(P)>NuGet 패키지 관리> [찾아보기]탭>'penCV4.2'로 검색>오른쪽 [설치] > 1분 대기

```
#include<OpenCV2/openCV.hpp>
using namespace cv;
int main() {
	Mat img = imread("d:/face/shim.jpg");  // 수정하세요.
	imshow("GKNU", img);
	waitKey(0);
}
```


## [haarcascade파일](https://raw.githubusercontent.com/opencv/opencv/master/data/haarcascades/haarcascade_frontalface_default.xml) 복사해서 
## "haarcascade_frontalface_default.xml" 이름으로 저장(UTF8)

```
#include<OpenCV2/openCV.hpp>
#include<iostream>
using namespace cv;
int main() {
	Mat img = imread("d:/face/shim.jpg"); // 수정하세요.
	CascadeClassifier hc("D:/face/haarcascade_frontalface_default.xml"); // 수정하세요.
	std::vector<Rect> fs;
	hc.detectMultiScale(img, fs);
	for (auto& f : fs) {
		rectangle(img, f, Scalar(255, 0, 0), 2);
	}
	imshow("Faces", img);
	waitKey(0);
}
```
``
