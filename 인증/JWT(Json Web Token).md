# JWT(Json Web Token)
Jwt는 Json Web Token의 약자로 Json으로 된 Token을 사용하는 방식이다.  
Jwt는 인증에 필요한 정보들을 암호화 시킨 토큰을 뜻한다.  
사용자의 세션 상태를 저장하는게 아니라 필요한 정보를 토큰 body에 저장하여 사용자가 가지고 있고 그것을 증명처럼 사용한다.  
사용자는 Access Token(Jwt Token)을 HTTP헤더에 실어 서버에 보낸다.  

그런데 Jwt는 **토큰 자체가 의미를 갖는 claim 기반의 토큰** 방식이다.  
**claim(권한)** 은 사용자에 대한 프로퍼티나 속성을 의미한다.  
즉, Jwt는 아무 의미없는 문자열로 된 토큰이 아니다.

Token을 생성하고 요청하는 프로세스는 다음과 같다.
- Json 객체의 요구사항을 작성
- 어떠한 암호화 방식을 사용해서 문자열 인코딩
- HTTP header에 추가함으로써 사용자 인증을 요청
- 서버에서는 header에 추가된 Token을 디코딩하여 사용자를 인증함
## Jwt의 특징
1. 정보가 담긴 데이터(Json 객체)를 암호화 하여, HTTP 헤더에 추가시킨다.
    - 이로 인하여 보안성을 증가시킴
2. 권한을 부여하기 위해 필요한 데이터가 Jwt안에 모두 포함되어 있다.
    - 인증 서버에서 토큰에 대한 정보를 찾을 필요가 없다.

그렇다고 Jwt가 완벽한 것은 아니다.  
누군가 토큰을 탈취한다면, 그 토큰을 이용하여 권한을 수행할 수 있다.  
그래서 토큰을 서버에 저장하는 것이 아니기 때문에 **토큰 유효시간**을 설정해야 하며, 탈취 될 가능성을 줄이기 위해 **유효시간을 짧게 해주는 것**이 좋다.
## Jwt의 데이터 무결성 - HMAC
Jwt는 토큰이 탈취당하더라도 위변조의 위험을 벗어날 수 있도록 무결성을 보장하는 방법이 몇 가지 있다.  
그 방법 중 하나가 데이터를 암호화하고, 해싱하는 **HMAC**(Hash-based Message Authentication) 기법을 사용하는 것이다.  
**해싱**은 원문을 일정 길이의 byte로 변환하는데 그 결과가 유일하다는 특징이 있다.  
즉, 원문이 조금이라도 변하면 해싱의 결과는 완전히 달라진다.  
그래서 토큰을 탈취해서 수정하게 되면 해싱의 결과가 완전히 달라지므로 토큰이 위조되었다는걸 알 수 있다.
## Jwt의 구조 및 생성
토큰을 만들기 위해서는 Header(헤더), Payload(내용), Verify Signature(서명)가 필요하다.
1. 헤더(header)
    - typ
        - 토큰 타입을 명시한다.
    - alg
        - 해싱 알고리즘을 명시한다.
        - 이 알고리즘은 서버에서 토큰을 검증할 때 signatue에서 사용된다.

    Jwt인 토큰 유형이나 HMAC SHA256 또는 RSA와 같이 사용되는 해시 알고리즘이 무엇으로 사용했는지 등 정보가 담긴다.
2. 내용(payload)  
클라이언트에 대한 정보, META data같은 내용이 들어가 있고, Base64로 인코딩 되어 있다.  
토큰 정보는 속성, 값으로 표현되며 이를 claim이라 한다.
claim은 다음과 같이 3가지로 작성이 가능하다.  
    1. registered claim  
        미리 정의된 claim으로써, 토큰에 대한 정보를 작성한다.
        - iss
            - 토큰 발급자
        - exp
            - 토큰의 만료 시간
        - sub
            - 토큰 제목
        - 그 밖에 여러가지 존재
    2. public claim
        - 공개적인 claim을 명시하는데, 충돌방지를 위한 URI 형식으로 작성한다.
    3. private claim
        - 서버와 클라이언트가 협의한 claim을 명시한다.
