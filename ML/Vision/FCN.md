

# FCN(Fully Convolutional Networks) 
- 기존의 네트워크들은 아키텍쳐의 끝 부분에 FC Layer가 있어 공간 정보를 담지 못한 채 
- FCN은 픽셀 단위의 예측(pixelwise prediction)을 수행 함.
- image recognition에 일반적으로 사용하는 FC(Fully Connected Layer) Classifier를 CNN으로 바꿔주어 공간 정보를 보전하고 다양한 Image size를 입력으로 받음. 

- convolution을 진행 후, Upsampling 과정을 거쳐 원래 이미지와 똑같은 크기의 feature map을 만듬. 
- 이 feature map 의 픽셀 하나하나를 classification 결과 값으로 하여, segmentation을 진행 함. 
- Convolution Layer들 만으로 Segmentation 수행.

- NIN(Network in Network, 1x1 Conv) 로 FC(Fully Connected Layer)를 대체하여 Spatial한 정보를 보존 함. (->???) 



### Upsampling is backwards strided convolution 
- Coarse(거친, 알맹이가 큰) Map을 원본 이미지 크기에 가까운 Dense Map으로 변환해야 함.
- 방법 1: Deconvolution (backward strided convolution)
- 방법 2: Bilinear Interpolation


### Skip Architecture
- 최종 Feature map은 지역 정보를 '대략적으로' 유지하고 있어 이미지가 뭉개진다.
- 이를 방지하기 위해, **feature 추출 단계의 feature map도 Upsampling에 포함(Sum)하여 정보 손실을 막자**는 것이 핵심 아이디어이다. 

### 참고자료
https://nolja98.tistory.com/295
