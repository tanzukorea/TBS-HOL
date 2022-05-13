# 1. Tanzu Build Service 설치
## 1. 환경 변수 설정
설치에 필요한 환경 변수를 정의합니다.
'''
export INSTALL_REGISTRY_HOSTNAME=<IMAGE-REGISTRY>
export INSTALL_REPOSITORY=<IMAGE-REPOSITORY>
export INSTALL_REGISTRY_USERNAME=<REGISTRY-USERNAME>
export INSTALL_REGISTRY_PASSWORD=<REGISTRY-PASSWORD>
export TANZUNET_REGISTRY_USERNAME=<TANZUNET_REGISTRY_USERNAME>
export TANZUNET_REGISTRY_PASSWORD=<TANZUNET_REGISTRY_PASSWORD>
export TBS_VERSION=<LATEST-TBS-VERSION>
'''

- IMAGE-REGISTRY : 이미지 레지스트리의 호스트 이름입니다.
- IMAGE-REPOSITORY : 이미지 레지스트리의 레파지토리입니다.
    - Dockerhub : my-dockerhub-username/build-service 혹은 index.docker.io/my-dockerhub-username/build-service
    - gcr.io : gcr.io/my-project/build-service
    - Harbor: my-harbor.io/my-project/build-service
- REGISTRY_USERNAME, REGISTRY_PASSWORD : 위 이미지 레지스트리의 ID/비밀번호 입니다.
- TANZUNET_REGISTRY_USERNAME, TANZUNET_REGISTRY_PASSWORD : TanzuNet (https://network.tanzu.vmware.com/)의 ID/비밀번호 입니다.
- LATEST-TBS-VERSION : 설치하고자 하는 TBS 버전입니다. (예시 : 1.5.1)
<br/>

## 2. 프라이빗 레지스트리에 이미지 준비
Tanzu Network에 있는 이미지를 프라이빗 레지스트리로 옮기는 과정입니다. <br/>

**1) Tanzu Registry 로그인**
'''
docker login registry.tanzu.vmware.com
'''

**2) Private Registry 로그인**
'''
docker login ${INSTALL_REGISTRY_HOSTNAME}
'''

**3) Carvel Tool의 imgpkg를 사용한 이미지 복사**
'''
imgpkg copy -b registry.tanzu.vmware.com/build-service/package-repo:$TBS_VERSION --to-repo=${INSTALL_REPOSITORY}
'''

(예시)
'''
imgpkg copy -b registry.tanzu.vmware.com/build-service/package-repo:1.5.1 --to-repo=index.docker.io/bnewkk/tbs
'''

<br/>

## 3. 설치
**1) namespace 생성**
'''
kubectl create ns tbs-install
'''

**2) tanzu secret 생성**
'''
tanzu secret registry add tbs-install-registry \
  --username ${INSTALL_REGISTRY_USERNAME} --password ${INSTALL_REGISTRY_PASSWORD} \
  --server ${INSTALL_REGISTRY_HOSTNAME} \
  --export-to-all-namespaces --yes --namespace tbs-install
'''

**3) Tanzu Build Service 패키지 레파지토리 추가**
'''
tanzu package repository add tbs-repository \
    --url "${INSTALL_REPOSITORY}:${TBS_VERSION}" \
    --namespace tbs-install
'''

**4) 패키지 레파지토리 설치 확인**
'''
tanzu package repository get tbs-repository --namespace tbs-install
'''

**5) kp secret 생성**
레파지토리에 대한 kp 시크릿을 생성하는 과정입니다.
'''
 kp secret create kp-default-repository-creds \
   --registry "${INSTALL_REGISTRY_HOSTNAME}" \
   --registry-user "${INSTALL_REGISTRY_USERNAME}" \
   --namespace tbs-install
'''

주의) docker hub에서 시크릿 생성에 문제가 있는 경우, 다음 커맨드를 사용합니다.
'''
kp secret create kp-default-repository-creds --dockerhub ${INSTALL_REGISTRY_USERNAME}
'''
이후 비밀번호를 입력합니다.


**6) tbs-values.yml 파일 생성**

- plain text로 파일 구성
    '''
    ---
    kp_default_repository: <INSTALL_REPOSITORY>
    kp_default_repository_username: <INSTALL_REGISTRY_USERNAME>
    kp_default_repository_password: <INSTALL_REGISTRY_PASSWORD>
    pull_from_kp_default_repo: true
    tanzunet_username: <TANZUNET_REGISTRY_USERNAME>
    tanzunet_password: <TANZUNET_REGISTRY_PASSWORD>
    descriptor_name: <DESCRIPTOR_NAME>
    enable_automatic_dependency_updates: true
    ca_cert_data: <CA_CERT_CONTENTS> (optional)
    '''
- secret으로 파일 구성
    보안 위험성이 있는 경우, id/password를 직접 yaml에 삽입하는 대신 secret을 사용합니다.
    '''
    ---
    kp_default_repository: <INSTALL_REPOSITORY>
    kp_default_repository_secret:
    name: kp-default-repository-creds
    namespace: tbs-install
    pull_from_kp_default_repo: true
    tanzunet_secret:
    name: tanzunet-creds
    namespace: tbs-install
    descriptor_name: <DESCRIPTOR_NAME>
    enable_automatic_dependency_updates: true
    ca_cert_data: <CA_CERT_CONTENTS> (optional)
    '''

-  추가 설명
    - DESCRIPTOR_NAME 옵션이 full일 경우, 모든 dependency를 포함 (production 환경). lite일 경우, 클러스터의 인터넷 접근이 필요함
    - CA_CERT_CONTENTS : 레지스트리가 사설 인증서로 구성되었을 경우 필요. .PEM 파일이 필요합니다.

-  기타 설치 옵션
    - 추가적인 value schema는 아래 명령어 실행을 통해 조회 가능하며, tbs-values.yml 에 추가합니다.
    '''
    tanzu package available get buildservice.tanzu.vmware.com/$TBS_VERSION --values-schema --namespace tbs-install
    '''

tbs-values.yml 파일을 저장합니다.


**7) 패키지 설치**
최종적으로 패키지를 설치합니다.
'''
tanzu package install tbs -p buildservice.tanzu.vmware.com -v $TBS_VERSION -n tbs-install -f tbs-values.yml --poll-timeout 30m
'''

설치가 완료되면 다음과 같이 확인이 가능합니다.
![](../Images/reconcile.png)

<br/>


본 실습을 성공적으로 마치셨습니다.

<br/><br/>

# 2. Air-Gapped 환경에서 Tanzu Build Service 설치
To Be Updated



본 실습을 성공적으로 마치셨습니다.