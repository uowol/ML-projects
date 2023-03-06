## Todo
- 논문 읽기
    - [x]  CoOp
        - traditional representation learning 방법에 대한 문제 제기, 기존 VL 모델의 문제 제기
        - prompt learning의 새로운 방법 제시
        - 고정된 prompt 대신 학습가능한 context vector를 사용함으로써 task 에 맞는 prompt를 알맞게 생성할 수 있다.
        - 실험
            1. Zeroshot, linear probing model과의 성능 비교 그래프 그리기
                1. 가로축 : n-shot, 세로축 : 성능
                2. 평균 성능 그래프와 각각에 데이터에 대한 그래프
            2. 본 모델을 Zeroshot model과 비교했을 때의 성능 비교 그래프 그리기
                1. 개선하고자 한 모델을 대상으로 비교 그래프 그리기
                2. 우리 모델의 강점이라고 생각한 부분이 비교 그래프에서 드러났는지 확인하기
            3. Domain generalization test 표 작성하기
                1. Source: ImageNet, Target: ImageNet-V2/Sketch/A/R
                2. num of data at each class : 16
            4. Average Barplot (vision backbones)
                1. 앞에서 그린 선 그래프를 바 그래프로 그려 비교 대상과의 차이를 부각하였음
                2. 우리의 모델 또한 함께 비교해 보기
    - [x]  CoCoOp
        - CoOp 모델에 대한 문제 제기 : 주어진 class에 과적합, 따라서 unseen class에 대한 성능이 떨어짐
        - context vector과 함께 주어진 image에 대한 input-conditional token(vector)을 사용함으로써 더 일반적인 prompt를 만들 수 있다. 이는 scene recognition에서 중요한 요소로, 전이학습과 DG 문제 해결에 좋은 성능을 보였다.
        - 아쉬운 점은 하나의 token이 아니라 v1. …, vm 각각에 대한 token이 주어졌을 때의 성능이 어떻게 나타나는지 적혀있지 않아 성능과 계산 효율성을 얼마나 적절하게 챙길 수 있었는지 알 수 없었다.
        - 실험
            1. 자신의 모델의 장점을 부각할 수 있는 새로운 지표를 만들었음(Table 1)
                1. 우리의 모델의 강점이 드러나는 지표 떠올리기
            2. CoOp 모델을 개선하고자 한 방향이 잘 반영되었는지 비교 그래프로 확인하기
                1. Figure 3에서 본 모델이 세웠던 가설을 잘 따르고 있음을 보여주었음
                2. 우리의 모델 역시 우리가 세운 가설을 잘 따르는지 확인하기
            3. CoOp 논문과 마찬가지로 전이학습과 DG 성능을 보여줌으로써 VLP model인 CLIP 모델을 개선시킨 만큼 VLP 모델의 특징인 downstream task로의 적용이 용이한 것을 증명함
                1. 우리도 CLIP 모델을 개선하는 것인 만큼 비슷한 실험을 진행하면 좋을 듯 하다
    - [ ]  Tip Adapter
    - 어떤 실험을 했는지 참고하고 괜찮은 실험이 있다면 내 모델에 적용해보기
- 가설 세우기
    - [ ]  정량분석 : 내 모델의 성능이 잘 나타나는가
        - 가설 : |
    - [ ]  정성분석 : 왜 잘 나타나는지 설명할 수 있어야
        - 가설 : |
- 0,1,2,4,8,16-shot train graph 그리기
    - [ ]  clip zeroshot
    - [ ]  clip probing model
    - [ ]  mymodel
- domain generalization 성능 측정 (16-shot)
    - [ ]  source : ImageNet
    - [ ]  target : -V2
    - [ ]  target : -Sketch
    - [ ]  target : -A
    - [ ]  target : -R

## 성능 측정

- epoch = 6
- loss function = CrossEntropyLoss
- optimizer = SGD(leraning-rate = 0.001, momentum=0.1)
![그림1](https://user-images.githubusercontent.com/20416616/223057176-0c4046ad-1e85-4e63-8be9-c951b8076558.png)
---

- epoch = 200
- loss function = CrossEntropyLoss
- optimizer = SGD(leraning-rate = 0.001, momentum=0.1)
![그림2](https://user-images.githubusercontent.com/20416616/223057217-a9999551-d17c-42a2-820f-f7ff5fe0a884.png)

---
- epoch = 500
- loss function = CrossEntropyLoss
- optimizer = SGD(leraning-rate = 0.001, momentum=0.1)
![그림3](https://user-images.githubusercontent.com/20416616/223057256-2e6e7d8d-445d-4932-a893-b10c40a58e1a.png)
