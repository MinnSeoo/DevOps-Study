# **EC2에 spring code clone 받고 curl 보내기**


### **EC2 생성**
----
aws 콘솔창에서 ec2 서비스를 선택한 후, 인스턴스 시작 버튼을 눌러준다.

이름은 spring-test-ec2라고 지정하였다.

그 다음으로 ami 를 선택하는 부분이 나오는데, 여기서는 Amazon Linux ami를 선택하였고
인스턴스 유형으론 가장 기본적인 t2.micro를 선택하였다.

그 다음으론 키페어를 지정해 주는 부분이 나오는데, 과제에서 EC2를 생성후 spring 프로젝트와 잘 통신되는지 확인하려면 ssh로 접속해야 하기 때문에 키페어를 지정해 준다. (없을 경우 새로 생성해 준다.)

그 다음으론 네트워크를 설정하는 부분이 나오는데, 이번 과제에서는 vpc는 default를 사용하고, subnet은
default subnet 중에서 가용 영역이 2a인 subnet을 사용하고, 보안 그룹은 새로 생성해 준다. (이때 sg 규칙은 ssh에 접속하기 위해 22번 포트를 all trafic으로, 웹 서버와 통신하기 위해 80번 port를 all trafic으로 열어준다. 그리고 spring은 기본적으로 서버 포트가 8080으로 되어있기 때문에 사용자 지정 TCP에 포트 번호를 8080으로 지정해 준다.)

그리고 elastic ip를 생성하여 ec2에 연결시켜 서버를 중지 후 재 실행 했을때 ip 주소가 변경되지 않도록 한다.

정상적으로 EC2를 생성 했다면 아래와 같은 화면이 뜨게 된다.

<img width="1160" alt="스크린샷 2023-07-11 오후 8 53 19" src="https://github.com/MinnSeoo/DevOps-Study/assets/102645965/4618ada5-0ad7-4472-9ff0-535bd5088980">

<br>

### **ssh 접속**
---
EC2를 정상적으로 생성했으면 이제 인스턴스에 직접 접속할 차례이다.

터미널에서 pem키가 있는 경로로 이동후 위에서 생성한 EC2 인스턴스의 public ip 주소를 복사 후 터미널에 붙어넣어주면 EC2에 접속 완료 된 것이다.

<br>

### **설치해야 할 것**
---
과제에서 spring 프로젝트를 clone 받아오라고 했기 때문에 git과, spring 을 다운로드 하겠다.
git → `sudo yum install git`

spring → `sudo yum install spring` 

그리고 가장 중요한 iptables을 설치해야 한다.

iptables → `sudo yum install iptables` 

iptables를 설치하는 이유는 아까 보안그룹 포트 설정할때 80포트를 8080으로 포워딩 하도록 했으니 이제 리다이렉트 설정을 해야 하기 때문이다.

iptables를 설치하고 루트 권한으로 접속해 주고, 아래 명령어를 사용하여 포트 리다이렉트를 설정해 준다.

`iptables -A PREROUTING -t nat -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 8080`  

이렇게 하면 포트 리다이렉트 설정 완료!

<br>

### **Git Clone 후 curl 보내기**
---
과제 git에서 spring 프로젝트를 클론 받아오라고 했으므로 spring 프로젝트 레포에 가서 clone 주소를 복분해 준다.  

`git clone 클론받을 레포 주소` 

(이때 mkdir 명령어를 사용해서 폴더를 하나 생성해 주고, 생성한 폴더로 경로를 지정하고 클론을 받아와야한다.)

<br>

그 다음 클론 받아온 폴더를 안의 목록들을 출력해 보면 아래와 같은 폴더들이 존재한다.


<img width="772" alt="스크린샷 2023-07-12 오전 11 18 40" src="https://github.com/MinnSeoo/CI-CD-Test/assets/102645965/7cf086a4-564a-4965-8ed9-7873c0db3dde">


이 중에서 gradlew 폴더에 실행 권한을 추가해 준다. → `chmod +x gradlew`

<br>

그 다음 gradlew 를 build를 해 준다. → `sudo ./gradlew build` 

build를 하면 자동으로 `.jar` 파일이 생성될 것이다.

그럼 이제 경로를 `build/libs` 이동하여 jar 파일이 있는지 확인한다.

<img width="977" alt="스크린샷 2023-07-12 오전 11 26 34" src="https://github.com/MinnSeoo/CI-CD-Test/assets/102645965/1e054bbd-b6a3-4dfe-96ff-978e4f5a7fe4">


<br>

그리고 `java -jar build할 jar파일.jar` 명령어를 사용해서 jar파일을 빌드 해 준다.

<img width="1728" alt="스크린샷 2023-07-12 오전 11 31 15" src="https://github.com/MinnSeoo/CI-CD-Test/assets/102645965/c191d4b8-bce9-42f0-bf0d-f2b344bcfce8">


정상적으로 빌드가 되었다면 위와 같은 화면이 뜨면서 build가 완료된다.

<br>

빌드가 완료되었으면 터미널 창을 1개 더 열어서 [localhost](http://localhost) 8080으로 curl을 보내고 응답을 확인한다.

**[터미널 화면]**

![image](https://github.com/MinnSeoo/CI-CD-Test/assets/102645965/15fb2784-f413-4bf9-a6c7-e22da922053b)
