2021 Data analysis-capstone-design
==================================
CNN을 활용한 재활용품 분류
---------------------------
산업경영공학과 18학번 김태성
## 1. 개요
### 1.1. 과제 선정 배경
- 최근 코로나로 인한 온라인 주문과 배달량 증가로 배출되는 쓰레기의 양이 증가되면서 기존에도 문제가 심각했던 쓰레기 대란 문제가 더욱 고조화 되었다. 이를 해결하는데 매립지와 소각시설과 같은 쓰레기 처리시설을 증설하는 것도 하나의 방법이지만 환경적, 경제적, 시간적 문제 등 다양한 제약이 존재한다. 
- 쓰레기 대란을 잠재우는 또 다른 방법은 정확한 분리 배출이다. 알맞은 분리 배출은 품질 좋은 재활용 원료를 추출할 수 있도록 하며 다양한 방면으로 사용될 수 있다. 그러나, 정확한 분리 배출이 이루어지지 않게 되면 수거된 쓰레기들을 쓰레기 매립 및 매각시설로 보내져 큰 효과를 보지 못하게 된다.    
- 현재 분리 배출은 의무화가 되어 있지만, 국민들의 자발적인 참여에 의존하고 있는 상황이다. 따라서 분리 배출에 대한 효과를 더욱 보기 위해선 알맞게 분리 배출이 이루어질 필요가 있다. 하지만 수거된 쓰레기를 일일이 다시 분류해 내는 작업은 많은 자원을 요구하기 때문에 자동으로 손쉽게 쓰레기를 분리 배출될 수 있도록 분류해주는 시스템이 필요하다. 

### 1.2. 과제 주요내용
- 많은 자원을 들이지 않고, 정확한 분리 배출이 이루어질 수 있도록 하기 위해 CNN 모델을 활용하여 자동화된 쓰레기 분류 모델을 만들고자 한다. 쓰레기 이미지를 통해 모델을 학습시키도록 하며, 재활용되는 'can', 'plastic', 'paper', 'glass'와 재활용 되지 않는 'trash'로 분류하도록 한다.

### 1.3. 최종결과물의 목표
- 이전에 진행되었던 쓰레기 분류 모델의 validation accuracy는 80%를 넘지 못했다. 따라서 이보다 개선된 더 높은 accuracy를 갖는 모델을 학습시키는 것을 목표로 한다.
https://github.com/vasantvohra/TrashNet

## 2. Dataset
### 2.1. Data 수집
직접 이미지를 수집한 것이 아닌 기존에 미리 수집되었던 이미지를 활용하였다. 총 21202개의 이미지를 수집할 수 있었다.
|class|training set|validation set|test set|total
|------|---|---|---|---|
|glass|2265|943|943|4151|
|plastic|1973|785|785|3543|
|paper|3204|1328|1328|5860|
|can|2895|965|964|4824|
|trash|1694|565|564|2824|   
    
수집 이미지 출처 : https://github.com/AgaMiko/waste-datasets-review

### 2.2. Data 전처리
- 효과적으로 학습시키기위해 이미지의 픽셀별 RGB값을 255로 나눠 0~1사이의 값으로 scaling 해주었다.
- 모델별로 필요에 따라서 horizontal_flip과 같은 data augmentation을 진행하였다.
- 모델에 따라 input size(target_size)가 다르기 때문에 그에 맞춰 이미지를 resize할 수 있도록 하였다. 

