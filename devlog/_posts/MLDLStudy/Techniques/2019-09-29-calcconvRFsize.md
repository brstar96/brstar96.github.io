---
layout: post
title: Convolution output size와 RF(Receptive Field)사이즈 계산하기
tags: [DL, CV, CNN, Receptive Field]
categories: [MLDLStudy]
comments: true
sitemap: true
image: /assets/img/devlog/MLDLStudy/convsize.png
accent_image: 
  background: url('/assets/img/sidebar-bg.gif') center/cover
  overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  Convolution output size와 RF(Receptive Field) Size를 계산하는 공식입니다. 
related_posts:
    - /devlog/_posts/Event&Seminar/2019-02-23-NAVERVisionAIHack.md
---
## Calculate CNN Output Size

$$ O=\frac{I-K+2P}{S}+1 $$
<br>

$$
\begin{align*}
O: \text{Size of output image}& \\
I: \text{Size of input image}& \\ 
K: \text{Size of kernels used in the convolution layer}& \\ 
N: \text{Number of kernels}& \\
S: \text{Stride of the convolution layer}& \\
P: \text{Padding size}& \\
\end{align*}
$$

### AlexNet 예시
  - AlexNet의 입력 이미지 크기를 227 x 227 x 3이라 하고 첫 번째 convolution layer(`conv_1`)가 11 x 11 x 3 `kernel` 96개, `stride = 4`, `padding = 0`일 경우 : $$ O=\frac{227 - 11 + 2 * 0}{4}+1=55 $$
  - 따라서 `conv_1`의 출력 텐서 사이즈는 55 x 55 x 96이며, 3채널(RGB) 이미지이기 때문에 3이 곱해져 총 55 x 55 x 96(kernel개수) x 3이 최종 출력 사이즈가 됨.
<br><br>

## Calculate Receptive Field Size
- <b>$InputSize=(OutputSize\ -\ 1)\times KernelStride+KernelSize.$</b>
  - <b><i>$ReceptiveField$</i></b> : Size of the receptive field can be reversed from the output network size.<br>
  - <b><i>$InputSize$</i></b> : Size of the sense node of the output node. <br>
  - <b><i>$KernelStride$</i></b> : Moving step size of the convolutional kernel.<br>
  - <b><i>$KSize$</i></b> : Size of the convolution kernel between input and output.<br>
<br><br>

## 같이 보면 좋은 글
- [(Stack Overflow) CS231n: Total memory of VGGnet](https://stackoverflow.com/questions/49423323/cs231n-total-memory-of-vggnet)