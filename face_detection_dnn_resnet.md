## 1. NuGet에서 "opencv4.2" 설치하기

✅ Visual Studio 메뉴:

```
솔루션 탐색기 → 프로젝트 우클릭 → NuGet 패키지 관리 → 찾아보기
```

### 검색창에:
```
opencv4.2
```
(※ 그냥 "opencv" 말고 **"opencv4"** 로 정확히 검색해야 합니다.)

**설치할 것:**
- `opencv4` (작성자가 **shimat**인 패키지)

✔️ 이걸 설치하면  
✔️ C++용 헤더파일과 라이브러리(.lib), DLL 파일이 다 자동 세팅됩니다.

---

## 2. NuGet 설치하면 자동으로 설정되는 것

- C++ 추가 포함 디렉토리: `opencv2` 폴더 연결
- 링커 추가 라이브러리 디렉토리: `opencv4xx.lib`, `opencv_dnn4xx.lib` 등 연결
- DLL 파일 위치: 프로젝트 `packages` 폴더에 생성

**👉 별도로 Include/Linker 설정 안 해도 됩니다.**  
(단, 실행파일과 같은 폴더에 DLL 복사만 필요)

---

## 3. 플랫폼을 x64로 설정하기 (중요)

**프로젝트 속성 → 구성 관리자 → 솔루션 플랫폼 ➔ `x64`로 설정**

(기본은 x86으로 되어 있는데, OpenCV NuGet 설치한 건 64비트 버전이기 때문에 꼭 맞춰야 합니다.)

---

## 4. 테스트 코드 (C++용)

NuGet 설치하고 바로 쓸 수 있는 **C++ 얼굴 검출 기본 예제** 드릴게요.

```cpp
#include <opencv2/opencv.hpp>
#include <opencv2/dnn.hpp>

using namespace cv;
using namespace cv::dnn;
using namespace std;

int main()
{
    // 네트워크 모델 로드
    Net net = readNetFromCaffe("deploy.prototxt", "res10_300x300_ssd_iter_140000.caffemodel");

    // 이미지 읽기
    Mat img = imread("test.jpg");
    if (img.empty()) {
        cout << "Image load failed!" << endl;
        return -1;
    }

    // Blob 변환
    Mat blob = blobFromImage(img, 1.0, Size(300, 300), Scalar(104.0, 177.0, 123.0));

    // 네트워크 입력
    net.setInput(blob);

    // 추론
    Mat detection = net.forward();

    // 결과 처리
    Mat detectionMat(detection.size[2], detection.size[3], CV_32F, detection.ptr<float>());

    for (int i = 0; i < detectionMat.rows; i++) {
        float confidence = detectionMat.at<float>(i, 2);

        if (confidence > 0.5) { // 신뢰도 threshold
            int x1 = static_cast<int>(detectionMat.at<float>(i, 3) * img.cols);
            int y1 = static_cast<int>(detectionMat.at<float>(i, 4) * img.rows);
            int x2 = static_cast<int>(detectionMat.at<float>(i, 5) * img.cols);
            int y2 = static_cast<int>(detectionMat.at<float>(i, 6) * img.rows);

            rectangle(img, Point(x1, y1), Point(x2, y2), Scalar(0, 255, 0), 2);
        }
    }

    // 출력
    imshow("Face Detection", img);
    waitKey(0);

    return 0;
}
```
## 📍 필수 모델 파일 준비하기
주신 코드에서는 **모델 파일**이 필요합니다:

| 파일명 | 설명 |
|:---|:---|
| `deploy.prototxt` | 네트워크 구조 정의 파일 |
| `res10_300x300_ssd_iter_140000.caffemodel` | 학습된 가중치 파일 |

### 다운로드 링크:
- [`deploy.prototxt`](https://github.com/opencv/opencv/blob/4.x/samples/dnn/face_detector/deploy.prototxt)
- [`res10_300x300_ssd_iter_140000.caffemodel`](https://github.com/opencv/opencv_3rdparty/raw/dnn_samples_face_detector_20170830/res10_300x300_ssd_iter_140000.caffemodel)

---

# 🛠️ 마무리 체크리스트

| 항목 | 체크 |
|:---|:---|
| NuGet에서 **opencv4** 설치했는가? | ✅ |
| 프로젝트 플랫폼을 **x64**로 바꿨는가? | ✅ |
| `deploy.prototxt`, `caffemodel` 파일 준비했는가? | ✅ |
| DLL 파일을 exe 폴더에 복사했는가? | ✅ |

---
