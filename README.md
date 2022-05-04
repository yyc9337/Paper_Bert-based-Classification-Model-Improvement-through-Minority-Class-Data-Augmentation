# Paper_Bert-based-Classification-Model-Improvement-through-Minority-Class-Data-Augmentation

## 논문 요약
 자연어처리 분야에서 딥러닝 기반의 분류 모델은 획기적인 성능을 보여주고 있다. 특히 2018 년
발표된 구글의 BERT 는 다양한 태스크에서 높은 성능을 보여준다. 본 논문에서는 이러한
BERT 가 클래스 불균형이 심한 데이터에 대해 어느 정도 성능을 보여주는지 확인하고 이를
해결하는 방법으로 EDA 를 선택해 성능을 개선하고자 한다. BERT 에 알맞게 적용하기 위해
다양한 방법으로 EDA 를 구현했고 이에 대한 성능을 평가하였다. 

### 서론 
 모든 뉴럴 네트워크가 그렇듯 BERT 역시 학습시키는 데이터의 클래스가 비슷한 비율로
구성되어 있지 않으면 모델의 성능이 저하되게 된다. 이와 같은 문제를 데이터 불균형 
문제라고 하며 소수 클래스에 속하는 데이터들이 다수 클래스에 속하는 데이터보다 잘못
분류되는 경우가 생길 수 있다[4]. 이를 해결하기 위해 더 많은 데이터를 수집하거나
성능지표를 변경하는 등의 다양한 기법들이 존재한다. 본 논문에서는 다양한 데이터 불
균형 처리 기법을 BERT 모델 맞게 적용해 실험하고 결과를 비교하는 것을 목표로 한다.

### 실험 환경 및 구성
 논문에서는 특정 지역 지방 경찰청의 과거 112 신고 내용을 학습하여 신규 접수 시 유형을 예측
 하기 위한 연구로 진행하였지만 필자는 캐글의 악성댓글 분류 대회(Toxic Comment Classifica
 tion Challenge | Kaggle)를 인용하여 실험 환경을 구성하겠다. 
 

1) 해결 방법: SMOTE(Synthetic Minority Oversampling Technique)
SMOTE는 오버샘플링 방법 중 하나로, 소수의 클래스에 속하는 데이터 주변에 원본 데이터와 동일하지 않으면서 소수의 클래스에 해당하는 가상의 데이터를 생성하는 방법입니다.

2) 해결 방법: Focal loss
 Focal loss는 Object Detection에서 학습 중 클래스 불균형 문제를 손실 함수로 해결하기 위해 고안된 방법입니다. Cross entropy의 클래스 불균형 문제는 백그라운드로 분류될 수 있는 easy negative가 대부분이어서 학습에 비효율적입니다. Focal loss는 Cross Entropy의 이런 클래스 불균형 문제를 개선해, 어렵거나 쉽게 잘못 분류되는 케이스에 더 큰 가중치를 줍니다. Focal loss에 대해 간단히 요약하자면, 잘 찾은 class의 경우에는 loss를 적게 줘 loss 갱신을 거의 하지 못하게 하고, 잘 찾지 못한 class의 경우 loss를 크게 줘서 loss 갱신을 크게 하는 것입니다. 결론적으로 잘 찾지 못한 class에 대해 더 집중해서 학습하도록 하는 방법이라고 볼 수 있습니다.
 
3) 해결 방법: EDA(Easy Data Augmentation)
EDA는 학습 데이터가 부족하거나, 불균형 문제가 발생했을 때, 현재 보유하고 있는 데이터를 변형시켜 데이터의 양을 늘리는 기법입니다. EDA에는 다음과 같은 기법들이 있습니다.
① SR(Synonym Replacement, 동의어 교체): 문장에서 랜덤으로 stop words가 아닌 단어들 중 n개를 선택해 임의로 선택한 동의어들 중 하나로 바꿈
② RI(Random Insertion, 무작위 삽입): stop word를 제외한 나머지 단어들 중, 랜덤으로 단어를 선택하여 동의어를 임의어로 정하고, 이를 각 문장 내에 임의의 자리에 넣음
③ RS(Random Swap, 무작위 교체): 각 문장에서 무작위로 두 단어를 선택해 그 위치를 바꿈
④ RD(Random Deletion, 무작위 삭제): 각 문장 내에서 랜덤하게 단어를 선택해 이를 삭제함
