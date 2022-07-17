---
layout: post
title: CBIS-DDSM 데이터셋 Simple EDA (上)
tags: [Dataset review, EDA]
categories: [Shoveling]
comments: true
sitemap: true
image: /assets/img/devlog/shoveling/CBISDDSM_EDA/case_merged.jpg
accent_image: 
  background: url('/assets/img/sidebar-bg.gif') center/cover
  overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  CBIS-DDSM은 유방암 Classfication 또는 Detection 분야에서 종종 사용되는 데이터셋입니다. 이번 포스트에서는 개인 연구에 활용하기 위해 개인적으로 EDA를 수행한 노트북을 다룹니다.
related_posts:
    - /devlog/_posts/Event&Seminar/2019-02-23-NAVERVisionAIHack.md
---

CBIS-DDSM은 유방암 Classfication 또는 Detection 분야에서 종종 사용되는 데이터셋입니다. 이번 포스트에서는 개인 연구에 활용하기 위해 개인적으로 EDA를 수행한 노트북을 다룹니다. 보다 상세한 통계 정보를 다루는 하(下)편은 언제가 될 지는 모르겠으나 빠른 시일 내로 업데이트해 보겠습니다.   

## CBIS-DDSM_EDA
CBIS-DDSM 데이터셋에서 제공되는 부가 파일들을 활용해 EDA를 수행해 봅니다. CBIS-DDSM 데이터셋은 이미지 데이터(`.dcm`)와 메타 데이터(`.csv`)로 구성되며, 이미지 데이터는 아래와 같은 내역으로 구성되어 있습니다. 

