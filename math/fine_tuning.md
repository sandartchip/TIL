

### Fine-Tuning (미세조정)
- fine-tuning은 결과를 극대화시키기 위해 무언가를 살짝 조정하는 것을 의미함.

#### 방법
- 모델의 끝 단을 바꿔 끼워 마지막 layer의 w와 B를 initialize해 전체적 재학습
- 


시나리오 1 – 데이터 세트의 크기는 작은 반면 데이터 유사성은 매우 높은 경우
이 경우 데이터 유사도가 매우 높기 때문에 모델을 재학습할 필요가 없다. 해야 할 일은 출력 레이어를 해당 문제에 맞게 수정하는 것이다. 
(softmax layer와 dense layer만 수정) 사전 훈련된 모델을 feature extrator로 사용한다.

시나리오 2 - 데이터 크기가 작고 데이터 유사도가 매우 낮은 경우
이 경우 사전 훈련된 모델의 초기 layers 을 freeze하고 나머지 layers만 다시 훈련할 수 있다. 
그런 다음 top layer는 새 데이터 세트에 맞게 커스텀 한다. 새 데이터 세트는 유사성이 낮기 때문에 새 데이터 세트에 따라 상위 layer(출력에 가까운 layer)를 다시 훈련하고 커스텀하는 것이 중요하다. 작은 크기의 데이터 세트는 초기 layer가 pretrained된 상태로 유지되고(이전에 큰 데이터 세트에서 훈련된) 해당 계층의 가중치가 고정됨을 의미한다.

시나리오 3 - 데이터 세트의 크기는 크지만 데이터 유사성은 매우 낮은 경우
이 경우 데이터 세트가 크기 때문에 신경망 훈련 하는 것 자체가 효과적이다. 
그러나 데이터는 사전 훈련된 모델을 훈련하는 데 사용된 데이터와 비교할 때 매우 다르기때문에 사전 훈련된 모델을 사용하여 만든 예측은 효과적이지 않다. 따라서 데이터에 따라 신경망을 처음부터 훈련하는 것이 가장 좋다.

시나리오 4 – 데이터의 크기가 크고 데이터 유사도가 높은 경우
이상적인 상황. 이 경우 사전 훈련된 모델이 가장 효과적인 상황이라고 볼 수 있다. 
모델을 사용하는 가장 좋은 방법은 모델의 아키텍처와 모델의 초기 가중치를 유지하는 것이다. 그런 다음 사전 훈련된 모델에서 초기화된 가중치를 사용하여 이 모델을 다시 훈련할 수 있다.

https://velog.io/@boom109/Transfer-learning-and-Pre-trained-Models-%ED%99%9C%EC%9A%A9
