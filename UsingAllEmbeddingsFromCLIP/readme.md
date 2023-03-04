## Todo
- 논문 읽기
    - [x] CoOp
    - [] CoCoOp
    - [] Tip Adapter
    - 어떤 실험을 했는지 참고하고 괜찮은 실험이 있다면 내 모델에 적용해보기
- 가설 세우기
    - [] 정량분석 : 내 모델의 성능이 잘 나타나는가
        - 가설 : | 
    - [] 정성분석 : 왜 잘 나타나는지 설명할 수 있어야
        - 가설 : | 
- 0,1,2,4,8,16-shot train graph 그리기
    - [] clip zeroshot
    - [] clip probing model
    - [] mymodel
- domain generalization 성능 측정    (16-shot)
    - [] source : ImageNet
    - [] target : -V2
    - [] target : -Sketch
    - [] target : -A
    - [] target : -R

## 성능 측정

- Zeroshot clip image classification
Testing accuracy: 93.80
![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b3f781ed-9cfe-4b48-814c-f9106d0721f5/Untitled.png)
---

- epoch = 200
- loss function = CrossEntropyLoss
- optimizer = SGD(leraning-rate = 0.001, momentum=0.1)
1. train all by one tensor / max_score
Testing accuracy: 95.3959 % (최대 95.4259 %)
2. train all by one tensor / mean_score
Testing accuracy: 83.4051 %
3. train all by one tensor / count_score
Testing accuracy: 17.3056 %
4. train all by one tensor / new_score
Testing accuracy: 93.5842 %
5. train all by each tensor(init with last weight) / max_score
Testing accuracy: 95.3658 %
6. train all by each tensor(init with last weight) / mean_score
Testing accuracy: 89.2904 %
7. train all by each tensor(init with last weight) / count_score
Testing accuracy: 17.2956 %
8. train all by each tensor(init with last weight) / new_score
Testing accuracy: 92.1529 %
9. train all by each tensor(init with randn weight) / max_score
Testing accuracy: 95.3858 %
10. train all by each tensor(init with randn weight) / mean_score
Testing accuracy: 89.9810 %
11. train all by each tensor(init with randn weight) / count_score
Testing accuracy: 17.3056 %
12. train all by each tensor(init with randn weight) / new_score
Testing accuracy: 95.3758 %
---
- epoch = 500
- loss function = CrossEntropyLoss
- optimizer = SGD(leraning-rate = 0.001, momentum=0.1)
1. train all by one tensor / max_score
Testing accuracy: 93.6443
2. train all by one tensor / mean_score
Testing accuracy: 85.0365 
4. train all by one tensor / new_score
Testing accuracy: 94.3249
5. train all by each tensor(init with last weight) / max_score
Testing accuracy: 
6. train all by each tensor(init with last weight) / mean_score
Testing accuracy: 
8. train all by each tensor(init with last weight) / new_score
Testing accuracy: 
9. train all by each tensor(init with randn weight) / max_score
Testing accuracy: 
10. train all by each tensor(init with randn weight) / mean_score
Testing accuracy: 
12. train all by each tensor(init with randn weight) / new_score
Testing accuracy: 