![image](https://user-images.githubusercontent.com/80897270/123382489-f6777280-d5cc-11eb-9c9a-edafe3be35f2.png)


## 3. Models
- 많이 알려져 있는 CNN모델 구조를 사용하도록 하며, 점점 복잡한 구조를 이용하여 학습시켜 보았다.
### 3.1. LeNet5

![image](https://user-images.githubusercontent.com/80897270/123383723-78b46680-d5ce-11eb-9bc9-57eb75f90696.png)

- CNN개념이 최초로 적용된 모델
- 3개의 convolution layer, 2개의 sub-sampling layer, 1개의 fully connected layer로 구성
- 훈련시킬 파라미터 수는 약 6만개로 다른 CNN 모델들의 비해 파라미터 수가 적다.
![image](https://user-images.githubusercontent.com/80897270/123383974-ca5cf100-d5ce-11eb-8de7-2979e44c9660.png)

#### 3.1.1. LeNet5 training

- Batch size : 32
- Learning rate : 0.005
- Batch normalization
- Weight initializer : he_normal
- Accuracy : 0.6690

![image](https://user-images.githubusercontent.com/80897270/123391511-11e77b00-d5d7-11eb-9568-ee4d695546c3.png)
********************************************
- Batch size : 32
- Learning rate : 0.005
- Drop out : 0.3
- Batch normalization
- Weight initializer : he_normal
- Accuracy : 0.6790

![image](https://user-images.githubusercontent.com/80897270/123391525-16ac2f00-d5d7-11eb-89c4-8c3b7da966e2.png)


### 3.2. AlexNet
![image](https://user-images.githubusercontent.com/80897270/123389104-80770980-d5d4-11eb-9dc9-fb89f7bd0c4c.png)    

- 5개의 convolution layers, 3개의 fully-connected layers로 구성
- 11x11, 5x5, 3x3의 filter를 사용    

![image](https://user-images.githubusercontent.com/80897270/123389278-ae5c4e00-d5d4-11eb-839a-fc8bd6f205b6.png)

#### 3.2.1 AlexNet training
- Batch size : 32
- Learning rate : 0.001
- Batch normalization
- Horizontal flip
- Accuracy : 0.8127

![image](https://user-images.githubusercontent.com/80897270/123391918-786c9900-d5d7-11eb-8858-16bf14b2c074.png)
********************************************
- Batch size : 32
- Learning rate : 0.001
- Batch normalization
- L2 normalization
- Horizontal flip
- Accuracy : 0.8136

![image](https://user-images.githubusercontent.com/80897270/123391939-7efb1080-d5d7-11eb-90b5-ef127a589075.png)
********************************************
- Batch size : 32
- Learning rate : 0.001
- Batch normalization
- Drop out : 0.3
- Horizontal flip
- Accuracy : 0.8114

![image](https://user-images.githubusercontent.com/80897270/123391991-89b5a580-d5d7-11eb-8fb6-75da6ff8135c.png)
********************************************
- Batch size : 32
- Learning rate : 0.01
- Batch normalization
- Drop out : 0.3
- Horizontal flip
- Accuracy : 0.8302

![image](https://user-images.githubusercontent.com/80897270/123392011-8de1c300-d5d7-11eb-8a0b-768ed0569013.png)


### 3.3. VGG 16
![image](https://user-images.githubusercontent.com/80897270/123389340-bf0cc400-d5d4-11eb-868d-10b8bd5896ba.png)    

- 13개의 convolution layers, 3개의 fully connected layers로 구성
- 3x3 filter만을 사용하여 깊지만 단순한 구조를 갖음    

![image](https://user-images.githubusercontent.com/80897270/123389582-fda27e80-d5d4-11eb-87f4-5109b95bb2fc.png)
![image](https://user-images.githubusercontent.com/80897270/123389599-01360580-d5d5-11eb-9c88-3aff09638f44.png)

#### 3.3.1. VGG 16 training

- Batch size : 32
- Learning rate : 0.01
- Batch normalization
- Drop out : 0.3
- Horizontal flip
- Accuracy : 0.8494

![image](https://user-images.githubusercontent.com/80897270/123392863-7eaf4500-d5d8-11eb-856e-055a98aceada.png)

********************************************

- Batch size : 64
- Learning rate : 0.01
- Batch normalization
- Drop out : 0.3
- Horizontal flip, width, height shift
- Accuracy : 0.8149

![image](https://user-images.githubusercontent.com/80897270/123392879-82db6280-d5d8-11eb-92ba-80ca0683a086.png)

********************************************
- Batch size : 16
- Learning rate : 0.005
- Batch normalization
- Drop out : 0.5
- Horizontal flip, width, height shift
- Weight Initialization : he_normal
- Accuracy : 0.8370

![image](https://user-images.githubusercontent.com/80897270/123392911-8c64ca80-d5d8-11eb-9140-8658d15c9196.png)

********************************************
- Batch size : 16
- Learning rate : 0.005
- Batch normalization
- Drop out : 0.3
- Horizontal flip, width, height shift, rotation, zoom
- Accuracy : 0.8665

![image](https://user-images.githubusercontent.com/80897270/123392934-925aab80-d5d8-11eb-806a-01507278dd93.png)


### 3.4. Inception v3
![image](https://user-images.githubusercontent.com/80897270/123389874-4b1eeb80-d5d5-11eb-85a2-85266e243d13.png)

- 작은 filter를 사용하여 깊으면서도 계산량을 줄일 수 있도록 함
- NxN filter만 사용하는 것이 아닌 Nx1 과 같은 asymmetric filter 사용    
![image](https://user-images.githubusercontent.com/80897270/123390561-00ea3a00-d5d6-11eb-9a53-20b082d1b87b.png)
![image](https://user-images.githubusercontent.com/80897270/123390576-06478480-d5d6-11eb-9703-a566573ec82f.png)
![image](https://user-images.githubusercontent.com/80897270/123390591-0a73a200-d5d6-11eb-8c5b-383edf8fbbb0.png) 

- 보조 분류기를 사용하여 성능 개선
- pooling layer로 grid size를 줄이는 것 대신 representational bottleneck을 줄이면서 계산 비용은 비교적 저렴하도록 하는 구조 사용    
![image](https://user-images.githubusercontent.com/80897270/123390633-13647380-d5d6-11eb-9d19-3eaca786c569.png)

#### 3.4.1. Inception v3 training
- Batch size : 32
- Learning rate : 0.001
- Batch normalization
- Drop out : 0.5
- Horizontal flip, width, height shift
- RMSprop
- Accuracy : 0.8370

![image](https://user-images.githubusercontent.com/80897270/123393375-fe3d1400-d5d8-11eb-9258-155cfeaf150d.png)

********************************************
- Batch size : 32
- Learning rate : 0.01
- Batch normalization
- Drop out : 0.5
- Horizontal flip, width, height shift
- SGD
- Accuracy : 0.8411


![image](https://user-images.githubusercontent.com/80897270/123393385-009f6e00-d5d9-11eb-91ea-2b7be33662fb.png)

********************************************
- Batch size : 16
- Learning rate : 0.005
- Batch normalization
- Drop out : 0.5
- Horizontal flip, width, height shift
- SGD
- Weight initializer : he_normal 
- Accuracy : 0.8654


![image](https://user-images.githubusercontent.com/80897270/123393393-039a5e80-d5d9-11eb-958a-304e23e1badf.png)

********************************************
- Batch size : 16
- Learning rate : 0.005
- Batch normalization
- Drop out : 0.5
- Horizontal flip, width, height shift, rotation, zoom
- SGD
- Weight initializer : he_normal
- Accuracy : 0.8558

![image](https://user-images.githubusercontent.com/80897270/123393399-05fcb880-d5d9-11eb-9ec2-025a4817f6e7.png)

## 4. Results
|모델|batch|learning rate|batch normalization|Drop out|augmentation|weight initializer|기타|accuracy|
|------|---|---|---|---|---|---|---|---|
|LeNet|32|0.005|O|X|X|random|SGD|0.6690|
|LeNet|32|0.005|O|0.3|X|He_normal|SGD|0.6790|
|AlexNet|32|0.001|O|X|Hf|random|SGD|0.8127|
|AlexNet|32|0.001|O|X|Hf|random|L2, SGD|0.8136|
|AlexNet|32|0.001|O|0.3|Hf|random|SGD|0.8114|
|AlexNet|32|0.01|O|0.3|Hf|random|SGD|0.8302|
|VGG 16|32|0.01|O|0.3|Hf|random|SGD|0.8494|
|VGG 16|64|0.01|O|0.3|Hf, wh|random|SGD|0.8149|
|VGG 16|16|0.005|O|0.5|Hf, wh|He_normal|SGD|0.8370|
|**VGG 16**|**16**|**0.005**|**O**|**0.3**|**Hf, wh, rz**|**random**|**SGD**|**0.8665**|
|Inception|32|0.001|O|0.5|Hf, wh|random|RMSprop|0.8370|
|Inception|32|0.01|O|0.5|Hf, wh|random|SGD|0.8411|
|Inception|16|0.005|O|0.5|Hf, wh|He_normal|SGD|0.8654|
|Inception|16|0.005|O|0.5|Hf, wh, rz|He_normal|SGD|0.8558|

- 굵은 글씨로 표시된 VGG 16에서 0.8665로 가장 높은 accuracy 달성
- 학습된 해당 모델로 최종적으로 test set에 대하여 성능을 검증해 보았다.
- 0.8224를 달성하였다.
- 깊은 구조로 갈수록 대체적으로 accuracy가 향상하는 것을 확인할 수 있었다.
- 모델별로 사용했을 때 효과적인 기법들이 상이했고, 오히려 성능을 떨어트리는 기법이 존재했다.
- 이를 테면 inception에 경우 동일한 hyper parameter임에도 불구하고 data augmentation으로 rotation과 zoom을 추가했을 때 오히려 성능이 떨어짐을 확인할 수 있었다.

## 5. Conclusion
### 5.1. 결론 및 제언
- 이론으로만 학습했던 모델들을 직접구현함으로써 모델에 대한 이해도를 높일 수 있었다.
- 모델별로 성능을 향상시키기 위한 여러 기법들이 동일하게 성능을 개선시키는 것이 아니라 모델에 따라서 개선의 정도가 상이했다.
- 설령 같은 모델이라 할지라도 어떤 hyper parameter로 학습시켰는지에 따라서도 성능이 개선되는 모델이 있는 가 하면 오히려 악영향을 미친 경우도 있었다.
- 딥러닝에서는 만병통치약 같이 어떤 모델이나 어떠한 domain에서도 공통되어 성능을 개선시키는 기법이 있는 것이 아니라 본인의 data와 여러 hyper parameter에 따라 성능이 천차만별이 되기때문에 본인의 상황에 맞게 다양한 기법과 hyper parameter를 수정해 학습시켜가며, 최적의 성능을 얻도록 하는 것이 바람직함을 다시금 느낄수 있었다.
### 5.2. 활용 방안
- 이미지를 통해서만 쓰레기를 분류하는데 한계가 존재한다고 생각한다. 그러므로 이미지 뿐만아니라 무게, 재활용마크 등 다른 부가적인 요소도 참고하여 분류해 낼수 있도록 한다면 더욱 높은 정확도로 분류해 낼수 있을 것이다. 
- 더욱 품질 높은 재활용 원료를 추출하기 위해선 정확한 분리 배출뿐만 아니라 올바르게 배출하는 것도 중요하다. 이를 테면 페트병의 라벨을 제거하여 버려야 하거나 안에 내용물을 비우고 배출해야 한다. 따라서 수거된 쓰레기를 분류하는 것 뿐만 아니라 잘못된 방법으로 배출된 쓰레기를 검출해내거나 올바르게 배출될 수 있도록 하는 시스템이 필요하다.
