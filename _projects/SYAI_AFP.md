---
layout: project
title: Synergy A.I. 12-lead PAF Risk Prediction Research PoC (진행중)
caption: 12-lead Paroxysmal Atrial Fibrillation Risk Modeling 연구는 정상 심전도 신호를 분석해 향후 심방세동 발생 가능성을 추정하는 연구입니다. 
description: >
  고령화 시대에 접어들며 전 세계적으로 심방세동 유병률이 증가하고 있습니다. 뇌졸중, 원인 미상 급사 등 중증 질환과 큰 연관이 있는 심방세동의 징후를 사전에 탐지해 관련 기관과 환자에게 알릴 수 있는 체계를 개발하고자 합니다.  
featured: true
date: 1 March 2022
image: 
  path: /assets/img/projects/SYAI_head.png
  srcset: 
    1920w: /assets/img/projects/SYAI_head.png
    960w:  /assets/img/projects/SYAI_head@0,5.png
    480w:  /assets/img/projects/SYAI_head@0,25.png
links:
  - title: Link (Service not released yet)
    url: https://www.synergyai.co/
accent_color: '#4fb1ba'
accent_image: 
  background: url('/assets/img/projects/ignites_head.png') center/cover
theme_color: '#193747'
sitemap: false
---

## Synergy A.I. 12-lead PAF Risk Prediction Research PoC
### Research PoC 소개 
심장부정맥은 심장 리듬이 흐트러지는 병으로, 뇌졸중과 급사 등 심각한 질환과 연관되어 있습니다. 부정맥 중 특히 심방세동은 가장 흔하며, 고령화로 인해 전 세계적으로 유병률이 증가하고 있습니다. 심방세동은 특히 뇌졸중의 위험도를 약 4배 정도 올리지만 아직까지 국내 심방세동 환자의 항응고요법 사용률은 30% 미만으로 낮은 실정입니다.[1] 뇌졸중 예방의 중요성을 강조하고 심방세동의 치료를 위해 의료 기관들의 협력이 요구되는 새로운 의료 환경 변화의 필요성이 대두됨에 따라 심방세동의 징후를 사전에 탐지해 관련 기관과 환자에게 알릴 수 있는 체계를 개발하고자 합니다. 

무증상으로 시작하는 심방 세동을 조기 진단하면 적절한 항응고 요법을 시작할 수 있고, 뇌졸중 위협을 낮출 수 있습니다. 전체 허혈성 뇌졸중의 약 10%는 처음 진단된 심방세동 환자에서 관찰되고 있어, 심방세동의 스크리닝이 강조되고 있습니다.[1] 최근에는 혈압기, 스마트시계 등 다양한 장치에서 맥박 측정이 가능하므로, 이를 이용해 효율적인 심방세동 경보 시스템을 구축할 수 있습니다. 

<blockquote>
[1] “심방세동 치료 가이드라인” - 정보영(연세대학교 의과대학 세브란스병원 심장내과), J Korean Med Assoc 2019 May; 62(5):265-274
</blockquote><br>


- <b>유사 연구와 샘플 수 비교</b><br>
  <table>
  <thead>
    <tr>
      <th></th>
      <th>Healthy NSR</th>
      <th>AF NSR</th>
      <th>Total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Nature</td>
      <td>1,057</td>
      <td>1,355</td>
      <td>2,412</td>
    </tr>
    <tr>
      <td>Lancet (8-lead)</td>
      <td>Not announced</td>
      <td>Not announced</td>
      <td>180,922 patients,<br><br>649,931 ecgs</td>
    </tr>
    <tr>
      <td>Ours</td>
      <td>3,331 ecgs</td>
      <td>3,331 ecgs</td>
      <td>6,662 ecgs</td>
    </tr>
  </tbody>
  </table>
- <b>Prevalence* & Performance</b>
  - Lancet
    - Train:Valid:Test 7:1:2, Test(Class0:Positive) 10.89:1
    - Positive F1 0.87, Mean AUC 0.87
  - Nature
    - Train:Valid:Test 0.77:1:N/A, External Test(Class0:Positive) 1.29:1
  - Ours
    - Train:Valid:Test 6:2:2, Test(Class0:Positive) 1:1
    - Positive F1 0.79, Mean AUC 0.81

<i>* Estimated Prevalence: 1:9(Class 0, Positive)</i>


### 참여부문 소개 
- <b>참여 기간:</b> 2022.03.01 ~ 현재
- <b>참여 연구원 수:</b> 3명
- <b>참여 내용</b>
  - `INFINITT`, `PHILIPS`, `GE` Device로 측정한 임상 데이터 326,905례로부터 `*.xml` 파싱 및 실험 가정에 맞도록 환자 병렬처리 스크리닝(`Ray`)
  - Prevalence별 다양한 모델 훈련 및 성능 검증(1D `ResNet-34`, 1D `Transformer`)
  - Gradient map 시각화를 통한 PoC 타당성 인사이트 발굴 
  ![gradient map](/assets/img/projects/NSR_saliency.png)<br>
  ![](/assets/img/projects/signal_gradientmap.png)