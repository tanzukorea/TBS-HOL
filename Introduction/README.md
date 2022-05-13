## 1. Tanzu Build Service란?
Tanzu Build Service는 Cloud Native Buildpack을 사용하는 Kubernetes 기반 자동화 컨테이너 빌드 및 관리 시스템입니다.  

<br/>

## 2.Tanzu Build Service 구성 요소
- Cloud Native Buildpack
    - 소스 코드를 OCI 호환 가능한 이미지로 변환합니다 
- kpack
    - k8s 기본 빌드 서비스로 함께 작동하는 오픈소스 리소스 컨트롤러의 모음입니다.
    - 이미지를 빌드하고, dependency 변경시 이미지 Re-build를 예약합니다. 

<br/>

## 3. Tanzu Build Service 특징
**1) 용이한 시작 및 확장**
- TBS가 자동으로 소스코드를 분석하고, 언어를 가마지해 배포 가능한 container artifact를 빌드합니다.
- 업데이트 : 코드 커밋, 런타임 종속성, 라이브러리 및 OS를 업데이트합니다.
- 자동으로 컨테이너 이미지를 빌드합니다.
- Java, Nodejs, .NET core, Python, Go, PHP 등 다양한 언어가 지원됩니다.

**2) Cloud Native Buildpack을 통한 편리한 관리**
- 빌드가 편리하며, 개발자는 개발에만 집중할 수 있습니다.

**3) Day2 운영**
- BoM을 통한 편리한 정책 관리
- Pipeline에서 신뢰할 수 있는 소프트웨어만을 가져올 수 있습니다.

**4) 보안 및 규정 준수**
- CVE 탐지 & 패치
- signed container image가 지원됩니다.
- air gap 환경 설치가 지원됩니다

**5) 모든 레지스트리, 모든 Kubernetes 플랫폼**
- 빌드한 이미지는 OCI와 호환되므로, 모든 OCI 호환 레지스트리에 푸시 가능합니다.
- 빌드한 컨테이너 이미지는 모든 k8s 플랫폼에서 실행 가능합니다.
- 기존 사용중인 CI/CD 툴과 연동이 가능합니다.