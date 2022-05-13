# TBS-HOL

VMware Tanzu Build Service (TBS) - Hands-On-Labs
<img width="830" alt="image" src="https://user-images.githubusercontent.com/14763080/168219454-255a35d5-b477-42b6-9f6f-c6ea25157951.png">


## Introduction
본 핸즈온 문서는 VMWare Tanzu Build Service 에 대한 전반적인 내용과 실습을 위한 가이드 문서입니다. <br/>
본 과정에서는 Tanzu Build Service의 제품에 대한 소개와 실습을 포함하고 있습니다.<br/>
본 문서는 Tanzu Build Service 1.5 버전 기준입니다.
<br/>
<br/>

## Objective
1. Tanzu Build Service에 대해 알아봅니다<br/>
2. Tanzu Build Service 환경을 세팅합니다<br/>
3. (필수) 이미지를 build/rebuild 합니다<br/>
4. (선택) dependency를 관리합니다<br/>
5. (선택) CI/CD와 연동합니다<br>


## Prerequisites
- 쿠버네티스 클러스터 v1.19 이상
- 모든 워커 노드에 50GB 이상의 임시 볼륨(ephemeral storage) 할당
- Carvel Imgpkg v0.12.0 이상
- Tanzu CLI : Tanzu Network에서 다운로드 가능하며, Secret과 Package plugin 필요
- Carvel kapp controller, secret gen controller
- kp CLI v0.5 이상 (https://network.tanzu.vmware.com/products/build-service/)
- Docker CLI
- Image Registry (Docker, Harbor 등)


## Tanzu Build Service란?
[What is TBS?](./Introduction/) <br/>

## Hands-On-Labs 순서
1. [Tanzu Build Service 설치](./Install/) <br/>
1. [이미지 Build/Rebuild](./Image_Mgmt/) <br/>
1. [Dependency 관리]<br/>
1. [CI/CD 연동]<br/>

