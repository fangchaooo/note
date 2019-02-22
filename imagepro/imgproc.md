# Introduction to OpenCV

### Head file intriduction

> #include "opencv2/core/core_c.h"
> Old C data structures and arithmetic routines
> **#include "opencv2/core/core.hpp"**
> **New C++ data structures and arithmetic routines**
> #include "opencv2/flann/miniflann.hpp"
> Approximate nearest neighbor matching functions
> #include "opencv2/imgproc/imgproc_c.h"
> Old C image processing functions
> **#include "opencv2/imgproc/imgproc.hpp"**
> **New C++ image processing functions**
> #include "opencv2/video/photo.hpp"
> Algorithms specific to handling and restoring photographs
> #include "opencv2/video/video.hpp"
> Video tracking and background segmentation routines
> #include "opencv2/features2d/features2d.hpp"
> Two-dimensional feature tracking support
>
> #include "opencv2/objdetect/objdetect.hpp"
> Cascade face detector; latent SVM; HoG; planar patch detector
> #include "opencv2/calib3d/calib3d.hpp"
> Calibration and stereo
> #include "opencv2/ml/ml.hpp"
> Machine learning: clustering, pattern recognition
> #include "opencv2/highgui/highgui_c.h"
> Old C image display, sliders, mouse interaction, I/O
> **#include "opencv2/highgui/highgui.hpp"**
> **New C++ image display, sliders, buttons, mouse, I/O**
> #include "opencv2/contrib/contrib.hpp"
> User-contributed code: flesh detection, fuzzy mean-shift tracking, spin images,
> self-similar features

### Basic

- 图像

  `cv::Mat img`

- 读取图像

  `imread(name,  args)`

- 创建窗口

  `namedWindow(name, size)`

- 展示

  `imshow(name, img)`

- 退出

  `waitkey()`

- 视频

  ``` c++
  cv::namedWindow( "Example3", cv::WINDOW_AUTOSIZE );
  cv::VideoCapture cap;
  cap.open( string(argv[1]) );
  cv::Mat frame;
  for(;;) {
   cap >> frame;
   if( frame.empty() ) break; // Ran out of film
   cv::imshow( "Example3", frame );
   if( cv::waitKey(33) >= 0 ) break;
  }
  ```

- 创建trackbar

  ``` c++
  createTrackbar(const String& trackbarname, const String& winname,
                                int* value, int count,
                                TrackbarCallback onChange = 0,
                                void* userdata = 0);
  void onChangeTrackbar(int ,void* );
  
  # example
  createTrackbar("Position", "Example2_4", &g_slider_position, frames,
   				onTrackbarSlide);
  ```

- 图像变换

  `pyrDown()` 高斯平滑变换

  `cvtColor()` rgb变换

  `Canny()`边缘检查
