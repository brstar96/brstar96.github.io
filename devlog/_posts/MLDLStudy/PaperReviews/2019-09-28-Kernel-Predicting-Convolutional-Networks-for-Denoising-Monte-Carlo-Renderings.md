---
title: "(PaperReview) Kernel Predicting Convolutional Networks For Denoising Monte Carlo Renderings"
tags: 
  - Deep Learning
  - Realtime 3D Rendering
  - Paper Review
categories:
  - PaperReview
toc: false
author_profile: false
comments: 
  provider: "disqus"
  disqus:
    shortname: "https-brstar96-github-io"
use_math: true
header:
  teaser: /assets/Images/paper-reviewkernel-predictingconvolutionalnetworksfordenoisingmontecarlorenderings-1-638.jpg
---
<center>
<iframe src="//www.slideshare.net/slideshow/embed_code/key/Fybg7X1zg3p5Tq" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe></center><br>

<Blockquote><span style="font-size:11pt">review date: 2017/12/5 (by Meyong-Gyu.LEE @Soongsil Univ.)</span></Blockquote>

<span style="font-size:11pt">
SIGGRAPH 2017에는 딥러닝을 사용한 논문들이 굉장히 많이 발표되었습니다. 그중 'Kernel Predicting Convolutional Networks for Denoising Monte Carlo Renderings'는 CNN(Convolutional Neural Network)을 사용하여 General하고 Complex한 상황에 대응하는 Filtering Kernel을 찾아내는 과정에 대해 이야기하고 있습니다. 대학원 입학 후 첫 세미나 리뷰 논문이었는데, 수식은 물론이거니와 아예 인공지능 관련 지식이 없어 고생이 많았던 논문입니다. 지금 보면 이걸 왜 그렇게 쩔쩔멨나 싶으면서도 2년 사이에 엄청난 속도로 발전한 논문들의 퀄리티에 다시 마음을 다잡게 되네요.<br><br>
Convolution은 기본적으로 인풋 이미지에 필터를 씌워 값을 도출하는 방법인데, 이 필터의 값을 가중치라고 합니다. 본격적인 이야기를 시작하기에 앞서, 몬테카를로 메소드(Monte Carlo Method)는 난수를 이용해 함수의 값을 확률적으로 계산하는 적분법을 의미합니다. 수학이나 물리학에서 자주 사용되는 개념이며 계산하려는 값이 닫힌 형식으로 표현되지 않거나 복잡한 경우 근사적으로 계산할 때 사용됩니다. 몬테카를로 렌더링(이하 MC 렌더링)은 사진과 흡사한 이미지를 렌더링하는 데 널리 쓰이는 방식으로, 렌더링 과정에서 고품질의 이미지를 얻으려면 픽셀 당 샘플(spp)의 수를 증가시켜야 하며 이는 곧 렌더링 시간의 증가로 이어지게 됩니다. 따라서 러프하게 MC 렌더링을 걸고 출력 영상에 발생한 노이즈를 신경망 학습을 통해 제거하겠다는 것이 본 논문의 주 목표입니다. 시간과 품질을 모두 챙기면서도 noisy한 입력에 대해 오버피팅되지 않음과 동시에 기존의 회귀 방식 기반 Denoiser들처럼 단일 입력만 처리하지 않으며 노이즈가 많아도 고품질의 Denoising 결과를 얻고자 합니다. <br><br>
Vray나 Corona, Renderman 같은 상업 렌더러들은 대부분 디노이저를 제공하지만 회귀 방식 기반이라 오버피팅되는 경향이 있습니다. 이 문제를 해결하기 위해 저자들은 노이즈가 낀 인풋 렌더링과 그에 일대일 대응하는 레퍼런스 데이터(높은 spp를 갖는 말끔한 이미지) 간의 복잡한 매핑을 학습시키고 기존 디노이저들처럼 단일 입력만 처리하는 게 아니라 학습된 전체 이미지 정보를 활용해 비선형 모델을 만들었습니다. 본 논문의 주된 Contribution은 다음과 같습니다. <br>
</span>

- <span style="font-size:11pt">MC 렌더링에 대해 최신의 Denoiser보다 좋은 딥러닝 기반 디노이징 알고리즘 제공</span>
- <span style="font-size:11pt">Locally optimal neighborhood weights를 계산하는 CNN 기반 Kernel predicting 아키텍쳐를 제안</span>
- <span style="font-size:11pt">이미지에서 Diffuse와 Specular의 노이즈를 개별적으로 제거하기 위한 2-network framework를 제안(각 채널에 대한 정규화 수행)</span>
- <span style="font-size:11pt">HDR 이미지에 대해 본 논문의 기술을 접목해 품질을 크게 개선</span>

<span style="font-size:11pt">
CNN은 다차원 구조의 인풋 데이터에 대해 대응이 가능하며, Convolution layer를 깊게 여러 겹 쌓으면 더 복잡하고 추상화된 정보를 추출할 수 있기 때문입니다. 완전연결(fully-connected) 신경망을 사용하게 되면 이미지 입력 시 1차원으로 데이터가 한번 변환되기 때문에 데이터의 형상이 무시되는 문제가 있습니다. 그러나 CNN은 입력과 출력의 포맷이 동일하기 때문에 이미지처럼 형상을 가진 데이터를 제대로 이해할 수 있어서 본 연구에 적합합니다. CNN에서는 합성곱 계층의 입출력 데이터를 feature map이라고 하는데, 본 논문의 CNN은 합성곱 계층을 이용해 디노이징을 위한 필터 즉 output feature map(=kernel)을 찾아 내는 것이 주 목적입니다.<br><br>
네트워크 학습은 '도리를 찾아서'의 600 프레임으로 진행했습니다. 이 학습 데이터 샘플은 다양한 특수 효과들을 포함하고 있기 때문에 효과들에 대해서도 학습이 진행되어 오버피팅을 방지할 수 있게 됩니다. 32spp와 128spp를 갖는 러프한 이미지를 1024spp의 레퍼런스 이미지에 대해 학습시켰습니다. 이후 다양한 특수효과들을 포함해서 렌더링된 'Car 3'과 'Coco'의 25프레임들에 대해 테스트를 진행했습니다. 테스트 결과는 논문의 fig.4에서 보실 수 있습니다. <br><br>
본 논문은 MC 렌더링을 통해 러프한 렌더링을 뽑고 그 결과물에 대해 딥러닝을 적용한 디노이징을 수행함으로서 프로덕션 시간과 비용을 줄이는 것이 목표입니다. Deep CNN을 활용한 디노이징의 첫 번째 성공적인 결과물로서, 프로덕션 환경에 적용해도 좋을 만큼 빠르면서도 안정적인 솔루션을 제공하고 있습니다.<br> 
</span> 