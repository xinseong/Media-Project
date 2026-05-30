<div align="center">
  
  <h1>몇ML (How Many ML)</h1>
  <h3>AR 기반 액체 부피 측정 애플리케이션</h3>

  <p>
    <img src="https://img.shields.io/badge/Ajou_Univ-Media_Project-004A92?style=flat-square"/>
    <img src="https://img.shields.io/badge/Exhibition-SOFTCON_2026-FF69B4?style=flat-square"/>
    <img src="https://img.shields.io/badge/Platform-iOS-000000?style=flat-square&logo=apple&logoColor=white"/>
  </p>
</div>

---

## 1. 프로젝트 소개

> 스마트폰 카메라와 AI 모델을 활용하여, 별도의 도구 없이 용기에 담긴 액체의 부피를 자동으로 측정하는 **AR 기반 계량 서비스**입니다.

| 구분 | 설명 |
| :--- | :--- |
| **개발 배경** | 1인 가구 증가 및 홈쿡 문화 확산으로 정확한 레시피 계량 수요 증가 |
| **기대 효과** | 별도의 계량 도구(계량컵, 스푼 등) 없이 누구나 간편하고 정확하게 계량 가능한 환경 제공 |

---

## 2. 팀원 및 역할

**팀명**: 면밀히 몇ML <br>
**지도 멘토**: 정태홍 멘토님

| 이름 | 담당 역할 (Role) | 주요 업무 |
| :---: | :--- | :--- |
| **최재현** | **Backend / Algorithm** | - FastAPI 서버 구축<br>- 기하학 기반 부피 계산 알고리즘 개발 | 
| **설현웅** | **AI / ML** | - Depth Anything V3 모델 커스텀<br>- 가상 환경(ARIA) 데이터 증강 및 학습 | 
| **윤민성** | **Client / AR** | - iOS 클라이언트 앱 개발<br>- ARKit 기반 인터페이스(UI/UX) 구현 |

---

## 3. System Pipeline

### 3.1. 시스템 아키텍처 및 알고리즘 Flow
<p align="center">
<img width="759" height="430" alt="Image" src="https://github.com/user-attachments/assets/72931a69-a381-431d-a5d6-020d126eb343" />
</p>

* **Client (iOS App)**: 사용자가 카메라로 용기를 촬영하면 프레임 데이터를 서버로 전송합니다.
* **Server (FastAPI)**: 전송된 이미지 데이터를 받아 AI 모델 및 기하학 알고리즘 연산을 수행합니다.
* **Core Algorithm Flow**:
  1. **Image Filter & Detection**: 이미지 입력 후 Plane Detection(평면 탐지) 및 Circle Detection 수행
  2. **Depth Anything V3 (Fine-tuning)**: 커스텀 학습된 모델을 통해 정밀한 Depth(깊이) 및 Mask 정보 추출
  3. **Coordinate Transform & Height Calculation**: 카메라 포즈와 영상 데이터를 결합하여 실제 수면 높이 계산
  4. **Volume Calculation**: 최종 용기 형태와 높이 데이터를 기반으로 액체의 Volum 산출

---

## 4. Model Customization

### 4.1. Depth Anything V3 모델 최적화
프로젝트 목적에 맞는 정밀한 깊이 추정을 위해 **Depth Anything V3** 적용하였습니다.

<p align="center">
<img width="701" height="366" alt="Image" src="https://github.com/user-attachments/assets/3329571c-3d3f-46d8-a014-aa8b99041c48" />
</p>

* **Loss Function**<br>
  기존 Loss 스케일에 Object Head 및 Dual-OPT Head를 연동하여 오차를 최소화하는 $L_{music}$ 손실 함수를 추가 정의했습니다.

$$\mathcal{L} = \mathcal{L}_{D}(\hat{D},D) + \mathcal{L}_{M}(\hat{R},M) + \mathcal{L}_{P}(\hat{D}\otimes d+t,P) + \mathcal{L}_{grad}(\hat{D},D) + L_{music}$$

<<<<<<< HEAD
  * $\mathcal{L}_{D}$ /  $\mathcal{L}_{grad}$ : 깊이 추정 및 오차 최소화
=======
  * $\mathcal{L}_{D}$/$\mathcal{L}_{grad}$: 깊이 추정 및 오차 최소화
>>>>>>> a17dd888965bbf48e52a6eeb2b2040c044865aba
  * $\mathcal{L}_{M}$: Object Mask 분할
  * $L_{music}$: 커스텀 아키텍처 최적화를 위한 핵심 손실 함수

### 4.2. 데이터 증강 및 파인 튜닝 (Data Augmentation)
* **ARIA & Blender Python API 활용**: 3D 가상 환경 데이터를 활용하여 다양한 카메라 위치, 텍스처, 크기, 랜덤 스케일 변화를 주는 Augmentation 파이프라인을 구축했습니다.
* **Fine-tuning 효과**: Base Model 대비 파인 튜닝 후 실제 데이터에 수렴하는 정밀한 Depth와 Mask 결과를 확보하였습니다.

<p align="center">
<img width="534" height="263" alt="Image" src="https://github.com/user-attachments/assets/ae2d4afc-f915-48b3-8872-b90ffc9d06da" />
</p>


---

## 5. 결과 및 분석

> **평균 부피 측정 정확도 89% 달성**

용기 용량을 대상으로 측정한 결과 실제 부피에 근접하는 높은 측정 정확도를 확인하였습니다.

<p align="center">
<img width="468" height="417" alt="Image" src="https://github.com/user-attachments/assets/14f8007d-5f69-4b14-b38e-e49c2f1a2013" />
</p>

---

## 6. 기술 스택 

**Client**<br>
<img src="https://img.shields.io/badge/iOS-000000?style=flat-square&logo=apple&logoColor=white"/> <img src="https://img.shields.io/badge/ARKit-000000?style=flat-square&logo=apple&logoColor=white"/>

**Backend**<br>
<img src="https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white"/> <img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white"/>

**AI & ML**<br>
<img src="https://img.shields.io/badge/PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white"/> <img src="https://img.shields.io/badge/Depth_Anything_V3-4B0082?style=flat-square&logo=alibabacloud&logoColor=white"/> <img src="https://img.shields.io/badge/Blender_API-F5792A?style=flat-square&logo=blender&logoColor=white"/>

---