* Dataset format
    * Modality : MG(Mammography
    * Number of Patients : 6671
    * Number of Studies : 6775
    * Number of Series : 6775
    * Number of Images : 10239
    * Entire image size : 163.6GB

전체 이미지는 6671장으로 구성되어 있으며 각 case별 상세 설명이 기재된 네 개의 `.csv`파일이 제공됩니다. `Abnormality type`은 'Calculation case'와 'Mass case'로 나뉘며, Calculation case는 흰색 반점(spots)이나 얼룩덜룩한 형태(flecks)로 관찰되는 `Abnormality type`을 의미하고, Mass case는 Breast Lumps가 구름처럼 mass하게 퍼져 있는 `Abnormality type`을 의미합니다. 

- `calc_case_description_train_set.csv`: 두 개의 `abnormality type` 중 `calc`(calcification, 석회화 조직)인 case들을 모아 구성된 <b>train set</b> list입니다.
- `calc_case_description_test_set.csv`: 두 개의 `abnormality type` 중 `calc`인 case들을 모아 구성된 <b>test set</b> list입니다.
- `mass_case_description_train_set.csv`: 두 개의 `abnormality type` 중 `mass`(mass lumps, 덩어리 조직)인 case들을 모아 구성된 <b>train set</b> list입니다.
- `mass_case_description_test_set.csv`: 두 개의 `abnormality type` 중 `mass`인 case들을 모아 구성된 <b>test set</b> list입니다.

EDA에 앞서, 데이터셋을 한바퀴 둘러보겠습니다.

## 1. 데이터셋 둘러보기

### 1.1 `Abnormality type`별 feature 탐색
`Abnormality type`의 구분은 파일명에서 `calc`와 `mass` 접두사로 되어 있습니다. 각 사례(case)는 <u>①원본 이미지</u>와, <u>②원본 이미지에 대응되는 segment annotation</u>과, 해당 segment annotation을 포함하는 ③<u>bounding box를 crop한 이미지</u>를 포함하고 있습니다. 이 꼭지에서는 `Abnormality type`별 각 컬럼의 feature가 의미하는 바를 알아 보도록 하겠습니다.

![case_details](/assets/img/devlog/shoveling/CBISDDSM_EDA/case_merged.jpg)


```python
import os
import pandas as pd

# Setting base path
ROOT_PATH = os.path.join(os.getcwd(), 'Dataset_csv')
CBISDDSM_csvPATH = os.path.join(ROOT_PATH, 'CBISDDSM')
MIAS_PATH = os.path.join(ROOT_PATH, 'MIAS')

# Load Dataframe from .csv
calc_case_description_train_set_df = pd.read_csv(os.path.join(CBISDDSM_csvPATH, "calc_case_description_train_set.csv"), index_col=0)
calc_case_description_test_set_df = pd.read_csv(os.path.join(CBISDDSM_csvPATH, "calc_case_description_test_set.csv"), index_col=0)
mass_case_description_train_set_df = pd.read_csv(os.path.join(CBISDDSM_csvPATH, "mass_case_description_train_set.csv"), index_col=0)
mass_case_description_test_set_df = pd.read_csv(os.path.join(CBISDDSM_csvPATH, "mass_case_description_test_set.csv"), index_col=0)
```


```python
# 몇 가지 테스트 및 데이터 타입 조사를 위한 더미 코드입니다.
# print(calc_case_description_train_set_df['abnormality type'].unique())
# print(calc_case_description_test_set_df['abnormality type'].unique())
print(mass_case_description_train_set_df['subtlety'].unique())
print(mass_case_description_train_set_df['abnormality id']['P_00001'])
print(mass_case_description_test_set_df['subtlety'].unique())
```

    [4 3 5 1 2 0]
    P_00001    1
    P_00001    1
    Name: abnormality id, dtype: int64
    [5 4 2 3 1]
    
각 `.csv`파일은 아래와 같은 내용들을 포함하고 있습니다. 각 column에 대한 설명은 다음과 같으며, 보다 자세한 설명은 https://www.nature.com/articles/sdata2017177에서 확인하실 수 있습니다. <i>(중복 feature가 존재하는 경우는 `dataframe['columnname'].unique()`로 유니크한 값만 뽑아 확인)</i>

- <b>`calcification` 타입의 데이터셋 각 컬럼별 설명 (`calc_case_description_train_set_df`, `calc_case_description_test_set_df`)</b>
    - <b>`patient_id`:</b> 신원 정보가 제거된(de-identification) 환자 번호입니다. (e.g. P_00005, P_00013, ...)
    - <b>`breast density`:</b> 유방 조직의 밀도를 구분한 정수형 feature입니다. train csv에는 `1`에서 `4`까지의 int형 feature를, test csv에는 `0`~`4`의 int형 feature를 포함하고 있습니다. (e.g. 3, 4, 1, 2, ...)
    - <b>`left or right breast`:</b> 유방을 촬영한 각도입니다. `LEFT`, `RIGHT`로 구성된 srt형 바이너리 feature입니다. (e.g. LEFT, RIGHT, ...)
    - <b>`image view`:</b> mammography 촬영 방법에 따른 구분입니다. `CC`, `MLO`로 구성된 str형 바이너리 feature입니다. (e.g. CC, MLO, ...)
    - <b>`abnormality id`:</b> 해당 case 안에 anomaly 패치가 몇 개인지 구분하기 위한 id입니다. train csv는 `1`부터 `7`까지의 범위를 갖는 int형 feature를, test csv는 `1`~`5`의 범위를 갖는 int형 feature를 포함하고 있습니다. (e.g. 3, 2, 5, 1, ...)
    - <b>`abnormality type`:</b> `Abnormality type`에 따른 구분입니다. `train`과 `test`로 나뉜 네 개의 `.csv`파일에서 해당 테이블은 파일에 대해 모두 동일한 값을 가지며, `calcification`과 `mass` 두 타입이 존재합니다. 아래 `dataframe.head`를 통해 해당 column 정보를 확인하실 수 있습니다.
    - <b>`calc type`:</b> `calcification` 타입의 lumps(덩어리)에서 나타나는 Cancer diagnostic 정보입니다. 46종으로 구분되어 있으며, str형 feature입니다. (e.g. AMORPHOUS, ROUND_AND_REGULAR-AMORPHOUS, ...)
    - <b>`calc distribution`:</b> `calc`타입의 lumps 조직이 얼마나 뭉쳐 있는지, 퍼져 있는지에 대한 str형 feature입니다. `CLUSTERED`, `LINEAR`, `REGIONAL`, `DIFFUSELY_SCATTERED`, `SEGMENTAL`, `CLUSTERED-LINEAR`, `CLUSTERED-SEGMENTAL`, `LINEAR-SEGMENTAL`, `REGIONAL-REGIONAL`로 구성되어 있으며, `nan`값도 포함되어 있으므로 핸들링에 주의가 필요합니다. (e.g. CLUSTERED-LINEAR, SEGMENTAL, CLUSTERED, ...)
    - <b>`assessment`:</b> 비정상 정도에 대한 레벨을 구분하는 정수형 feature입니다. [BI-RADS](https://en.wikipedia.org/wiki/BI-RADS)에 따른 각 사례별 분류로, `0`, `2`, `3`, `4`, `5`의 unique한 int형 features가 포함되어 있습니다.
    - <b>`pathology`:</b> 해당 lumps에 대한 병리학적 분류를 의미합니다. `BENIGN_WITHOUT_CALLBACK`, `BENIGN`, `MALIGNANT` 세 개의 unique한 str형 features로 구성되어 있으며, 각각 정상(추정), 양성, 음성을 의미합니다. (e.g. BENIGN, BENIGN, MALIGNANT, ...)
    - <b>`subtlety`:</b> 확인되지 않은 컬럼입니다. `1`~`5`의 범위를 갖는 int형 feature로 구성되어 있습니다. (e.g. 3, 4, 1, 5, 2, ...)
    - <b>`image file path`:</b> 원본 이미지 파일(`.dcm`)의 경로입니다.
    - <b>`cropped image file path`:</b> 의심 ROI segment에 대해 crop 후 저장한 `.dcm` 이미지 경로입니다. segment 단위로 bounding box를 근사해 crop한 것으로 추정됩니다.
    - <b>`ROI mask file path`:</b> 원본 이미지 파일에 대응해 의심 ROI 영역을 segment로 annotation한 마스크 `.dcm` 이미지입니다.
- <b>`mass` 타입의 데이터셋 각 컬럼별 설명 (`mass_case_description_train_set_df`, `mass_case_description_test_set_df`)</b>
    - <b>`patient_id`:</b> 상기와 동일
    - <b>`breast density`:</b> 유방 조직의 밀도를 구분한 정수형 feature입니다. train csv와 test csv 모두 `1`~`4`의 int형 feature를 포함하고 있습니다. (e.g. 3, 4, 1, 2, ...)
    - <b>`left or right breast`:</b> 상기와 동일
    - <b>`image view`:</b> 상기와 동일
    - <b>`abnormality id`:</b> 비정상 정도에 대한 레벨을 구분하는 정수형 feature입니다. [BI-RADS](https://en.wikipedia.org/wiki/BI-RADS)에 따른 각 사례별 분류로 추정되며, train csv는 `1`부터 `6`까지의 범위를 갖는 int형 feature를, test csv는 `1`~`4`의 범위를 갖는 int형 feature를 포함하고 있습니다. (e.g. 3, 2, 5, 1, ...)
    - <b>`abnormality type`:</b> 상기와 동일
    - <b>`mass shape`:</b> mass 타입의 lumps 조직이 어떤 모양을 갖고 있는지에 대한 정보입니다. 약 19개의 str형 features로 구분되어 있습니다. `nan`값도 포함되어 있으므로 핸들링에 주의가 필요합니다. (e.g. IRREGULAR-ARCHITECTURAL_DISTORTION, LOBULATED, ...)
    - <b>`mass margins`:</b> 확인되지 않은 컬럼입니다. `SPICULATED`, `ILL_DEFINED`, `CIRCUMSCRIBED`, `ILL_DEFINED-SPICULATED`, `OBSCURED`, `OBSCURED-ILL_DEFINED`, `MICROLOBULATED`, `MICROLOBULATED-ILL_DEFINED-SPICULATED`, `MICROLOBULATED-SPICULATED`, `CIRCUMSCRIBED-ILL_DEFINED`, `MICROLOBULATED-ILL_DEFINED`, `CIRCUMSCRIBED-OBSCURED`, `OBSCURED-SPICULATED`, `OBSCURED-ILL_DEFINED-SPICULATED`, `CIRCUMSCRIBED-MICROLOBULATED`의 unique한 str features를 갖고 있으며 `nan`값도 포함되어 있으므로 핸들링에 주의가 필요합니다. (e.g. CIRCUMSCRIBED, SPICULATED, ...)
    - <b>`assessment`:</b> 확인되지 않은 컬럼입니다. `0`~`5`의 unique한 int형 features가 포함되어 있습니다.
    - <b>`pathology`:</b> 상기와 동일
    - <b>`subtlety`:</b> 확인되지 않은 컬럼입니다. train csv는 `1`부터 `5`까지의 범위를 갖는 int형 feature로 구성되어 있으며, test csv는 `1`~`5`의 범위를 갖는 int형 features로 구성되어 있습니다. (e.g. 3, 4, 1, 5, 2, ...)
    - <b>`image file path`</b>, <b>`cropped image file path`</b>, <b>`ROI mask file path`</b>: 상기와 동일


```python
calc_case_description_train_set_df.head(3)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>breast density</th>
      <th>left or right breast</th>
      <th>image view</th>
      <th>abnormality id</th>
      <th>abnormality type</th>
      <th>calc type</th>
      <th>calc distribution</th>
      <th>assessment</th>
      <th>pathology</th>
      <th>subtlety</th>
      <th>image file path</th>
      <th>cropped image file path</th>
      <th>ROI mask file path</th>
    </tr>
    <tr>
      <th>patient_id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>P_00005</th>
      <td>3</td>
      <td>RIGHT</td>
      <td>CC</td>
      <td>1</td>
      <td>calcification</td>
      <td>AMORPHOUS</td>
      <td>CLUSTERED</td>
      <td>3</td>
      <td>MALIGNANT</td>
      <td>3</td>
      <td>Calc-Training_P_00005_RIGHT_CC/1.3.6.1.4.1.959...</td>
      <td>Calc-Training_P_00005_RIGHT_CC_1/1.3.6.1.4.1.9...</td>
      <td>Calc-Training_P_00005_RIGHT_CC_1/1.3.6.1.4.1.9...</td>
    </tr>
    <tr>
      <th>P_00005</th>
      <td>3</td>
      <td>RIGHT</td>
      <td>MLO</td>
      <td>1</td>
      <td>calcification</td>
      <td>AMORPHOUS</td>
      <td>CLUSTERED</td>
      <td>3</td>
      <td>MALIGNANT</td>
      <td>3</td>
      <td>Calc-Training_P_00005_RIGHT_MLO/1.3.6.1.4.1.95...</td>
      <td>Calc-Training_P_00005_RIGHT_MLO_1/1.3.6.1.4.1....</td>
      <td>Calc-Training_P_00005_RIGHT_MLO_1/1.3.6.1.4.1....</td>
    </tr>
    <tr>
      <th>P_00007</th>
      <td>4</td>
      <td>LEFT</td>
      <td>CC</td>
      <td>1</td>
      <td>calcification</td>
      <td>PLEOMORPHIC</td>
      <td>LINEAR</td>
      <td>4</td>
      <td>BENIGN</td>
      <td>4</td>
      <td>Calc-Training_P_00007_LEFT_CC/1.3.6.1.4.1.9590...</td>
      <td>Calc-Training_P_00007_LEFT_CC_1/1.3.6.1.4.1.95...</td>
      <td>Calc-Training_P_00007_LEFT_CC_1/1.3.6.1.4.1.95...</td>
    </tr>
  </tbody>
</table>
</div>


```python
calc_case_description_test_set_df.head(3)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>breast density</th>
      <th>left or right breast</th>
      <th>image view</th>
      <th>abnormality id</th>
      <th>abnormality type</th>
      <th>calc type</th>
      <th>calc distribution</th>
      <th>assessment</th>
      <th>pathology</th>
      <th>subtlety</th>
      <th>image file path</th>
      <th>cropped image file path</th>
      <th>ROI mask file path</th>
    </tr>
    <tr>
      <th>patient_id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>P_00038</th>
      <td>2</td>
      <td>LEFT</td>
      <td>CC</td>
      <td>1</td>
      <td>calcification</td>
      <td>PUNCTATE-PLEOMORPHIC</td>
      <td>CLUSTERED</td>
      <td>4</td>
      <td>BENIGN</td>
      <td>2</td>
      <td>Calc-Test_P_00038_LEFT_CC/1.3.6.1.4.1.9590.100...</td>
      <td>Calc-Test_P_00038_LEFT_CC_1/1.3.6.1.4.1.9590.1...</td>
      <td>Calc-Test_P_00038_LEFT_CC_1/1.3.6.1.4.1.9590.1...</td>
    </tr>
    <tr>
      <th>P_00038</th>
      <td>2</td>
      <td>LEFT</td>
      <td>MLO</td>
      <td>1</td>
      <td>calcification</td>
      <td>PUNCTATE-PLEOMORPHIC</td>
      <td>CLUSTERED</td>
      <td>4</td>
      <td>BENIGN</td>
      <td>2</td>
      <td>Calc-Test_P_00038_LEFT_MLO/1.3.6.1.4.1.9590.10...</td>
      <td>Calc-Test_P_00038_LEFT_MLO_1/1.3.6.1.4.1.9590....</td>
      <td>Calc-Test_P_00038_LEFT_MLO_1/1.3.6.1.4.1.9590....</td>
    </tr>
    <tr>
      <th>P_00038</th>
      <td>2</td>
      <td>RIGHT</td>
      <td>CC</td>
      <td>1</td>
      <td>calcification</td>
      <td>VASCULAR</td>
      <td>NaN</td>
      <td>2</td>
      <td>BENIGN_WITHOUT_CALLBACK</td>
      <td>5</td>
      <td>Calc-Test_P_00038_RIGHT_CC/1.3.6.1.4.1.9590.10...</td>
      <td>Calc-Test_P_00038_RIGHT_CC_1/1.3.6.1.4.1.9590....</td>
      <td>Calc-Test_P_00038_RIGHT_CC_1/1.3.6.1.4.1.9590....</td>
    </tr>
  </tbody>
</table>
</div>


```python
mass_case_description_train_set_df.head(3)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>breast_density</th>
      <th>left or right breast</th>
      <th>image view</th>
      <th>abnormality id</th>
      <th>abnormality type</th>
      <th>mass shape</th>
      <th>mass margins</th>
      <th>assessment</th>
      <th>pathology</th>
      <th>subtlety</th>
      <th>image file path</th>
      <th>cropped image file path</th>
      <th>ROI mask file path</th>
    </tr>
    <tr>
      <th>patient_id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>P_00001</th>
      <td>3</td>
      <td>LEFT</td>
      <td>CC</td>
      <td>1</td>
      <td>mass</td>
      <td>IRREGULAR-ARCHITECTURAL_DISTORTION</td>
      <td>SPICULATED</td>
      <td>4</td>
      <td>MALIGNANT</td>
      <td>4</td>
      <td>Mass-Training_P_00001_LEFT_CC/1.3.6.1.4.1.9590...</td>
      <td>Mass-Training_P_00001_LEFT_CC_1/1.3.6.1.4.1.95...</td>
      <td>Mass-Training_P_00001_LEFT_CC_1/1.3.6.1.4.1.95...</td>
    </tr>
    <tr>
      <th>P_00001</th>
      <td>3</td>
      <td>LEFT</td>
      <td>MLO</td>
      <td>1</td>
      <td>mass</td>
      <td>IRREGULAR-ARCHITECTURAL_DISTORTION</td>
      <td>SPICULATED</td>
      <td>4</td>
      <td>MALIGNANT</td>
      <td>4</td>
      <td>Mass-Training_P_00001_LEFT_MLO/1.3.6.1.4.1.959...</td>
      <td>Mass-Training_P_00001_LEFT_MLO_1/1.3.6.1.4.1.9...</td>
      <td>Mass-Training_P_00001_LEFT_MLO_1/1.3.6.1.4.1.9...</td>
    </tr>
    <tr>
      <th>P_00004</th>
      <td>3</td>
      <td>LEFT</td>
      <td>CC</td>
      <td>1</td>
      <td>mass</td>
      <td>ARCHITECTURAL_DISTORTION</td>
      <td>ILL_DEFINED</td>
      <td>4</td>
      <td>BENIGN</td>
      <td>3</td>
      <td>Mass-Training_P_00004_LEFT_CC/1.3.6.1.4.1.9590...</td>
      <td>Mass-Training_P_00004_LEFT_CC_1/1.3.6.1.4.1.95...</td>
      <td>Mass-Training_P_00004_LEFT_CC_1/1.3.6.1.4.1.95...</td>
    </tr>
  </tbody>
</table>
</div>


```python
mass_case_description_test_set_df.head(3)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>breast_density</th>
      <th>left or right breast</th>
      <th>image view</th>
      <th>abnormality id</th>
      <th>abnormality type</th>
      <th>mass shape</th>
      <th>mass margins</th>
      <th>assessment</th>
      <th>pathology</th>
      <th>subtlety</th>
      <th>image file path</th>
      <th>cropped image file path</th>
      <th>ROI mask file path</th>
    </tr>
    <tr>
      <th>patient_id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>P_00016</th>
      <td>4</td>
      <td>LEFT</td>
      <td>CC</td>
      <td>1</td>
      <td>mass</td>
      <td>IRREGULAR</td>
      <td>SPICULATED</td>
      <td>5</td>
      <td>MALIGNANT</td>
      <td>5</td>
      <td>Mass-Test_P_00016_LEFT_CC/1.3.6.1.4.1.9590.100...</td>
      <td>Mass-Test_P_00016_LEFT_CC_1/1.3.6.1.4.1.9590.1...</td>
      <td>Mass-Test_P_00016_LEFT_CC_1/1.3.6.1.4.1.9590.1...</td>
    </tr>
    <tr>
      <th>P_00016</th>
      <td>4</td>
      <td>LEFT</td>
      <td>MLO</td>
      <td>1</td>
      <td>mass</td>
      <td>IRREGULAR</td>
      <td>SPICULATED</td>
      <td>5</td>
      <td>MALIGNANT</td>
      <td>5</td>
      <td>Mass-Test_P_00016_LEFT_MLO/1.3.6.1.4.1.9590.10...</td>
      <td>Mass-Test_P_00016_LEFT_MLO_1/1.3.6.1.4.1.9590....</td>
      <td>Mass-Test_P_00016_LEFT_MLO_1/1.3.6.1.4.1.9590....</td>
    </tr>
    <tr>
      <th>P_00017</th>
      <td>2</td>
      <td>LEFT</td>
      <td>CC</td>
      <td>1</td>
      <td>mass</td>
      <td>ROUND</td>
      <td>CIRCUMSCRIBED</td>
      <td>4</td>
      <td>MALIGNANT</td>
      <td>4</td>
      <td>Mass-Test_P_00017_LEFT_CC/1.3.6.1.4.1.9590.100...</td>
      <td>Mass-Test_P_00017_LEFT_CC_1/1.3.6.1.4.1.9590.1...</td>
      <td>Mass-Test_P_00017_LEFT_CC_1/1.3.6.1.4.1.9590.1...</td>
    </tr>
  </tbody>
</table>
</div>


```python
print('Length of calc_case_description_train_set_df: ', len(calc_case_description_train_set_df))
print('Length of calc_case_description_test_set_df', len(calc_case_description_test_set_df))
print('Length of mass_case_description_train_set_df: ', len(mass_case_description_train_set_df))
print('Length of mass_case_description_test_set_df:', len(mass_case_description_test_set_df))
```

    Length of calc_case_description_train_set_df:  1546
    Length of calc_case_description_test_set_df 326
    Length of mass_case_description_train_set_df:  1318
    Length of mass_case_description_test_set_df: 378
    

### 1.2 `Abnormality type`별 이미지 관찰
이제 `calc`와 `mass` 두 가지 타입 별로 이미지 몇 장을 뽑아 시각화해 보겠습니다. 각 column의 인덱스는 다음과 같이 `patient_id`로 접근할 수 있습니다. 우선 3개 정도의 이미지를 랜덤 샘플링해 출력해 보겠습니다. 

```python
sample_calc_case_description_train_set_df = calc_case_description_train_set_df.sample(n=3)
sample_mass_case_description_train_set_df = mass_case_description_train_set_df.sample(n=3)
```


```python
sample_calc_case_description_train_set_df
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>breast density</th>
      <th>left or right breast</th>
      <th>image view</th>
      <th>abnormality id</th>
      <th>abnormality type</th>
      <th>calc type</th>
      <th>calc distribution</th>
      <th>assessment</th>
      <th>pathology</th>
      <th>subtlety</th>
      <th>image file path</th>
      <th>cropped image file path</th>
      <th>ROI mask file path</th>
    </tr>
    <tr>
      <th>patient_id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>P_01830</th>
      <td>3</td>
      <td>RIGHT</td>
      <td>MLO</td>
      <td>3</td>
      <td>calcification</td>
      <td>PUNCTATE</td>
      <td>REGIONAL</td>
      <td>2</td>
      <td>BENIGN_WITHOUT_CALLBACK</td>
      <td>5</td>
      <td>Calc-Training_P_01830_RIGHT_MLO/1.3.6.1.4.1.95...</td>
      <td>Calc-Training_P_01830_RIGHT_MLO_3/1.3.6.1.4.1....</td>
      <td>Calc-Training_P_01830_RIGHT_MLO_3/1.3.6.1.4.1....</td>
    </tr>
    <tr>
      <th>P_01002</th>
      <td>3</td>
      <td>LEFT</td>
      <td>CC</td>
      <td>1</td>
      <td>calcification</td>
      <td>FINE_LINEAR_BRANCHING</td>
      <td>REGIONAL</td>
      <td>5</td>
      <td>MALIGNANT</td>
      <td>5</td>
      <td>Calc-Training_P_01002_LEFT_CC/1.3.6.1.4.1.9590...</td>
      <td>Calc-Training_P_01002_LEFT_CC_1/1.3.6.1.4.1.95...</td>
      <td>Calc-Training_P_01002_LEFT_CC_1/1.3.6.1.4.1.95...</td>
    </tr>
    <tr>
      <th>P_01136</th>
      <td>2</td>
      <td>RIGHT</td>
      <td>MLO</td>
      <td>1</td>
      <td>calcification</td>
      <td>PLEOMORPHIC</td>
      <td>CLUSTERED</td>
      <td>5</td>
      <td>MALIGNANT</td>
      <td>5</td>
      <td>Calc-Training_P_01136_RIGHT_MLO/1.3.6.1.4.1.95...</td>
      <td>Calc-Training_P_01136_RIGHT_MLO_1/1.3.6.1.4.1....</td>
      <td>Calc-Training_P_01136_RIGHT_MLO_1/1.3.6.1.4.1....</td>
    </tr>
  </tbody>
</table>
</div>


```python
sample_mass_case_description_train_set_df
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>breast_density</th>
      <th>left or right breast</th>
      <th>image view</th>
      <th>abnormality id</th>
      <th>abnormality type</th>
      <th>mass shape</th>
      <th>mass margins</th>
      <th>assessment</th>
      <th>pathology</th>
      <th>subtlety</th>
      <th>image file path</th>
      <th>cropped image file path</th>
      <th>ROI mask file path</th>
    </tr>
    <tr>
      <th>patient_id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>P_00528</th>
      <td>3</td>
      <td>LEFT</td>
      <td>MLO</td>
      <td>2</td>
      <td>mass</td>
      <td>OVAL</td>
      <td>CIRCUMSCRIBED</td>
      <td>3</td>
      <td>BENIGN</td>
      <td>5</td>
      <td>Mass-Training_P_00528_LEFT_MLO/1.3.6.1.4.1.959...</td>
      <td>Mass-Training_P_00528_LEFT_MLO_2/1.3.6.1.4.1.9...</td>
      <td>Mass-Training_P_00528_LEFT_MLO_2/1.3.6.1.4.1.9...</td>
    </tr>
    <tr>
      <th>P_01163</th>
      <td>2</td>
      <td>LEFT</td>
      <td>CC</td>
      <td>1</td>
      <td>mass</td>
      <td>OVAL</td>
      <td>SPICULATED</td>
      <td>4</td>
      <td>MALIGNANT</td>
      <td>5</td>
      <td>Mass-Training_P_01163_LEFT_CC/1.3.6.1.4.1.9590...</td>
      <td>Mass-Training_P_01163_LEFT_CC_1/1.3.6.1.4.1.95...</td>
      <td>Mass-Training_P_01163_LEFT_CC_1/1.3.6.1.4.1.95...</td>
    </tr>
    <tr>
      <th>P_00847</th>
      <td>4</td>
      <td>LEFT</td>
      <td>MLO</td>
      <td>1</td>
      <td>mass</td>
      <td>ARCHITECTURAL_DISTORTION</td>
      <td>NaN</td>
      <td>2</td>
      <td>BENIGN</td>
      <td>4</td>
      <td>Mass-Training_P_00847_LEFT_MLO/1.3.6.1.4.1.959...</td>
      <td>Mass-Training_P_00847_LEFT_MLO_1/1.3.6.1.4.1.9...</td>
      <td>Mass-Training_P_00847_LEFT_MLO_1/1.3.6.1.4.1.9...</td>
    </tr>
  </tbody>
</table>
</div>

### 1.2.1. `calc` 타입 이미지 확인
샘플 오리지널 이미지와 cropped ROI, ROI mask의 이미지 비율은 각각 다르지만 시각화의 편의를 위해 1000 x 1000 해상도로 출력했습니다.

```python
import PIL
import numpy as np
import pydicom as dicom
from PIL import ImageDraw
import matplotlib.pyplot as plt
import cv2

font = cv2.FONT_HERSHEY_SIMPLEX
fontScale = 0.5

# Dataset directory
DDSM_dataPATH = 'E:/CBIM-DDSM'

# sample image file lists
calc_case_original_img = sample_calc_case_description_train_set_df['image file path'].to_list()
calc_case_croppedROI_img = sample_calc_case_description_train_set_df['cropped image file path'].to_list()
calc_case_ROImask_img = sample_calc_case_description_train_set_df['ROI mask file path'].to_list()

for i in range(3):
    plt.figure(figsize=(12,20))
    original_image_ds = dicom.dcmread(os.path.join(DDSM_dataPATH, calc_case_original_img[i]).replace('\n', '').replace('\r', ''))
    original_image_ds_resized = cv2.resize(original_image_ds.pixel_array, dsize=(750, 1000), interpolation=cv2.INTER_CUBIC)

    croppedROI_image_ds = dicom.dcmread(os.path.join(DDSM_dataPATH, calc_case_croppedROI_img[i]).replace('\n', '').replace('\r', ''))
    croppedROI_image_ds_resized = cv2.resize(croppedROI_image_ds.pixel_array, dsize=(1000, 1000), interpolation=cv2.INTER_CUBIC)

    ROIMask_image_ds = dicom.dcmread(os.path.join(DDSM_dataPATH, calc_case_ROImask_img[i]).replace('\n', '').replace('\r', ''))
    ROIMask_image_ds_resized = cv2.resize(ROIMask_image_ds.pixel_array, dsize=(1000, 1000), interpolation=cv2.INTER_CUBIC)
    merged_image = np.concatenate((np.concatenate((original_image_ds_resized, croppedROI_image_ds_resized), axis=1), ROIMask_image_ds_resized), axis=1)
        
    plt.title('Preview of calc type images(original, mask, croppedROI order)', fontsize=15)
    plt.imshow(merged_image, cmap = 'gray')
    plt.show()
```

![case_details](/assets/img/devlog/shoveling/CBISDDSM_EDA/output_14_0.png)

![case_details](/assets/img/devlog/shoveling/CBISDDSM_EDA/output_14_1.png)

![case_details](/assets/img/devlog/shoveling/CBISDDSM_EDA/output_14_2.png)


### 1.2.2. `mass` 타입 이미지 확인


```python
font = cv2.FONT_HERSHEY_SIMPLEX
fontScale = 0.5

# sample image file lists
mass_case_original_img = sample_mass_case_description_train_set_df['image file path'].to_list()
mass_case_croppedROI_img = sample_mass_case_description_train_set_df['cropped image file path'].to_list()
mass_case_ROImask_img = sample_mass_case_description_train_set_df['ROI mask file path'].to_list()

for i in range(3):
    plt.figure(figsize=(12,20))
    original_image_ds = dicom.dcmread(os.path.join(DDSM_dataPATH, mass_case_original_img[i].replace('\n', '').replace('\r', '')))
    original_image_ds_resized = cv2.resize(original_image_ds.pixel_array, dsize=(750, 1000), interpolation=cv2.INTER_CUBIC)

    croppedROI_image_ds = dicom.dcmread(os.path.join(DDSM_dataPATH, mass_case_croppedROI_img[i].replace('\n', '').replace('\r', '')))
    croppedROI_image_ds_resized = cv2.resize(croppedROI_image_ds.pixel_array, dsize=(1000, 1000), interpolation=cv2.INTER_CUBIC)

    ROIMask_image_ds = dicom.dcmread(os.path.join(DDSM_dataPATH, mass_case_ROImask_img[i].replace('\n', '').replace('\r', '')))
    ROIMask_image_ds_resized = cv2.resize(ROIMask_image_ds.pixel_array, dsize=(1000, 1000), interpolation=cv2.INTER_CUBIC)
    merged_image = np.concatenate((np.concatenate((original_image_ds_resized, croppedROI_image_ds_resized), axis=1), ROIMask_image_ds_resized), axis=1)
        
    plt.title('Preview of mass type images', fontsize=15)
    plt.imshow(merged_image, cmap = 'gray')
    plt.show()
```


![case_details](/assets/img/devlog/shoveling/CBISDDSM_EDA/output_16_0.png)

![case_details](/assets/img/devlog/shoveling/CBISDDSM_EDA/output_16_1.png)

![case_details](/assets/img/devlog/shoveling/CBISDDSM_EDA/output_16_2.png)

위 이미지들을 통해 확인할 수 있듯, cropped ROI에는 히스토그램 평활화로 추정되는 이미지 전처리 기법이 적용되어 있습니다. X-Ray 이미지를 전처리하는 데엔 다양한 방법들이 존재하며, 아래 노트북을 참고해 전처리 및 Augmentaion 작업을 진행하면 큰 도움이 될 것으로 예상됩니다.<br>
https://github.com/yuyuyu123456/CBIS-DDSM/blob/master/EDA.ipynb <br>

## 2. CSV data EDA
이제 각 csv 파일이 갖고 있는 column들에 대해 보다 자세히 EDA를 수행한 후 분포를 확인해 보도록 하겠습니다. 각 csv파일들이 갖고 있는 정수형 변수들에 대한 간단한 통계 정보는 아래와 같습니다.

```python
calc_case_description_train_set_df.describe()
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>breast density</th>
      <th>abnormality id</th>
      <th>assessment</th>
      <th>subtlety</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1546.000000</td>
      <td>1546.000000</td>
      <td>1546.000000</td>
      <td>1546.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>2.663648</td>
      <td>1.415265</td>
      <td>3.258732</td>
      <td>3.411384</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.937219</td>
      <td>0.903571</td>
      <td>1.229231</td>
      <td>1.179754</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2.000000</td>
      <td>1.000000</td>
      <td>2.000000</td>
      <td>3.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>3.000000</td>
      <td>1.000000</td>
      <td>4.000000</td>
      <td>3.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>3.000000</td>
      <td>1.000000</td>
      <td>4.000000</td>
      <td>4.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>4.000000</td>
      <td>7.000000</td>
      <td>5.000000</td>
      <td>5.000000</td>
    </tr>
  </tbody>
</table>
</div>


```python
calc_case_description_test_set_df.describe()
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>breast density</th>
      <th>abnormality id</th>
      <th>assessment</th>
      <th>subtlety</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>326.000000</td>
      <td>326.000000</td>
      <td>326.000000</td>
      <td>326.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>2.696319</td>
      <td>1.214724</td>
      <td>3.453988</td>
      <td>3.319018</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.909667</td>
      <td>0.529061</td>
      <td>1.188159</td>
      <td>1.188175</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2.000000</td>
      <td>1.000000</td>
      <td>2.000000</td>
      <td>3.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>3.000000</td>
      <td>1.000000</td>
      <td>4.000000</td>
      <td>3.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>3.000000</td>
      <td>1.000000</td>
      <td>4.000000</td>
      <td>4.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>4.000000</td>
      <td>5.000000</td>
      <td>5.000000</td>
      <td>5.000000</td>
    </tr>
  </tbody>
</table>
</div>


```python
mass_case_description_train_set_df.describe()
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>breast_density</th>
      <th>abnormality id</th>
      <th>assessment</th>
      <th>subtlety</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1318.000000</td>
      <td>1318.000000</td>
      <td>1318.000000</td>
      <td>1318.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>2.203338</td>
      <td>1.116085</td>
      <td>3.504552</td>
      <td>3.965857</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.873774</td>
      <td>0.467013</td>
      <td>1.414609</td>
      <td>1.102032</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2.000000</td>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>3.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2.000000</td>
      <td>1.000000</td>
      <td>4.000000</td>
      <td>4.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>3.000000</td>
      <td>1.000000</td>
      <td>4.000000</td>
      <td>5.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>4.000000</td>
      <td>6.000000</td>
      <td>5.000000</td>
      <td>5.000000</td>
    </tr>
  </tbody>
</table>
</div>


```python
mass_case_description_test_set_df.describe()
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>breast_density</th>
      <th>abnormality id</th>
      <th>assessment</th>
      <th>subtlety</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>378.000000</td>
      <td>378.000000</td>
      <td>378.000000</td>
      <td>378.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>2.396825</td>
      <td>1.092593</td>
      <td>3.534392</td>
      <td>3.785714</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.859455</td>
      <td>0.398136</td>
      <td>1.343076</td>
      <td>1.171776</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2.000000</td>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>3.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2.000000</td>
      <td>1.000000</td>
      <td>4.000000</td>
      <td>4.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>3.000000</td>
      <td>1.000000</td>
      <td>4.000000</td>
      <td>5.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>4.000000</td>
      <td>4.000000</td>
      <td>5.000000</td>
      <td>5.000000</td>
    </tr>
  </tbody>
</table>
</div>


### 2.1 `assessment` and `abnormality id` distribution
우선, `assessment`와 `abnormality id`의 분포를 출력해 보겠습니다. `assessment`는 목적을 확인할 수 없는 컬럼이고, `abnormality id`는 BI-RADS 기준에 따라 분류된 비정상 정도를 의미합니다.

```python
import matplotlib
import matplotlib.pylab as pylab
import matplotlib.pyplot as plt

SMALL_SIZE = 17
MEDIUM_SIZE = 20
BIGGER_SIZE = 25
font = {'family' : 'DejaVu Sans',
        'weight' : 'normal',}
matplotlib.rc('font', **font)
params = {'legend.fontsize': SMALL_SIZE,
         'axes.labelsize': MEDIUM_SIZE,
         'axes.titlesize':BIGGER_SIZE,
         'xtick.labelsize':MEDIUM_SIZE,
         'ytick.labelsize':MEDIUM_SIZE}
pylab.rcParams.update(params)

fig, axes = plt.subplots(nrows=2, ncols=2)

calc_case_description_train_set_df['assessment'].value_counts().sort_index().plot(ax=axes[0, 0], kind='bar', rot=75, figsize=(20, 10)).set_title('assessment frequency of `calc_case_description_train`')
calc_case_description_test_set_df['assessment'].value_counts().sort_index().plot(ax=axes[0, 1], kind='bar', rot=75, figsize=(20, 10)).set_title('assessment frequency of `calc_case_description_test`')
mass_case_description_train_set_df['assessment'].value_counts().sort_index().plot(ax=axes[1, 0], kind='bar', rot=75, figsize=(30, 10)).set_title('assessment frequency of `mass_case_description_train`')
mass_case_description_test_set_df['assessment'].value_counts().sort_index().plot(ax=axes[1, 1], kind='bar', rot=75, figsize=(30, 10)).set_title('assessment frequency of `mass_case_description_test`')

plt.tight_layout()
plt.show()
```

![case_details](/assets/img/devlog/shoveling/CBISDDSM_EDA/output_24_0.png)


```python
import matplotlib
import matplotlib.pylab as pylab
import matplotlib.pyplot as plt

SMALL_SIZE = 17
MEDIUM_SIZE = 20
BIGGER_SIZE = 25
font = {'family' : 'DejaVu Sans',
        'weight' : 'normal',}
matplotlib.rc('font', **font)
params = {'legend.fontsize': SMALL_SIZE,
         'axes.labelsize': MEDIUM_SIZE,
         'axes.titlesize':BIGGER_SIZE,
         'xtick.labelsize':MEDIUM_SIZE,
         'ytick.labelsize':MEDIUM_SIZE}
pylab.rcParams.update(params)

fig, axes = plt.subplots(nrows=2, ncols=2)

calc_case_description_train_set_df['abnormality id'].value_counts().sort_index().plot(ax=axes[0, 0], kind='bar', rot=75, figsize=(20, 10)).set_title('abnormality id frequency of `calc_case_description_train`')
calc_case_description_test_set_df['abnormality id'].value_counts().sort_index().plot(ax=axes[0, 1], kind='bar', rot=75, figsize=(20, 10)).set_title('abnormality id frequency of `calc_case_description_test`')
mass_case_description_train_set_df['abnormality id'].value_counts().sort_index().plot(ax=axes[1, 0], kind='bar', rot=75, figsize=(30, 10)).set_title('abnormality id frequency of `mass_case_description_train`')
mass_case_description_test_set_df['abnormality id'].value_counts().sort_index().plot(ax=axes[1, 1], kind='bar', rot=75, figsize=(30, 10)).set_title('abnormality id frequency of `mass_case_description_test`')

plt.tight_layout()
plt.show()
```

![case_details](/assets/img/devlog/shoveling/CBISDDSM_EDA/output_25_0.png)


### 2.2 Pathology(`normal`, `benign`, `malignant`) distribution
그 다음으로, 학습에 가장 중요한 영향을 끼칠 `Pathology` column에 대해서도 분포를 출력해 보겠습니다.

```python
import matplotlib
import matplotlib.pylab as pylab
import matplotlib.pyplot as plt

SMALL_SIZE = 17
MEDIUM_SIZE = 20
BIGGER_SIZE = 25
font = {'family' : 'DejaVu Sans',
        'weight' : 'normal',}
matplotlib.rc('font', **font)
params = {'legend.fontsize': SMALL_SIZE,
         'axes.labelsize': MEDIUM_SIZE,
         'axes.titlesize':BIGGER_SIZE,
         'xtick.labelsize':MEDIUM_SIZE,
         'ytick.labelsize':MEDIUM_SIZE}
pylab.rcParams.update(params)

fig, axes = plt.subplots(nrows=2, ncols=2)

calc_case_description_train_set_df['pathology'].value_counts().sort_index().plot(ax=axes[0, 0], kind='bar', rot=0, figsize=(20, 10)).set_title('pathology distribution of `calc_case_description_train`')
calc_case_description_test_set_df['pathology'].value_counts().sort_index().plot(ax=axes[0, 1], kind='bar', rot=0, figsize=(20, 10)).set_title('pathology distribution of `calc_case_description_test`')
mass_case_description_train_set_df['pathology'].value_counts().sort_index().plot(ax=axes[1, 0], kind='bar', rot=0, figsize=(30, 10)).set_title('pathology distribution of `mass_case_description_train`')
mass_case_description_test_set_df['pathology'].value_counts().sort_index().plot(ax=axes[1, 1], kind='bar', rot=0, figsize=(30, 10)).set_title('pathology distribution of `mass_case_description_test`')

plt.tight_layout()
plt.show()
```

![case_details](/assets/img/devlog/shoveling/CBISDDSM_EDA/output_27_0.png)

`pathology` column은 train과 test csv에서 각기 다른 양상을 보이는 것을 확인할 수 있습니다. 
이어지는 내용은 下편에서 다루도록 하겠습니다.  