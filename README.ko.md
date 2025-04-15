[English](README.md) | [한국어](README.ko.md)


# [Ariel-Data-Challenge-2024](https://www.kaggle.com/competitions/ariel-data-challenge-2024/overview)
## 개요
🏆 이 대회의 주요 과제는 각 관측에서 대기 스펙트럼을 추출하고, 그 불확실성 수준을 추정하는 것이다.

태양 이외의 별을 도는 행성인 외계행성이 우리 시야에서 별 앞을 통과할 때, 별빛의 극히 일부(백만 개당 50~200개의 광자)가 행성의 대기 링을 통과하며 대기 흡수 스펙트럼이 발생. 이러한 미약한 신호는 일반적으로 Super-Earth와 같은 행성의 경우 50ppm, 목성 같은 행성의 경우 200ppm 정도의 크기를 가지며 이는 여러 노이즈로 뒤덮이는데, 이러한 노이즈의 주요 원인은 우주 공간에서의 필연적인 망원경 포인팅 진동, 지터 노이즈(약 200ppm)가 있다.
이 외에도 다양한 상관 노이즈와 비상관 노이즈들이 존재한다.

## 데이터셋
약 1,000개의 외계행성들이 모항성 앞을 지날 때(일식) 관측하여 수집하였다.

### 메타데이터
- adc_info.csv: 아날로그-디지털(ADC) 변환 파라미터(이득과 오프셋)를 포함하고 있으며, 데이터의 원래 dynamic range를 복원하는 데 사용됩니다. 또한 해당 행성 시뮬레이션에 사용된 별을 식별하는 star 열 포함.

- train_labels.csv: 실제 대기 스펙트럼(Ground truth spectra).

- axis_info.parquet: 두 기기의 축 정보.

- wavelength.csv: 데이터셋의 각 실제 스펙트럼에 대한 파장 값.

### 신호 파일
데이터셋은 두 개의 별도 기기에서 얻은 시계열 이미지와 보정(calibration) 데이터로 구성된다. Ariel은 서로 다른 스펙트럼 대역 및 관측 모드를 전문으로 하는 다양한 광학 기기를 포함하고 있다.

- FGS1_signal.parquet: 0.60 ~ 0.80 µm, `numpy.reshape(135000, 32, 32)`로 데이터 이미지 복원 가능.

- AIRS-CH0_signal.parquet: 1.95 ~ 3.90 µm, `numpy.reshape(11250, 32, 356)`로 데이터 이미지 복원 가능.

### 보정 파일 (Calibration files)
보정 파일은 센서의 전자적 특성을 기록하며, 신호 대 잡음비(SNR) 향상을 위한 이미지 후처리 시 "보조 프레임" 역할을 한다.

- dark.parquet: 셔터를 닫은 상태에서 촬영된 dark frame으로, 센서의 열 노이즈와 바이어스 레벨을 캡처한다. 이미지에서 dark current를 제거하는 데 사용된다.

- dead.parquet: 센서 내의 고장났거나 핫 픽셀인 픽셀 정보. 죽은 픽셀은 빛에 반응하지 않으며, 핫 픽셀은 빛과 관계없이 항상 높은 신호 값을 생성한다.

- flat.parquet: 균일하게 조명된 표면을 촬영해 생성된 플랫 필드 프레임. 픽셀 간 민감도 및 광학계 불규칙성을 보정하는 데 사용된다.

- linear_corr.parquet: 센서의 선형성 보정 정보. 픽셀에 전자가 축적되면서 포화 상태에 가까워지면 반응이 비선형이 되며, 더 이상 빛에 반응하지 않는다. 수신된 전하에 대한 기기 반응을 보정하기 위해 다항식 보정 모델이 적용되며, 이는 실제 측정된 전자 수를 선형 반응으로 환산해준다.

- read.parquet: 센서에서 신호를 읽어들이는 과정에서 발생하는 전자 노이즈를 캡처한 노이즈 프레임으로 빛이 없을 때도 존재하는 노이즈.

## 도메인 지식 (Domain knowledge)
[Transits and Occultations](https://opaque-sword-c15.notion.site/Transit-Occeltation-review-paper-overview-213d57682fa24748a886c6df2d5edc04?pvs=4) 리뷰 논문 정리

## 감사 (Acknowledgements)
이 프로젝트는 Sergei Fironov의 작업을 부분적으로 기반으로 하고 있음:

[MobileNet Training on Kaggle](https://www.kaggle.com/code/sergeifironov/mobilenet-training)        
[ariel_only_correlation on Kaggle](https://www.kaggle.com/code/sergeifironov/ariel-only-correlation)        
[ariel_local_purple_hat_inference on Kaggle](https://www.kaggle.com/code/sergeifironov/ariel-local-purple-hat-inference)     

원본 노트북은 Apache License 2.0 하에 배포되었으며, 우리는 해당 라이선스 조건에 따라 일부 학습 파이프라인 코드를 재사용 및 수정하였음.
