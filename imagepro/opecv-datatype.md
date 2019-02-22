# Filters and Convolution

### Making Border

``` c++
void cv::copyMakeBorder(
 cv::InputArray src, // Input image
 cv::OutputArray dst, // Result image
 int top, // Top side padding (pixels)
 int bottom, // Bottom side padding (pixels)
 int left, // Left side padding (pixels)
 int right, // Right side padding (pixels)
 int borderType, // Pixel extrapolation method
 const cv::Scalar& value = cv::Scalar() // Used for constant borders
);
```

### Threshold Operations

``` c++
double cv::threshold(
 cv::InputArray src, // Input image
 cv::OutputArray dst, // Result image
 double thresh, // Threshold value
 double maxValue, // Max value for upward operations
 int thresholdType // Threshold type to use (Example 10-3)
);
```

| Threshold type        | Operations                           |
| --------------------- | ------------------------------------ |
| cv::THRESH_BINARY     | DSTI= (SRCI> thresh) ? MAXVALUE : 0  |
| cv::THRESH_BINARY_INV | DSTI= (SRCI> thresh) ? 0 : MAXVALUE  |
| cv::THRESH_TRUNC      | DSTI= (SRCI> thresh) ? THRESH : SRCI |
| cv::THRESH_TOZERO     | DSTI= (SRCI > thresh) ? SRCI: 0      |
| cv::THRESH_TOZERO_INV | DSTI= (SRCI > thresh) ? 0 : SRCI     |

