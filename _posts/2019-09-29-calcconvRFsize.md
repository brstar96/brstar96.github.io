---
title: "Convolution output size와 RF(Receptive Field)사이즈 계산하기"
tags: 
  - Deep Learning
  - Computer Vision
  - Convolution
  - Receptive Field
categories:
  - MLDLstudy
toc: true
comments: 
  provider: "disqus"
  disqus:
    shortname: "https-brstar96-github-io"
use_math: true
header:
  teaser: /assets/Images/convsize.png
---

- ## Calculate CNN Output Size
    <Blockquote>
    · <span style="font-size:11pt"><b><i>$O$</i></b> : Size of output image<br></span>
    · <span style="font-size:11pt"><b><i>$I$</i></b> : Size of input image <br></span>
    · <span style="font-size:11pt"><b><i>$K$</i></b> : Size of kernels used in the convolution layer<br></span>
    · <span style="font-size:11pt"><b><i>$N$</i></b> : Number of kernels<br></span>
    · <span style="font-size:11pt"><b><i>$S$</i></b> : Stride of the convolution layer<br></span>
    · <span style="font-size:11pt"><b><i>$P$</i></b> : Padding size<br></span>
    </Blockquote>
    
    <span style="font-size:13pt">$O=\frac{I-K+2P}{S}+1$</span><br>

    - ### AlexNet 예시
        - <span style="font-size:11pt">AlexNet의 입력 이미지 크기를 227 x 227 x 3이라 하고 첫 번째 convolution layer(`conv_1`)가 11 x 11 x 3 `kernel` 96개, `stride = 4`, `padding = 0`일 경우 : $O=\frac{227 - 11 + 2 * 0}{4}+1=55$</span>
        - <span style="font-size:11pt">따라서 `conv_1`의 출력 텐서 사이즈는 55 x 55 x 96이며, 3채널(RGB) 이미지이기 때문에 3이 곱해져 총 55 x 55 x 96(kernel개수) x 3이 최종 출력 사이즈가 됨.</span>
- ## Calculate Receptive Field Size
    <Blockquote>
    · <span style="font-size:11pt"><b><i>$ReceptiveField$</i></b> : Size of the receptive field can be reversed from the output network size.<br></span>
    · <span style="font-size:11pt"><b><i>$InputSize$</i></b> : Size of the sense node of the output node. <br></span>
    · <span style="font-size:11pt"><b><i>$KernelStride$</i></b> : Moving step size of the convolutional kernel.<br></span>
    · <span style="font-size:11pt"><b><i>$KSize$</i></b> : Size of the convolution kernel between input and output.<br></span>
    </Blockquote>
    
    <span style="font-size:13pt">$InputSize=(OutputSize\ -\ 1)\times KernelStride+KernelSize.$</span>
    
- ## 같이 보면 좋은 글
    - <span style="font-size:11pt">[(Stack Overflow) CS231n: Total memory of VGGnet](https://stackoverflow.com/questions/49423323/cs231n-total-memory-of-vggnet)</span> 