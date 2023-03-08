## Todo
## 논문 읽기

### CoOp

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

### CoCoOp

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

### Tip Adapter

- 

## 가설 세우기

- Clip model은 매칭되는 이미지 임베딩과 텍스트 임베딩의 특징 벡터가 벡터공간 상에서 가까이 나타나게끔 학습하였다.
    - 각 인코더의 마지막 층은 동일한 특징을 표현하게끔 학습되었다.
    - Low level의 레이어에서는 가장 단순한, visual하거나 textual한 특징을 추출할 것이다.
    - Middle level의 레이어에서 어떤 특징을 추출하는가에 대해서는 미지의 영역이다.
        - 텍스트 인코더에 대해 실험적으로 알아낸 사실은 다음과 같다.
        
        <aside>
        💡 3번째 레이어의 6번째 head는 명사를 추출하는 경향을 보였다.
        6번째 레이어의 0번째 head는 전체적인 흐름, 상황에 대한 특징을 추출하는 경향을 보였다.
        8번째 레이어의 7번째 head는 photo of나 image of와 같은 특징을 추출하는 경향을 보였다.
        마지막 11번째 레이어의 5번째 head는 분류 대상을 특정하는 경향을 보였다.
        
        </aside>
        
    - 가설 1. (Visual-Language 모델에 대한 문제점) Image feature과 같은 공간 상에 나타내기 위하여 Textual encoder의 Low level에서 나타나는 non-visual한 features는 high level로 갈수록 소멸할 것이다.
        - “내 아들은 수영 실력이 뛰어나다” 와 같은 caption이 주어진다면 clip 모델의 high level에서 나타난 표현에 ‘실력’ 이나 ‘뛰어나다’에 대한 특징은 없거나 변형되었을 것이다. 그리고 ‘아들’은 ‘어린 남성’이라고 표현되지 않을까? 이는 학습이 특정 성향으로 편향될 수 있을 것이다.
    - 가설 2. (Clip 모델의 학습 데이터에 대한 문제점) 하나의 이미지를 글로 표현하는 방법에는 무한히 많은 방법이 있을 것이다. 그러나 학습에 사용된 사진에 대한 caption에서 일반적으로 사진의 배경까지 세세하게 묘사하지는 않았을 것이다. 따라서 Visual encoder의 Low level에서 나타나는 세세한 특징들 중 일부는 high level로 갈 때 소멸될 것이다.
        - 강아지가 들판에서 뛰어노는 사진 : 들판을 이루는 나무나 놀이에 사용되는 물건에 대한 묘사는 보통 하지 않을 것이다.
        - 마찬가지로 주로 활용되는 표현을 학습하여 편향된 모델이 만들어질 수 있을 것이다.
- 내 모델의 강점
    - 가설 1.에 따라 low-level의 non-visual한 features까지 활용할 수 있다면 더 정확한 image captioning 작업이 가능해질 것이다. (이 경우 모든 층의 feature를 활용할 수 있게끔 score function을 수정해야 하지 않을까? 지금의 가장 두드러진 특징만 사용하는 방법으로 가능할까?)
        - 뿐만 아니라 자세한 caption을 넣었을 때
        - 단순 이미지 분석을 넘어 감성분석과 같은 downstream task에서도 좋은 성능을 보이지 않을까?
    - 가설 2.에 따라 버려지는 low level의 특징들도 학습에 활용한다면 배경 이미지로부터 구한 세세한 특징과 분류 대상을 연결지을 수 있을 것이다.(low-image_features & high-text_features) 그렇게 된다면 대상을 분류하기 어려운 케이스도 배경에 대한 정보로부터 대상을 추측할 수 있지 않을까?
- [x]  정량분석에 대한 가설 세우기 : 내 모델의 성능이 잘 나타나는가
    1. Image captioning 실험을 할 경우 기존의 clip 모델보다 섬세한 결과를 얻을 수 있을 것이다.
    2. 분류 대상이 뭉개진 이미지로부터 대상을 잘 분류할 수 있을 것이다.
- [x]  정성분석에 대한 가설 세우기 : 왜 잘 나타나는지 설명할 수 있어야
    - 위의 ‘내 모델의 강점’에서 설명하였다.

## 아직 부족하다고 느끼는 부분

- 위에 적은 모델의 강점을 살리기 위해선 score function을 많이 개선할 필요가 있을 듯 하다.
- Domain generalization 문제 해결에 있어서 기존의 모델보다 나아진 부분이 있는지 생각해보아야겠다.

## 실험하기

- linear probing model
- domain generalization test
    - train : P / test : A,C,S
    - …

## 실험 기획하기

- Image captioning
    - Todo
        - One image and output captions table
        - 우리 모델의 강점을 부각할 수 있는 새로운 지표 생각해보기
            
            ![9F0A8F8D-85A9-4A27-96C8-67C09ED49C14.jpeg](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/136cdaeb-7e91-45ac-bca3-a8222da2ed10/9F0A8F8D-85A9-4A27-96C8-67C09ED49C14.jpeg)
            
    - ClipCap과 비교, CoOp, CoCoOp을 활용한 captioning과도 비교
- Image classify
    - Todo
        - T-SNE graph
        - 성능 비교 도표
        - 성능 비교 그래프
            - 0,1,2,4,8,16-shot train graph 그리기
                
                ![265BA77F-7266-4E55-8B1D-E824FA4E26F2.jpeg](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9c395faf-1c72-4a66-b934-7bae7012f58e/265BA77F-7266-4E55-8B1D-E824FA4E26F2.jpeg)
                
            - Dataset에 따라 비교 그래프 그리기
                
                ![42B85E77-FDD5-4C9B-8F9B-3E3A6B19E27F.jpeg](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cd4b565e-ea62-4aa1-854d-de062a488833/42B85E77-FDD5-4C9B-8F9B-3E3A6B19E27F.jpeg)
                
            - 확연히 좋아진 데이터셋이 있다면 아래 그래프로 강조하기
                
                ![F8537227-BE74-4392-81DC-9AFF98C79C08.jpeg](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/56a670b3-5dde-4e2f-be0e-1cb50c812bd2/F8537227-BE74-4392-81DC-9AFF98C79C08.jpeg)
                
    - Dataset
        - 뭉개진 이미지 데이터셋
        - imagenet
    - Model
        - Clip probing model
        - Clip zeroshot
        - Mymodel
        - CoOp, CoCoOp
    
    <aside>
    💡 Few-shot model을 학습시킬 때 epoch의 수는 크게 늘려도 상관 없는지?
    
    </aside>

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

---
- epoch = 200
- loss function = CrossEntropyLoss
- optimizer = SGD(leraning-rate = 0.001, momentum=0.1)
- linear probing score : 93.3340 %