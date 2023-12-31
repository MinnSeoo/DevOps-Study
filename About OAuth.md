# **About OAuth**

### **Why ? ? ?**

---

왜 갑자기 OAuth에 대하여 정리하게 되었을까 ? 

→ msg에서 가성선배가 내 주신 과제에서 GAuth의 아키텍처를 짜 보는 과제가 있었다.

GAuth의 아키텍처를 무작정 무지성으로 짜는 것은 매우 비효율적이고 시간낭비라고 생각했고,

이미 만들어져있는 프로토콜의 아키텍처를 참고해서 짜 보는것이 더 낫겠다고 생각해서 OAuth라는 프로토콜을 참고하게 되었고, 블로그나 공식문서들을 그냥 눈으로 보는것 보다는 똑같은 내용을 정리하더라도 직접 적어보는 것이 더 좋을것 같아서 이렇게 정리하게 되었다.  

<br>

## **OAuth란 ? ? ?**

인증을 위한 표준 프로토콜의 한 종류이며, 보안 된 리소스에 액세스하기 위해서 client에게 권한을 제공하는 프로세스를 단순화하는 프로토콜 중 한 방법이다.

웹이나 앱에서 흔히 사용하는 소셜 로그인 인증 방식은 OAuth 라는 기술을 바탕으로 구현된다.

OAuth는 인증을 중간에서 중개 해 주는 역할로, 이미 사용자의 정보를 가지고 있는 웹 서비스(facebook, google, naver ..등)에서 인증을 대신해주고, 접근 권한에 대한 토큰을 발급한 후, 이 토큰을 이용하여 내 서버에서 인증이 가능하다.

<br>

## **OAuth를 사용하는 이유 ? ? ?**

user 입장에서는 OAuth를 활용하여 자주 사용하는 중요한 서비스들의 ID와 PW를 기억만 해 두고 해당 서비스들을 통해 소셜 로그인을 할 수 있기 때문에 보다 Seamless한 경험을 느낄 수 있다.

그리고 OAuth를 활용하여 검증되지 않은 app에 로그인한다면, 사용자의 중요한 정보가 app에 직접적으로 노출될 일이 없고 user 및 토큰 … 등 user에게 사전에 미리 인증권한을 받아야하기 때문에 더 안전하게 사용할 수 있다.