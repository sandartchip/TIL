# python -m 

- 파이썬에서 모듈을 실행한다.

- 보통 pip를 python3 또는 python2에서 적절하게 실행하고자 할 때 사용함. 

```
python2 -m pip install pycrypto 
python3 -m pip install pycrypto 
```

# print 함수

- end 옵션과 sep 옵션이 있다.
 
- end 옵션: 마지막 문자열 출력 후 그 다음에 출력할 문자  

- sep 옵션 : 출력할 때 출력값 들 사이에 넣어질 구문자.
  ### end 옵션의 기본값은 개행(줄바꿈)이고
  
  print(a)
  print(b)가
  
  ```
   a
   b
  ```
  로 출력되는 이유는 end옵션 때문이다.
  
  ### sep 옵션의 기본값은 공백(띄어쓰기)다.
   print('a', 'b')가 ```a b```로 출력되는 이유는 sep 옵션 때문이다.





# 문자열 리터럴 
```python

        cmd_str += "C:\\Naver_MYBOX\\so_django\\so_project\\output\\210318 -t 8"
        print("----CMD STR ----")
        print(cmd_str)
```

파이썬에서 문자열 리터럴을 표기하는 경우,
역슬래시를 사용하기 위해서는 이스케이프 처리를 위해 **역슬래시 두 번 사용((

-컴퓨터에서 사용되는 경로: 역슬래시 \
-파이썬에서 사용되는 경로: 슬래시 /

-매번 귀찮지만 역슬래시를 슬래시로 바꿔주는 작업 해야 함




