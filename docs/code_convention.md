# 코드 컨벤션 (Code Convention)

[Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html) ([한글 번역](https://jongwook.kim/google-styleguide/trunk/cppguide.xml))를 기반으로 합니다.

## 개요

본 프로젝트는 C++ 기반의 비전 개발 키트로, AI 모델 추론 및 컴퓨터 비전 처리를 위한 코드 스타일을 정의합니다.

## 에디터 설정

### VSCode 설정

`.vscode/settings.json`:

```json
{
    "C_Cpp.clang_format_fallbackStyle": {
        "BasedOnStyle": "Google",
        "IndentWidth": 4,
        "TabWidth": 4,
        "UseTab": "Never",
        "ColumnLimit": 0,
        "SpacesBeforeTrailingComments": 4,
        "AlignTrailingComments": true
    },
    "editor.tabSize": 4,
    "editor.insertSpaces": true,
    "files.encoding": "utf8",
    "files.eol": "\n"
}
```

### .editorconfig

프로젝트 최상위 폴더에 `.editorconfig`:

```ini
root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true

[*.{cpp,hpp,c,h}]
indent_style = space
indent_size = 4
tab_width = 4

[CMakeLists.txt]
indent_style = space
indent_size = 4

[*.cmake]
indent_style = space
indent_size = 4

[*.{md,txt}]
trim_trailing_whitespace = false
```

### .clang-format

프로젝트 최상위 폴더에 `.clang-format`:

```yaml
---
Language: Cpp
BasedOnStyle: Google

# 들여쓰기
IndentWidth: 4
TabWidth: 4
UseTab: Never
ContinuationIndentWidth: 4

# 줄 길이
ColumnLimit: 0

# 공백
SpacesBeforeTrailingComments: 4
AlignTrailingComments: true
SpaceBeforeParens: ControlStatements
SpaceInEmptyParentheses: false
SpacesInParentheses: false
SpacesInAngles: false

# 중괄호
BreakBeforeBraces: Attach
AllowShortBlocksOnASingleLine: Empty
AllowShortFunctionsOnASingleLine: Inline
AllowShortIfStatementsOnASingleLine: Never
AllowShortLoopsOnASingleLine: false

# 포인터 및 레퍼런스
DerivePointerAlignment: false
PointerAlignment: Left

# 기타
SortIncludes: true
IncludeBlocks: Regroup
...
```

### .clang-tidy

프로젝트 최상위 폴더에 `.clang-tidy`:

```yaml
---
Checks: >
  -*,
  bugprone-*,
  cppcoreguidelines-*,
  modernize-*,
  performance-*,
  readability-*,
  -modernize-use-trailing-return-type,
  -readability-identifier-length,
  -cppcoreguidelines-avoid-magic-numbers,
  -readability-magic-numbers,
  -cppcoreguidelines-pro-bounds-pointer-arithmetic,
  -cppcoreguidelines-pro-type-reinterpret-cast

WarningsAsErrors: ''

HeaderFilterRegex: '.*'

CheckOptions:
  - key: readability-identifier-naming.ClassCase
    value: CamelCase
  - key: readability-identifier-naming.FunctionCase
    value: camelBack
  - key: readability-identifier-naming.VariableCase
    value: lower_case
  - key: readability-identifier-naming.MemberCase
    value: lower_case
  - key: readability-identifier-naming.MemberSuffix
    value: '_'
  - key: readability-identifier-naming.ConstantCase
    value: CamelCase
  - key: readability-identifier-naming.ConstantPrefix
    value: 'k'
...
```

## 파일 및 폴더 네이밍

### 규칙

- **소문자 snake_case** 사용
- 띄어쓰기가 필요한 경우 언더바(`_`) 사용
- C++ 헤더 파일: `*.hpp`
- C 헤더 파일: `*.h`
- C++ 소스 파일: `*.cpp`
- C 소스 파일: `*.c`
- **모든 파일 인코딩: UTF-8**
- **줄바꿈 방식: LF** (`\n`)

### 예시

```shell
src/
├── vision_development_kit/
│   ├── base_vision_engine.hpp
│   ├── base_vision_engine.cpp
│   ├── face_detector.hpp
│   └── face_detector.cpp
├── engine/
│   ├── ov_vision_engine/
│   │   ├── ov_engine.hpp
│   │   └── ov_engine.cpp
│   └── rk_vision_engine/
│       ├── rk_engine.hpp
│       └── rk_engine.cpp
└── utils/
    ├── image_utils.hpp
    └── image_utils.cpp
```

## 헤더 파일

### Header Guard

`#pragma once`와 전통적인 Header Guard를 함께 사용:

```cpp
#pragma once
#ifndef BASE_VISION_ENGINE_HPP_
#define BASE_VISION_ENGINE_HPP_

// 헤더 내용

#endif  // BASE_VISION_ENGINE_HPP_
```

### 기본 구조

```cpp
#pragma once
#ifndef FACE_DETECTOR_HPP_
#define FACE_DETECTOR_HPP_

#include <vector>
#include <opencv2/opencv.hpp>

// 클래스 선언
class FaceDetector {
private:
    // 멤버 변수
    int model_input_size_;
    float confidence_threshold_;
    std::vector<float> mean_values_;

public:
    // 생성자/소멸자
    FaceDetector();
    ~FaceDetector();

    // 멤버 함수
    bool initialize(const std::string& model_path);
    std::vector<BoundingBox> detect(const cv::Mat& image);
    void setConfidenceThreshold(float threshold);
    float getConfidenceThreshold() const;

private:
    // 내부 헬퍼 함수
    cv::Mat preprocessImage(const cv::Mat& image);
    std::vector<BoundingBox> postprocessOutput(const float* output);
};

#endif  // FACE_DETECTOR_HPP_
```

## 소스 파일

### 기본 구조

```cpp
#include "face_detector.hpp"

#include <algorithm>
#include <cmath>

// 생성자
FaceDetector::FaceDetector()
    : model_input_size_(640),
      confidence_threshold_(0.5f),
      mean_values_({0.485f, 0.456f, 0.406f}) {
    // 초기화 코드
}

// 소멸자
FaceDetector::~FaceDetector() {
    // 정리 코드
}

// 멤버 함수 구현
bool FaceDetector::initialize(const std::string& model_path) {
    // 구현
    return true;
}

std::vector<BoundingBox> FaceDetector::detect(const cv::Mat& image) {
    // 구현
    std::vector<BoundingBox> results;
    return results;
}

// Getter/Setter
void FaceDetector::setConfidenceThreshold(float threshold) {
    confidence_threshold_ = threshold;
}

float FaceDetector::getConfidenceThreshold() const {
    return confidence_threshold_;
}

// Private 함수
cv::Mat FaceDetector::preprocessImage(const cv::Mat& image) {
    // 구현
    cv::Mat processed;
    return processed;
}
```

## 네이밍 규칙

### 클래스명: PascalCase

```cpp
class VisionEngine { };
class FaceDetector { };
class ObjectTracker { };
class BaseVisionEngine { };
```

### 함수명: camelCase (첫 글자 소문자)

```cpp
class FaceDetector {
public:
    bool initialize();
    std::vector<Face> detectFaces(const cv::Mat& image);
    void setModelPath(const std::string& path);
    std::string getModelPath() const;

private:
    cv::Mat preprocessImage(const cv::Mat& image);
    void postprocessResults(std::vector<Face>& faces);
};
```

### 변수명: snake_case

```cpp
// 로컬 변수
int image_width = 640;
float confidence_threshold = 0.5f;
std::string model_path = "/path/to/model";
cv::Mat input_image;

// 함수 파라미터
void processImage(const cv::Mat& input_image,
                  float confidence_threshold,
                  std::vector<Face>& detected_faces);
```

### 클래스 멤버 변수: snake_case + 접미사 `_`

```cpp
class FaceDetector {
private:
    int model_input_size_;
    float confidence_threshold_;
    std::string model_path_;
    cv::Mat preprocessed_image_;
    std::vector<float> mean_values_;
    bool is_initialized_;
};
```

### 상수: kPascalCase

```cpp
const int kDefaultImageSize = 640;
const float kDefaultConfidenceThreshold = 0.5f;
const std::string kDefaultModelPath = "/models/default.onnx";

constexpr int kMaxBatchSize = 32;
constexpr float kMinConfidence = 0.1f;
```

### 열거형: PascalCase, 값은 kPascalCase

```cpp
enum class Status {
    kSuccess = 0,
    kErrorUnknown = -1,
    kErrorInvalidInput = -2,
    kErrorModelNotLoaded = -3,
    kErrorInference = -4
};

enum class ModelType {
    kFaceDetection,
    kObjectDetection,
    kPoseEstimation,
    kSegmentation
};
```

### 인터페이스 클래스: 접두사 `I`

```cpp
class IVisionEngine {
public:
    virtual ~IVisionEngine() = default;
    virtual bool initialize(const std::string& model_path) = 0;
    virtual Status inference(const cv::Mat& input, cv::Mat& output) = 0;
};

class IPreprocessor {
public:
    virtual ~IPreprocessor() = default;
    virtual cv::Mat preprocess(const cv::Mat& image) = 0;
};
```

## Namespace 규칙

### using 선언 금지

**`using namespace`를 사용하여 namespace prefix를 생략하는 것을 금지합니다.**

```cpp
// ❌ 절대 사용 금지
using namespace std;
using namespace cv;

std::vector<int> vec;     // ❌ (using namespace std 사용 시)
vector<int> vec;          // ❌ prefix 생략
cv::Mat img;              // ❌ (using namespace cv 사용 시)
Mat img;                  // ❌ prefix 생략

// ✅ 올바른 예시 - 항상 전체 namespace 명시
std::vector<int> vec;
std::string name;
cv::Mat img;
cv::Rect bbox;
```

### using 선언이 금지되는 이유

1. **이름 충돌 방지**: 서로 다른 라이브러리에서 같은 이름의 클래스/함수가 있을 수 있음
2. **가독성 향상**: 코드만 봐도 어느 namespace의 것인지 명확히 알 수 있음
3. **유지보수 용이**: 나중에 코드를 읽을 때 출처가 명확함

```cpp
// ❌ 잘못된 예시 - 이름 충돌 가능성
using namespace std;
using namespace boost;

vector<int> data;    // std::vector? boost::vector? 불명확!

// ✅ 올바른 예시 - 명확함
std::vector<int> std_data;
boost::container::vector<int> boost_data;
```

### Namespace 사용 예시

```cpp
// 헤더 파일
#pragma once
#ifndef IMAGE_PROCESSOR_HPP_
#define IMAGE_PROCESSOR_HPP_

#include <vector>
#include <string>
#include <opencv2/opencv.hpp>

// ❌ 헤더에서 using namespace 금지
// using namespace std;
// using namespace cv;

class ImageProcessor {
public:
    // ✅ 전체 namespace 명시
    bool loadImage(const std::string& path);
    cv::Mat processImage(const cv::Mat& input);
    std::vector<cv::Rect> detectObjects(const cv::Mat& image);

private:
    cv::Mat current_image_;
    std::vector<float> parameters_;
};

#endif  // IMAGE_PROCESSOR_HPP_
```

```cpp
// 소스 파일
#include "image_processor.hpp"

#include <algorithm>
#include <cmath>

// ❌ 소스 파일에서도 using namespace 금지
// using namespace std;
// using namespace cv;

bool ImageProcessor::loadImage(const std::string& path) {
    current_image_ = cv::imread(path);    // ✅ cv:: 명시
    return !current_image_.empty();
}

cv::Mat ImageProcessor::processImage(const cv::Mat& input) {
    cv::Mat output;
    cv::cvtColor(input, output, cv::COLOR_BGR2GRAY);    // ✅ cv:: 명시
    return output;
}

std::vector<cv::Rect> ImageProcessor::detectObjects(const cv::Mat& image) {
    std::vector<cv::Rect> objects;    // ✅ std::와 cv:: 명시
    // 처리 로직
    return objects;
}
```

### 예외: 타입 별칭 (Type Alias)

특정 타입이 너무 길어서 가독성이 떨어질 경우, **타입 별칭**은 허용됩니다:

```cpp
// ✅ 허용 - 타입 별칭 (using 타입명 = ...)
using StringVector = std::vector<std::string>;
using ImageMap = std::unordered_map<std::string, cv::Mat>;
using BoundingBoxList = std::vector<cv::Rect>;

// 사용
StringVector file_paths;
ImageMap image_cache;
BoundingBoxList detected_boxes;
```

하지만 이것도 남용하지 말고, 정말 필요한 경우에만 사용합니다.

## 클래스 구조

### 멤버 순서

1. 멤버 변수 (private → protected → public)
2. 생성자/소멸자 (public)
3. Public 멤버 함수
4. Protected 멤버 함수
5. Private 멤버 함수

```cpp
class FaceDetector {
private:
    // Private 멤버 변수
    int model_input_size_;
    float confidence_threshold_;
    bool is_initialized_;

protected:
    // Protected 멤버 변수
    std::string model_path_;

public:
    // Public 멤버 변수 (가급적 사용 지양)

public:
    // 생성자/소멸자
    FaceDetector();
    explicit FaceDetector(const std::string& model_path);
    virtual ~FaceDetector();

    // Public 멤버 함수
    bool initialize(const std::string& model_path);
    std::vector<Face> detect(const cv::Mat& image);

    // Getter/Setter
    void setConfidenceThreshold(float threshold);
    float getConfidenceThreshold() const;

protected:
    // Protected 멤버 함수
    virtual cv::Mat preprocess(const cv::Mat& image);

private:
    // Private 멤버 함수
    void validateInput(const cv::Mat& image);
    std::vector<Face> postprocess(const float* output, int output_size);
};
```

## CMakeLists.txt 작성 규칙

### CMake 명령어: 모두 대문자

```cmake
CMAKE_MINIMUM_REQUIRED(VERSION 3.24)
PROJECT(SecernVisionDevelopmentKit VERSION 0.2.0 LANGUAGES CXX)

SET(CMAKE_CXX_STANDARD 17)
SET(CMAKE_CXX_STANDARD_REQUIRED ON)

# 변수명: 대문자 스네이크 케이스
SET(TARGET_NAME SecernVisionDevelopmentKit)
SET(SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src)
SET(INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/include)

# 소스 파일 수집
FILE(GLOB_RECURSE SOURCE_FILES
    ${SOURCE_DIR}/*.cpp
    ${SOURCE_DIR}/*.c
)

FILE(GLOB_RECURSE HEADER_FILES
    ${INCLUDE_DIR}/*.hpp
    ${INCLUDE_DIR}/*.h
)

# 타겟 생성
ADD_LIBRARY(${TARGET_NAME} SHARED ${SOURCE_FILES})

TARGET_INCLUDE_DIRECTORIES(${TARGET_NAME}
    PUBLIC
        ${INCLUDE_DIR}
    PRIVATE
        ${SOURCE_DIR}
)

TARGET_LINK_LIBRARIES(${TARGET_NAME}
    PUBLIC
        opencv_core
        opencv_imgproc
)

# 설치
INSTALL(TARGETS ${TARGET_NAME}
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
)

INSTALL(FILES ${HEADER_FILES}
    DESTINATION include/${TARGET_NAME}
)
```

### 파일명: 소문자 snake_case

```cmake
# ✅ 올바른 예시
ADD_SUBDIRECTORY(base_vision_engine)
ADD_SUBDIRECTORY(ov_vision_engine)
INCLUDE(config_opencv.cmake)
INCLUDE(config_openvino.cmake)

# ❌ 잘못된 예시
ADD_SUBDIRECTORY(BaseVisionEngine)
ADD_SUBDIRECTORY(OVVisionEngine)
INCLUDE(ConfigOpenCV.cmake)
```

## 코드 스타일

### 들여쓰기

- **4칸 스페이스 사용 권장**
- 탭 사용 시 **탭 = 4칸**으로 설정
- 에디터 설정에서 `tab_width = 4` 지정

```cpp
class FaceDetector {
public:
    bool detect(const cv::Mat& image) {
        if (image.empty()) {
            return false;
        }

        for (int i = 0; i < image.rows; ++i) {
            for (int j = 0; j < image.cols; ++j) {
                // 처리
            }
        }

        return true;
    }
};
```

### 줄바꿈

- **LF** (`\n`) 사용 (Windows/Linux/Mac 호환)
- `.editorconfig`와 `.gitattributes`로 통일

### 인코딩

- **UTF-8** 사용 (BOM 없음)

### 중괄호

- K&R 스타일 (opening brace는 같은 줄)

```cpp
// ✅ 올바른 예시
if (condition) {
    doSomething();
} else {
    doOthers();
}

for (int i = 0; i < 10; ++i) {
    process(i);
}

class MyClass {
public:
    void myMethod() {
        // 구현
    }
};

// ❌ 잘못된 예시
if (condition)
{
    doSomething();
}
```

### 공백

```cpp
// 연산자 앞뒤 공백
int result = a + b;
bool is_valid = (x > 0) && (y < 100);

// 콤마 뒤 공백
myFunction(arg1, arg2, arg3);

// 주석 앞 공백 (4칸)
int value = 10;    // 설명

// 괄호 안쪽 공백 없음
if (condition) {    // ✅
if ( condition ) {  // ❌
```

### 주석

```cpp
// 한 줄 주석은 // 사용 (코드 위 또는 옆에 작성)
int image_width = 640;    // 입력 이미지 너비

// 여러 줄 주석
/*
 * 여러 줄에 걸친
 * 주석 내용
 */

/**
 * @brief 얼굴을 감지하는 함수
 * @param image 입력 이미지
 * @param threshold 신뢰도 임계값
 * @return 감지된 얼굴 목록
 */
std::vector<Face> detectFaces(const cv::Mat& image, float threshold);
```

### 포인터와 레퍼런스

- `*`와 `&`는 타입에 붙임

```cpp
// ✅ 올바른 예시
int* ptr;
int& ref;
const std::string& name;

// ❌ 잘못된 예시
int *ptr;
int &ref;
const std::string &name;
```

## 모범 사례

### include 순서

1. 해당 헤더 파일 (소스 파일의 경우)
2. C/C++ 표준 라이브러리
3. 서드파티 라이브러리
4. 프로젝트 내부 헤더

```cpp
#include "face_detector.hpp"    // 1. 해당 헤더

#include <algorithm>             // 2. C++ 표준
#include <string>
#include <vector>

#include <opencv2/opencv.hpp>   // 3. 서드파티

#include "base_vision_engine.hpp"    // 4. 프로젝트 내부
#include "types/face.hpp"
```

### 초기화

- 멤버 초기화 리스트 사용

```cpp
class FaceDetector {
private:
    int width_;
    int height_;
    std::string model_path_;

public:
    // ✅ 올바른 예시
    FaceDetector(int width, int height, const std::string& path)
        : width_(width),
          height_(height),
          model_path_(path) {
    }

    // ❌ 잘못된 예시
    FaceDetector(int width, int height, const std::string& path) {
        width_ = width;
        height_ = height;
        model_path_ = path;
    }
};
```

### const 정확성

```cpp
class FaceDetector {
public:
    // const 멤버 함수 (객체 상태 변경 안 함)
    int getWidth() const { return width_; }
    bool isInitialized() const { return is_initialized_; }

    // const 참조 파라미터 (읽기 전용)
    std::vector<Face> detect(const cv::Mat& image);

    // const 포인터
    void processData(const float* data, int size);
};
```

### 스마트 포인터 사용

```cpp
#include <memory>

// ✅ 올바른 예시 - 스마트 포인터
std::unique_ptr<VisionEngine> engine = std::make_unique<OvVisionEngine>();
std::shared_ptr<ModelConfig> config = std::make_shared<ModelConfig>();

// ❌ 지양할 예시 - Raw 포인터
VisionEngine* engine = new OvVisionEngine();  // 메모리 누수 위험
delete engine;
```

## 참고사항

- 행 길이 제한: 없음 (`ColumnLimit: 0`)
- Google C++ Style의 기본 `ColumnLimit`는 80이지만, 본 프로젝트는 제한 없음
- 코드 가독성을 위해 적절한 줄바꿈 권장

## 도구 사용

### Clang-Format

```bash
# 전체 프로젝트 포맷팅
find src -name "*.cpp" -o -name "*.hpp" | xargs clang-format -i

# 특정 파일 포맷팅
clang-format -i src/face_detector.cpp
```

### Clang-Tidy

```bash
# 전체 프로젝트 정적 분석
clang-tidy src/**/*.cpp -- -Iinclude

# 특정 파일 분석
clang-tidy src/face_detector.cpp -- -Iinclude
```

### Markdownlint

```bash
# 전체 마크다운 파일 검사
markdownlint docs/**/*.md

# 특정 파일 검사
markdownlint docs/code_convention.md

# 자동 수정
markdownlint --fix docs/**/*.md
```

## 마크다운 문서 작성 규칙

본 프로젝트의 마크다운 문서는 [markdownlint](https://github.com/DavidAnson/markdownlint) 규칙을 따릅니다.

### .markdownlint.json 설정

프로젝트 루트의 `.markdownlint.json` 참조

### 주요 규칙

- 제목은 `#`로 시작 (ATX 스타일)
- 코드 블록은 언어 지정 필수
- 리스트 들여쓰기는 2칸
- 줄 끝 공백 제거
- 문서 끝에 빈 줄 추가

## 수정 내역

- **2026.02.04**: 최초 작성
