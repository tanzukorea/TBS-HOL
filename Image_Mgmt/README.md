이번 Lab에서는 샘플 애플리케이션을 사용해 이미지를 Build 및 re-build 해 보겠습니다.  <br/>
예시에서는 Spring Boot 기반의 애플리케이션 및 레지스트리로 docker hub를 사용합니다. <br/><br/>

## 1. 이미지 build
**1) Secret 생성** 
<br/> 다음 커맨드로 계정에 대한 시크릿을 생성합니다.
```
kp secret create kp-default-repository-creds --dockerhub ${INSTALL_REGISTRY_USERNAME}
```

**2) 이미지 생성**
<br/>아래와 같은 커맨드로 이미지를 생성합니다. 
- tag 옵션 : 본인의 레파지토리
- git 옵션 : 소스코드를 가져올 git 주소
- git-revision 옵션 : git branch
<br/> 옵션 확인이 필요할 경우,  kp image create -h 로 확인합니다.

```
kp image create spring-petclinic --tag index.docker.io/$DH_USERNAME/spring-petclinic --git https://github.com/$GH_USERNAME/spring-petclinic.git --git-revision main
```
<br/>

**3) 이미지 생성 과정 조회**
kp image list 입력해 생성한 이미지를 확인합니다. <br/>
![](../Images/petclinic-0.png)

kp build logs spring-petclinic 을 입력해 이미지 빌드 로그를 조회합니다. <br/>
![](../Images/petclinic-1.png)

이미지 생성 과정은 prepare -> analyze -> detect -> restore -> build -> export -> completion순서입니다. <br/>
Completion 후 Build successful 메시지를 확인합니다.

<br/>

**4) Docker Hub에서 이미지 확인**
Docker Hub에서 이미지를 확인합니다.
![](../Images/docker.png)

<br/><br/>

## 2. 코드 수정 후 이미지 Re-build
To Be Updated



본 실습을 성공적으로 마치셨습니다.