
![image](https://user-images.githubusercontent.com/15938354/145309448-3b64fde1-978b-4922-a2e3-bdd25ed3e399.png)

- 리눅스의 쉘은, 커널과 사용자간의 다리 역할을 한다.
- 사용자로부터 명령어를 받아 그것을 해석하고 프로그램을 실행한다.

### 영구적인 환경 설정 스크립트(​Login shell vs Non-login shell)



- 리눅스에는 로그인 하면 자동으로 실행되는 스크립트 파일이 있는데 이걸 * 환경 초기화 스크립트* 라고 한다. 
(그런데, 이건 다른 유저로 로그인 후 전환하면 실행이 안 됨.)

- 로그인 하는 대상에 따라서 다음의 표와같이 실행되는 스크립트가 구분되며,  환경 초기화 스크립트에  적어두면 로그인할때마다 자동으로 환경 변수를 설정할수 있다.

![](https://images.velog.io/images/sandartchip/post/c91989fb-0586-449c-ac93-a6eb40d4940f/image.png)
 

- root로 로그인을 하면 먼저 /etc/profile을 읽어들여서 적용하고, 그 다음 root의 홈 디렉토리 아래있는 root의 .bash_profile을 읽어들  인다. 

- root가 아닌 scott 계정으로 로그인 한다면 /etc/profile을 읽어들이고 scott 계정의 홈 디렉토리에 있는 .bash_profile을 읽어들인다. 
(/etc/profile이 수행된다음 바로 수행된다.)

- 환경설정은 profile에, 다른 함수나 alias 설정은 bashrc에 저장하도록 권장한다. (bash 쉘을 사용할 경우)


​
### 스크립트 실행 순서

![](https://images.velog.io/images/sandartchip/post/697aecd9-2513-4243-b1e7-f8a792fbfe89/%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8.png)
  ### 2.1 Login shell
   - Login shell은 사용자가 로그인 했을때 적용되는 shell을 의미한다. **로그인할 때만 적용됨!!!!**
   - 로그인은 계정과 암호를 입력해서 Shell을 실행하는 것을 말한다. 따라서 ssh로 접속하거나 로컬에서 GUI를 통해 Shell을 실행하는 것은 Login Shell 이다.
   - .profile, .bash_profile 이 두 파일은 Login할 때 로드되는 파일이다. 
   
   * .profile은 꼭 bash가 아니더라도 로그인하면 로드되며, '.bash_profile'은 꼭 bash로 로그인 할 때만 실행된다. (리눅스의 기본 쉘은 bash이다)


   ### 2.2 Non Login shell
    
   - Non-Login Shell은 로그인 없이 실행하는 Shell을 말한다. ssh로 접속하고 나서 다시 bash를 실행하는 경우나,    GUI 세션에서 터미널을 띄우는 것도 여기 해당한다. ('sudo bash'나  'su'같은 것도 해당)

   - ​.bashrc은 이미 로그인 한 상태에서, bash 에서 작업할 때마다 실행된다. 
   - 터미널에 python을 타이핑하면 python3과 연결되는 (non-login shell에서 실행)
   - bashrc에는 alias, profile에는 환경변수 설정

   * 만약 새 터미널 창을 열 때마다 .bashrc를 로드하고 싶다면 .bash_profile 에서 .bashrc를 로드하면 된다.


### 3. 환경 설정 스크립트 작성및 적용 방법

  source 명령어는 스크립트나 설정 파일들을 수정한 후에 수정된 값이 바로 적용되도록 사용하는 명령어 이다.

 - 사용 방법 

   #source [file_name]



 - 사용 예)  

   [root@localhost] vi .bash_profile

   수정전 : PATH=$PATH:$HOME/bin 

   수정후 : PATH=$PATH:$HOME/bin:/usr/sbin  

   vi로 편집한 .bash_profile 저장후 빠져나오기


  [root@localhost] source .bash_profile

  [root@localhost] echo $PATH

   /usr/local/bin:/usr/local/sbin:/usr/bin:/usr/sbin:/bin:/sbin:/home/parallels/.local/bin:/home:/var:/root/bin:/usr/sbin

​

### 4. 사용자 환경 변수 설정

- .bashrc 이나 .bash_profile 파일 하단에 'export [변수명]=[환경변수값]' 명령줄을 추가한 후 source [파일 이름] 명령어로 설정.

- 삭제할 때는 역으로 'export [변수명]=[환경변수값]' 명령줄을 삭제한 후 source [파일 이름] 명령어를 입력해서 설정.



### 5. 시스템 환경 변수 설정

/etc/bash.bashrc 이나 /etc/profile 파일 하단에 'export [변수명]=[환경변수값]' 명령줄을 추가한 후 source [파일 이름] 명령어로  설정.

 

참고 자료 : http://blog.naver.com/PostView.nhn?blogId=sdrock&logNo=221508545540 