3. 서명(verify Signature)  
    - Base64 방식으로 인코딩한 header, payload 그리고 secret key를 더한 후 서명된다.
    - 서명은 "."으로 연결하여 합친 후 비밀키로 해싱하는 것을 말한다.  

최종적인 형태는 Encoded Header + "." + Encoded Payload + "." + Verify Signatue 형태이다.(xxxx.yyyy.zzzz)
![](https://t1.daumcdn.net/cfile/tistory/99329E345B53368603)
header와 payload는 base64로 인코딩만 되므로 누구나 디코딩하여 확인할 수 있다.  
따라서 payload에는 중요한 정보가 포함되면 안된다.  
하지만 Verify Signatue는 SECRET KEY를 알지 못하면 복호화할 수 없다.  
(ex. 사용자 A가 B의 데이터를 보고자 payload를 조작하려 한다. 그래서 payload에 있던 A의 ID를 B의 아이디로 바꿔서 다시 인코딩 후 토큰을 서버로 보낸다. 그러면 서버는 처음에 암호화된 Verify Signature를 검사하게 된다. 여기서 payload는 B사용자의 정보가 들어가 있으나 Verify Signatur는 A의 payload를 기반으로 암호화되었기 때문에 유효하지 않는 토큰으로 간주한다. 여기서 A 사용자는 SECRET KEY를 알지 못하는 이상 토큰을 조작할 수 없다는 것을 확인할 수 있다.)
## Jwt의 인증방식
![](https://t1.daumcdn.net/cfile/tistory/995EC2345B53368912)
1. 클라이언트가 로그인을 위해 유저 정보를 서버로 전달
2. 서버에서는 전달된 데이터로 사용자를 식별한 후, 사용자의 고유한 ID값을 부여한 후, 기타 정보와 함께 payload에 추가
3. Jwt 토큰의 유효기간을 설정
4. 암호화 할 SECRET KEY를 이용해 Access Token을 발급
5. 사용자는 Access Token을 받아 저장한 후, 인증이 필요한 요청마다 토큰을 헤더에 추가하여 전달
6. 서버에서는 해당 토큰의 Verify Signatue를 SECRET KEY로 복호화한 후, 조작 여부, 유효기간을 확인
7. 해당 토큰이 유효하면 payload를 디코딩하여 사용자의 ID에 맞는 데이터 호출  

세션/쿠키 방식과 가장 큰 차이점은 세션/쿠키는 저장소에 유저 정보를 넣는 반면, Jwt는 토큰 안에 유저 정보를 넣는다는 점이다.  
사용자 입장에서는 헤더에 세션 ID나 토큰을 실어 보내준다는 점에서는 동일하나, 서버 측에서는 인증을 위해 암호화를 하냐, 별도의 저장소를 이용하냐는 차이가 발생한다.
## 장점
1. Jwt는 발급한 후 토큰 검증만 하면 되기에 추가 저장소가 필요 없음
2. 서버 확장, 유지보수 하는데 유리함
3. 토큰을 기반으로 하는 다른 인증 시스템에 접근 가능
    - Facebook 로그인, Google 로그인 등은 모두 토큰을 기반으로 인증하므로 선택적으로 이름이나 이메일 등을 받을 수 있음
## 단점
1. 세션/쿠키의 경우 세션ID가 변질되었다고 판단되면 지워버리면 되지만, Jwt는 한 번 발급하면 유효기간이 만료되기 전까지는 계속 사용이 가능하여 유효기간이 지나기 전까지 정보 탈취가 가능
    - 기존의 Access Token의 유효기간을 짧게 하고 Refresh Token이라는 새로운 토큰을 발급하면 Access Token을 탈취당하더라도 상대적으로 피해를 줄일 수 있음
2. payload 정보가 제한적/payload는 따로 암호화되지 않기 때문에 디코딩 하면 누구나 정보를 볼 수 있어서 담는 데이터가 제한적임
3. 세션/쿠키 방식에 비해 Jwt의 길이가 기므로 인증이 필요한 요청이 많아질수록  서버의 자원낭비가 발생
4. 유효기간을 짧게 하면 재로그인 시도가 잦아지고 길면 해커에게 탈취될 가능성이 큼(1번과 연관된 문제)
---
## Refresh Token
Access Token만 사용한 인증 방식의 문제는 만일 제 3자에게 탈취당할 경우 보안이 취약하다는 것이다.  
유효 기간이 짧은 Token의 경우, 사용자는 새로운 Token을 발급받기 위해 자주 인증을 시도해서 유효기간을 늘려야 하여 불편하다.  
반대로 유효기간을 늘리면, 토큰을 탈취당했을 때 보안에 더 취약해지게 된다.  
'유효기간을 짧게 하면서 보안을 챙길수는 없을까?'라는 질문에 나온 방식이 바로 **Refresh Token**이다.  

Refresh Token은 Access Token과 똑같은 형태의 Jwt이다.  
로그인이 완료됐을 때 Access Token과 동시에 발급되는 Refresh Token은 긴 유효시간을 가지면서, AccessToken의 유효기간이 만료되었을 때 새로운 Access Token을 발급해주는 열쇠가 된다.  
(ex. Refresh Token의 유효기간은 2주, Access Token의 유효기간을 1시간으로 설정한다면 사용자에게 발급된 Access Token이 1시간이 지나게 되면 Access Token은 만료 된다. 아직 Refresh Token의 유효기간이 남아있는 동안은 계속 Access Token을 새롭게 재발급이 가능하다.)  
Refresh Token이 만료되면 사용자는 새로 로그인  
## Access Token + Refresh Token 인증 과정
![](https://t1.daumcdn.net/cfile/tistory/99DB8C475B5CA1C936)
1. 클라이언트가 로그인을 위해 유저 정보를 서버로 전달
2. 서버에서는 전달된 데이터로 사용자를 식별한 후, 사용자의 고유한 ID값을 부여한 후, 기타 정보와 함께 payload에 추가
3. Jwt 토큰의 유효기간을 설정
4. 암호화할 SECRET KET를 이용해 Access Token, **Refresh Token**을 발급
    - 이때 일반적으로 회원 DB에 Refresh Token을 저장
5. 사용자는 Refresh Token을 안전한 저장소에 저장
6. 사용자는 Access Token을 받아 저장(쿠키)한 후, 인증이 필요한 요청마다 토큰을 헤더에 추가하여 전달
7. Access Token을 검증하여 이에 맞는 데이터를 전송
8. 시간이 지나서 Access Token이 만료됨
9. 사용자는 6번과 마찬가지로 헤더에 토큰을 추가하여 전달
10. 서버는 Access Token이 만료된 것을 확인
11. 사용자에게 만료 메시지를 반환
    - 사용자는 Access Token의 payload를 통해 유효기간을 알 수 있으므로 요청 이전에 바로 재발급 요청을 할 수 있음
12. 사용자는 Refresh Token과 Access Token을 헤더에 담아 서버에 요청
13. 서버는 요청받은 토큰의 Verify Signature를 SECRET KEY로 복호화한 후, 조작 여부, 유효기간을 확인하고 전달받은 Refresh Token과 저장해둔 Refresh Token을 비교, Token이 동일하고 유효기간이 지나지 않았다면 Access Token을 발급
14. 새로 발급받은 Access Token을 저장하여 인증이 필요한 요청마다 헤더에 추가하여 전달
## 장점
- Access Token만 있을 때보다 안전
## 단점
- 구현이 복잡
- Access Token이 만료될 때마다 새롭게 발급하는 과정에서 HTTP 요청이 잦아짐