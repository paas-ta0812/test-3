# PaaS-TA\_GCP\_환경설정\_가이드v1.0

### Table of Contents

1. [개요](paas-ta_gcp_-_-v1.0.md#1)
   * [목적](paas-ta_gcp_-_-v1.0.md#2)
   * [범위](paas-ta_gcp_-_-v1.0.md#3)
   * [참고자료](paas-ta_gcp_-_-v1.0.md#4)
2. [Google Cloud Platform 기본 환경 설정](paas-ta_gcp_-_-v1.0.md#5)
   * [Google Cloud Platform 기본 환경 설정 절차](paas-ta_gcp_-_-v1.0.md#6)
     * [Google Cloud Platform 프로젝트 생성](paas-ta_gcp_-_-v1.0.md#7)
     * [Google Cloud Platform API Enable 설정](paas-ta_gcp_-_-v1.0.md#8)
     * [Google Cloud Platform Service Account 생성](paas-ta_gcp_-_-v1.0.md#9)
     * [Google Cloud Platform Service Account Key 생성](paas-ta_gcp_-_-v1.0.md#10)
     * [Google Cloud Platform VPC networks 생성](paas-ta_gcp_-_-v1.0.md#11)
     * [Google Cloud Platform External IP Address 생성](paas-ta_gcp_-_-v1.0.md#12)
     * [Google Cloud Platform Firewall rules 설정](paas-ta_gcp_-_-v1.0.md#13)
     * [Google Cloud Platform Key pair 생성](paas-ta_gcp_-_-v1.0.md#14)
     * [Google Cloud Platform Meta Data 생성](paas-ta_gcp_-_-v1.0.md#15)

### Executive Summary

본 문서는 PaaS-TA가 지원하는 IaaS 환경 중 Google에 대한 BOSH 설치 환경 설정하는 방법에 대해서 설명하였다.

## 1.  문서 개요

### 1.1.  목적

본 문서에서는 Inception 서버 환경에 BOSH를 설치하여 기능을 테스트할 수 있는 환경을 구축 시 Google Cloud Platform \(이하 GCP\) 기본 환경 설정을 하는 데 목적이 있다.

### 1.2.  범위

본 가이드에서는 Linux 환경\(Ubuntu 14.04 TRUSTY\) 을 설치환경 구성 기준으로 작성 하였으며, 전제 조건으로는 Google Cloud Platform 계정이 있어야 한다.

### 1.3.  범위

본 문서는 Cloud Foundry의 BOSH Document와 Google Cloud Platform Document를 참고로 작성하였다.

BOSH Document: [http://bosh.io](http://bosh.io)

GCP Document: [https://cloud.google.com/docs/](https://cloud.google.com/docs/)

## 2.  Google Cloud Platform 환경 설정

BOSH는 클라우드 환경에 서비스를 배포 관리하는 소프트웨어로 BOSH 자체도 클라우드에 배포 되어야 하는 서비스이다. 본 문서에서는 BOSH를 Google Cloud Platform 환경에서 배포 시 필요한 기본 환경 설정에 목적이 있다.

### 2.1. Google Cloud Platform 기본 환경 설정 절차

Google Cloud Platform 기본 환경 설정 절차는 다음과 같다.

* Google Cloud Platform 프로젝트 생성 
* Google Cloud Platform API Enable 설정 
* Google Cloud Platform Service Account 생성 및 권한 관리
* Google Cloud Platform Service Account Key 생성
* Google Cloud Platform VPC networks생성
* Google Cloud Platform External IP Address생성
* Google Cloud Platform Firewall Rules 설정 
* Google Cloud Platform KeyPair 생성
* Google Cloud Platform Meta Data생성

#### 2.1.1. Google Cloud Platform 프로젝트 생성

위에서 범위에서 언급 하였듯이 사전에 Google Cloud Platform 계정이 존해 해야 한다.

1.[https://console.cloud.google.com](https://console.cloud.google.com) GCP 콘솔 접속

![](../../../.gitbook/assets/1%20%282%29.png)

2.추가 버튼을 눌러 프로젝트를 생성한다.

![](../../../.gitbook/assets/2%20%282%29.png)

#### 2.1.2. Google Cloud Platform API Enable 설정

* API를 통하여 Token 및 데이터를 가져오기 위해 API를 Enable 설정 한다.

  ```text
    Enable the GCE API for your project
    Enable the IAM API for your project
    Enable the Cloud Resource Manager API
  ```

1.Google Cloud Platform 콘솔 화면에서 API Manager메뉴를 선택한다. ![](../../../.gitbook/assets/3%20%282%29.png)

2.Library 메뉴를 선택한다. ![](../../../.gitbook/assets/4%20%282%29.png)

3.GCP API를 Enable 하기 위해 compute를 검색창에 쓰고, 검색 결과 목록에서 Google Compute Engine API \(GCE API\)를 클릭한다.

4.GCE API 상태를 Enable로 변경 한다. ![](../../../.gitbook/assets/5%20%282%29.png)

5.위와 같은 방법으로 IAM, Cloud Resource Manager API의 상태도 활성화 한다.

#### 2.1.3. Google Cloud Platform Service Account 생성

1.Google Cloud Platform IAM & ADMIN 메뉴의 하위 Service accounts 메뉴에서 Service Account를 생성 한다. ![](../../../.gitbook/assets/6%20%282%29.png)

![](../../../.gitbook/assets/7%20%282%29.png)

![](../../../.gitbook/assets/8%20%282%29.png)

2.Service Account 권한

* 보안 상의 이유로 Owner 권한을 주면 안될 경우 
  * Compute Instance Admin, Compute Network Admin, Compute Network User, 
  * Compute Storage Admin, Service Account Actor, Storage Admin 5가지 권한을 부여 하면 된다.

![](../../../.gitbook/assets/9%20%282%29.png)

#### 2.1.4. Google Cloud Platform Service Account Key 생성

1.Service Account에 대한 접근 가능 key를 JSON으로 생성 한다.

* Service Account 목록에서 해당 Service Account의 우측에 Create Key메뉴를 클릭하면 된다.

![](../../../.gitbook/assets/10%20%282%29.png)

![](../../../.gitbook/assets/11%20%282%29.png)

#### 2.1.5. Google Cloud Platform VPC networks 생성

1.Networking의 하위메뉴 VPC networks 메뉴를 선택 하고 CREATE VPC NETWORK 버튼을 클릭 한다.

![](../../../.gitbook/assets/12%20%282%29.png)

2.NETWORK 및 서브넷 정보를 입력 하고 Create 버튼을 클릭 한다.

![](../../../.gitbook/assets/13%20%282%29.png)

#### 2.1.6. Google Cloud Platform External IP Address 생성

1.Networking의 하위메뉴 External IP address 메뉴를 선택 하고 Reserve static address 버튼을 클릭 한다.

![](../../../.gitbook/assets/14%20%282%29.png)

2.External IP address 정보를 입력하고 Reserve 버튼을 클릭 한다.

![](../../../.gitbook/assets/15%20%282%29.png)

#### 2.1.7. Google Cloud Platform External IP Address 생성

1.Networking의 하위메뉴 Firewall rules 메뉴를 선택 하고 CREATE FIREWALL RULE 버튼을 클릭 한다.

![](../../../.gitbook/assets/16%20%282%29.png)

2.firewall 아래 테이블의 rule 정보 입력 한다.

![](../../../.gitbook/assets/17%20%282%29.png)

3.firewall rule 정보를 입력하고 Create 버튼을 클릭 한다.

![](../../../.gitbook/assets/18%20%282%29.png)

1. 다음과 같이 firewall rules가 생성되는 것을 확인 할 수 있다

![](../../../.gitbook/assets/19%20%282%29.png)

#### 2.1.7. Google Cloud Platform Key pair 생성

1.Ubuntu Linux 명령어를 사용하여 Key Pair를 생성 한다.

```text
$ ssh-keygen -t rsa -f ~/.ssh/vcap -C vcap
```

#### 2.1.7. Google Cloud Platform ssh Meta Data 생성

1.Compute Engine 하위 메뉴 Metadata 메뉴를 클릭 하고 ssh Keys 탭 메뉴의 Add SSH keys를 선택 한다.

![](../../../.gitbook/assets/20%20%282%29.png)

![](../../../.gitbook/assets/21%20%282%29.png)

