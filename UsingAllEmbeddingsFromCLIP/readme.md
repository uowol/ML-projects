# ~02.27

# 성능 측정하기

- epoch = 3
- loss function = CrossEntropyLoss
- optimizer = SGD(leraning-rate = 0.001, momentum=0.1)
1. train all by one tensor / max_score
Testing accuracy: 94.9 %
2. train all by one tensor / mean_score
Testing accuracy: 35.6 %
3. train all by one tensor / cnt_score
Testing accuracy: 14.4 %
4. train all by each tensor / max_score
Testing accuracy: 95.9 % (copy last weight)
Testing accuracy: 92.0 % (random init)
5. train all by each tensor / mean_score
Testing accuracy: 15.5 %
Testing accuracy: 33.3 %
6. train all by each tensor / cnt_score
Testing accuracy: 14.6 %
Testing accuracy: 14.7 %
7. non_train / max_score
Testing accuracy: 44.1 %
8. non_train / mean_score
Testing accuracy: 13.9 %
9. non_train / cnt_score
Testing accuracy: 14.1 %
10. Zeroshot clip image classification
Testing accuracy: 94.0 %