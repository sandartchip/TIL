

# ResNet50
- ResNet은 간단한 구조를 가지고,  참신한 아이디어로 현재까지도 많은 network에서 사용되고 있음. 

### Motivation 
 
- ResNet이 나오기 전까지, 근방의 모델은 오직 깊은 모델, 즉 레이어를 많이/깊이 쌓아서 성능이 높은 모델을 고르는 데 치중됨.
- ResNet의 저자는 모델이 깊어질 수록 최적화에 멀어진다는 점에 주목함. 

##### Vanishing Gradient Problem
<img src="https://github.com/sandartchip/TIL/assets/15938354/08c5dbce-28b0-4bb6-bbed-2758b7bacd4e" width="500px"/>

- Vanishing Gradient Problem은 딥러닝 중 Backpropagation 시 발생되는 어려움이다.
- 간단히 말하면, 모델이 깊어질 수록 급격히 Gradient의 영향력이 줄어드는 현상이다.

### Residual Net
<img src="https://github.com/sandartchip/TIL/assets/15938354/34c5227a-ad2f-45a7-8a01-ca3755900f30" width="500px"/>

- ResNet 모델에 사용되는 가장 기본적인 구조.
- BottleNeck Architecture(Residual Block)이라고 불림.
- 같은 input, output을 취함.
``` input:x, output: relu(F(X)+x) ```

- 이렇게 구조를 띈 layer는 입력값이 출력으로 그대로 더해지는 short 구조를 취하게 됨. 
- 입력 값이 출력값에 들어감에 따라, 레이어들이 많아지면서 잊혀지는 vanishing gradient problem을 해결하게 됨.

#### ResNet50 Layers
<img src="https://github.com/sandartchip/TIL/assets/15938354/372360b1-3291-4afc-924c-9deccebf41b7" width="700px"/>
- 1+1+ 9+12+18+9 = 50
- 총 50개의 layer를 갖고 있으며, 각각의 conv 청크에 있는 []가 residual Block임을 명시해 줌.
- 


#### code로 구현
- 각 residual 함수 F에 관하여, 3개의 Layer stack으로 구현하게 됨.
- 3개의 layer는 1x1, 3x3, 

##### BottleNeck Architecture 
- 
- 각 residual 함수 F에 관하여, 3개의 Layer stack으로 구현하게 됨.
- 3개의 Layer는 1x1, 3x3, 1x1 convolutional layer로 이루어짐.

- 각각의 Convolution -> Batch Normalization -> ReLu로 묶인 블럭 별로
Residual 값을 추가적으로 더해줌으로써 Gradient Vanishing/Exploding 문제를 해결할 수 있다.

- Batch Normalization : 처음 input을 normalize한 다음 CNN을 통과하면 normalize상태가 무너짐. 걔가 normal distribution을 따른다는 보장이 없음
- 그래서 한 번 더 Normalize해주는 것.

<img src="https://github.com/sandartchip/TIL/assets/15938354/1429e480-2dac-48ac-842f-3d6b0f13476e" width="500px">

- 중간중간에 Fully Connected 층을 통해서 나오는 값에 붙여 줌
- 그 과정을 batch별로(?) 수행


https://jisuhan.tistory.com/71