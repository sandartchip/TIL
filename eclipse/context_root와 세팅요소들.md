### Xmx, Xms


### context root(path)

WAS(Web application service)에서 
웹 어플리케이션을 구분하기 위한 path

예시)

```
localhost:8080/context_root/main.do
```

### 변경방법


1. 프로젝트 우클릭 ->  [Properties] - [Web Project Settings]에 Context root에서 설정 후 적용
![image](https://user-images.githubusercontent.com/15938354/118432474-a1b22380-b713-11eb-821e-28c1699a5ede.png)


2. Server.xml 

코드 아래로 내려가면 Context 태그 내에 path값을 원하는 값으로 변경해주면 된다.
![image](https://user-images.githubusercontent.com/15938354/118432393-72031b80-b713-11eb-8386-d9a1d91f949b.png)
