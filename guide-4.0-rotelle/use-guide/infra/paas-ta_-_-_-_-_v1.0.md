# PaaS-TA\_인프라\_관리\_대시보드\_사용가이드\_v1.0

### Table of Contents

1. [개요](paas-ta_-_-_-_-_v1.0.md#1)
   * [목적](paas-ta_-_-_-_-_v1.0.md#2)
   * [범위](paas-ta_-_-_-_-_v1.0.md#3)
2. [인프라 관리 대시보드 메뉴얼](paas-ta_-_-_-_-_v1.0.md#4)
   * [인프라 관리 대시보드 화면 설명](paas-ta_-_-_-_-_v1.0.md#5)
   * [계정 관리 -&gt; 전체 인프라 계정 관리](paas-ta_-_-_-_-_v1.0.md#6)
   * [계정 관리 -&gt; AWS 계정 관리](paas-ta_-_-_-_-_v1.0.md#7)
   * [계정 관리 -&gt; Openstack 계정 관리](paas-ta_-_-_-_-_v1.0.md#8)
   * [계정 관리 -&gt; Google 계정 관리](paas-ta_-_-_-_-_v1.0.md#9)
   * [계정 관리 -&gt; vSphere 계정 관리](paas-ta_-_-_-_-_v1.0.md#10)
   * [대시보드 -&gt; 인프라 전체 리소스 사용량 조회](paas-ta_-_-_-_-_v1.0.md#11)
   * [대시보드 -&gt; AWS 리소스 사용량 조회](paas-ta_-_-_-_-_v1.0.md#12)
   * [대시보드 -&gt; Openstack 리소스 사용량 조회](paas-ta_-_-_-_-_v1.0.md#13)
   * [인프라 관리 -&gt; AWS 관리](paas-ta_-_-_-_-_v1.0.md#14)
   * [인프라 관리 -&gt; AWS 관리 -&gt; VPC 관리](paas-ta_-_-_-_-_v1.0.md#15)
   * [인프라 관리 -&gt; AWS 관리 -&gt; Subnet 관리](paas-ta_-_-_-_-_v1.0.md#16)
   * [인프라 관리 -&gt; AWS 관리 -&gt; Internet Gateway 관리](paas-ta_-_-_-_-_v1.0.md#17)
   * [인프라 관리 -&gt; AWS 관리 -&gt; Security Group 관리](paas-ta_-_-_-_-_v1.0.md#18)
   * [인프라 관리 -&gt; AWS 관리 -&gt; Elastic IPs 관리](paas-ta_-_-_-_-_v1.0.md#19)
   * [인프라 관리 -&gt; AWS 관리 -&gt; Key Pair 관리](paas-ta_-_-_-_-_v1.0.md#20)
   * [인프라 관리 -&gt; Openstack 관리](paas-ta_-_-_-_-_v1.0.md#21)
   * [인프라 관리 -&gt; Openstack 관리 -&gt; Network 관리](paas-ta_-_-_-_-_v1.0.md#22)
   * [인프라 관리 -&gt; Openstack 관리 -&gt; Router 관리](paas-ta_-_-_-_-_v1.0.md#23)
   * [인프라 관리 -&gt; Openstack 관리 -&gt; Security Group 관리](paas-ta_-_-_-_-_v1.0.md#24)
   * [인프라 관리 -&gt; Openstack 관리 -&gt; Floating IPs 관리](paas-ta_-_-_-_-_v1.0.md#25)
   * [인프라 관리 -&gt; Openstack 관리 -&gt; Key Pair 관리](paas-ta_-_-_-_-_v1.0.md#26)
3. [인프라 관리 대시보드 활용](paas-ta_-_-_-_-_v1.0.md#27)
   * [AWS 환경 설정 절차](paas-ta_-_-_-_-_v1.0.md#28)
   * [Openstack 환경 설정 절차](paas-ta_-_-_-_-_v1.0.md#29)
   * [계정 관리](paas-ta_-_-_-_-_v1.0.md#30)
   * [AWS 계정 관리](paas-ta_-_-_-_-_v1.0.md#31)
   * [Openstack 계정 관리](paas-ta_-_-_-_-_v1.0.md#32)
   * [Google 계정 관리](paas-ta_-_-_-_-_v1.0.md#33)
   * [vSphere 계정 관리](paas-ta_-_-_-_-_v1.0.md#34)
   * [AWS 관리](paas-ta_-_-_-_-_v1.0.md#35)
   * [AWS VPC 관리](paas-ta_-_-_-_-_v1.0.md#36)
   * [AWS Subnet 관리](paas-ta_-_-_-_-_v1.0.md#37)
   * [AWS Internet Gateway 관리](paas-ta_-_-_-_-_v1.0.md#38)
   * [AWS Security Group 관리](paas-ta_-_-_-_-_v1.0.md#39)
   * [AWS Elastic IP 관리](paas-ta_-_-_-_-_v1.0.md#40)
   * [AWS Key Pair 관리](paas-ta_-_-_-_-_v1.0.md#41)
   * [Openstack 관리](paas-ta_-_-_-_-_v1.0.md#42)
   * [Openstack Network 관리](paas-ta_-_-_-_-_v1.0.md#43)
   * [Openstack Router 관리](paas-ta_-_-_-_-_v1.0.md#44)
   * [Openstack Security Group 관리](paas-ta_-_-_-_-_v1.0.md#45)
   * [Openstack Floating IP 관리](paas-ta_-_-_-_-_v1.0.md#46)
   * [Openstack Key Pair 관리](paas-ta_-_-_-_-_v1.0.md#47)

## 1.  문서 개요

### 1.1.  목적

본 문서는 인프라 관리 대시보드 시스템의 사용 절차에 대해 기술하였다.

### 1.2.  범위

본 문서에서는 Linux 환경\(Ubuntu 14.04\)을 기준으로 인프라 관리 대시보드를 사용하는 방법에 대해 작성되었다

## 2.  인프라 관리 대시보드  매뉴얼

인프라 관리 대시보드는 인프라 환경의 계정 등록 정보를 관리하는 계정 관리와 등록 한 인프라 계정의 리소스 사용량 조회 및 플랫폼 설치 자동화에서 PaaS-TA를 설치 하는 인프라의 리소스를 생성 할 수 있는 부분으로 구성 되어 있다.

| 대시보드 | 인프라 전체 리소스 사용량 조회 | 등록 한 인프라 계정의 전체 리소스 사용량을 조회하는 화면 |
| :--- | :--- | :--- |
|  |  |  |
| AWS 리소스 사용량 조회 | 등록 한 전체 AWS 계정의 인스턴스, 네트워크, 볼륨, 과금 정보를 조회하는 화면 |  |
| Openstack 리소스 사용량 조회 | 등록 한 전체 Openstack 계정의 인스턴스, 네트워크, 볼륨 정보를 조회하는 화면 |  |
| 계정 관리 | AWS 계정 관리 | 실제 AWS 계정을 등록/삭제 하는 화면 |
|  |  |  |
| Openstack 계정 관리 | 실제 Openstack 계정을 등록/수정/삭제 하는 화면 |  |
| Google 계정 관리 | 실제 Google 계정을 등록/수정/삭제 하는 화면 |  |
| vSphere 계정 관리 | 실제 vSphere 계정을 인프라 관리 대시보드에 등록/수정/삭제 하는 화면 |  |
| 인프라 관리 | AWS 관리\_VPC 관리 | 기본 AWS 계정에 VPC를 생성/삭제 하는 화면 |
| AWS 관리\_Security Group 관리 | 기본 AWS 계정에 Security Group와 Ingress Rule을 생성/삭제 하는 화면 |  |
| AWS 관리\_Key Pair 관리 | 기본 AWS 계정에 Key Pair를 생성 하는 화면 |  |
| AWS 관리\_Subnet 관리 | 기본 AWS 계정에 Subnet을 생성/삭제 하는 화면 |  |
| AWS 관리\_Internet Gateway 관리 | 기본 AWS 계정에 Internet Gateway를 생성/VPC 연결/연결해제/삭제 하는 화면 |  |
| AWS 관리\_Elastic IPs 관리 | 기본 AWS 계정에 Elastic IP를 할당 하는 화면 |  |
| Openstack 관리\_Network 관리 | 기본 Openstack 계정에 Network와 서브넷을 생성/삭제 하는 화면 |  |
| Openstack 관리\_Route 관리 | 기본 Openstack 계정에 Network와 서브넷을 생성/삭제 하는 화면 |  |
| Openstack 관리\_Key Pair 관리 | 기본 Openstack 계정에 Key Pair를 생성 하는 화면 |  |
| Openstack 관리\_Floating IPs 관리 | 기본 Openstack 계정에 Floating IP를 할당 하는 화면 |  |
| Openstack 관리\_Security Group 관리 | 기본 Openstack 계정에 Security Group0를 할당 하는 화면 |  |

### 2.1.  인프라 관리 대시보드 화면 설명

본 장에서는 플랫폼 설치 자동화를 구성하는 18개의 메뉴에 대한 설명을 기술한다.

### 2.2.    계정 관리 -&gt; 전체 인프라 계정 관리

“계정 관리” 화면은 등록 한 전체 인프라 계정 목록을 조회하고 각 각의 인프라 환경 별 계정 관리 화면으로 이동 할 수 있는 화면이다.

![](../../../.gitbook/assets/allInfraAccout%20%284%29.png)

#### 1. 전체 인프라 계정 목록

* 등록 한 전체 인프라 계정 정보 목록을 보여 준다.

#### 2. AWS 계정 관리 화면 이동

* AWS 계정 관리 화면으로 이동 하는기능을 수행 한다.

#### 3. Openstack 계정 관리 화면 이동

* Openstack 계정 관리 화면으로 이동 하는 기능을 수행 한다.

#### 4. Google 계정 관리 화면 이동

* Google 계정 관리 화면으로 이동 하는 기능을 수행 한다.

#### 5. vSphere 계정 관리 화면 이동

* vSphere 계정 관리 화면으로 이동 하는 기능을 수행 한다.

### 2.3.    계정 관리 -&gt; AWS 계정 관리

등록 한 AWS 계정을 조회 하고 AWS 계정을 등록 및 삭제 할 수 있는 화면이다.

![](../../../.gitbook/assets/awsAccount%20%283%29.png)

#### 1.    AWS 계정 목록

* 등록 한 AWS 계정 목록을 보여 준다.

  **2.    등록**

* 실제 AWS 계정을 인프라 관리 대시보드에 등록하는 기능을 수행 한다.

  **3.    삭제**

* 인프라 관리 대시보드에 등록 한 AWS 계정을 삭제하는 기능을 수행 한다.

  **4.    계정 관리 화면 이동**

* 선택 한 인프라 환경의 계정 관리 화면으로 이동 하는 기능을 수행 한다.

### 2.4.    계정 관리 -&gt; Openstack 계정 관리

등록 한 Openstack 계정을 조회 하고 Openstack 계정을 등록, 수정 및 삭제 할 수 있는 화면이다.

![](../../../.gitbook/assets/openstackAccount%20%283%29.png)

#### 1.    Openstack 계정 목록

* 등록 한 Openstack 계정 목록을 보여 준다.

  **2.    등록**

* 실제 Openstack 계정을 인프라 관리 대시보드에 등록하는 기능을 수행 한다.

  **3.    수정**

* 인프라 관리 대시보드에 등록 한 Openstack 계정을 수정하는 기능을 수행 한다.

  **4.    삭제**

* 인프라 관리 대시보드에 등록 한 Openstack 계정을 삭제하는 기능을 수행 한다.

  **5.    계정 관리 화면 이동**

* 선택 한 인프라 환경의 계정 관리 화면으로 이동 하는 기능을 수행 한다.

### 2.5.    계정 관리 -&gt; Google 계정 관리

등록 한 Google계정을 조회 하고 Google 계정을 등록, 수정 및 삭제 할 수 있는 화면이다.

![](../../../.gitbook/assets/googleAccount%20%284%29.png)

#### 1.    Google 계정 목록

* 등록 한 Google 계정 목록을 보여 준다.

  **2.    등록**

* 실제 Google 계정을 인프라 관리 대시보드에 등록하는 기능을 수행 한다.

  **3.    수정**

* 인프라 관리 대시보드에 등록 한 Google 계정을 수정하는 기능을 수행 한다.

  **4.    삭제**

* 인프라 관리 대시보드에 등록 한 Google 계정을 삭제하는 기능을 수행 한다.

  **5.    계정 관리 화면 이동**

* 선택 한 인프라 환경의 계정 관리 화면으로 이동 하는 기능을 수행 한다.

### 2.6.    계정 관리 -&gt; vSphere 계정 관리

등록 한 vSphere계정을 조회 하고 vSphere계정을 등록, 수정 및 삭제 할 수 있는 화면이다.

![](../../../.gitbook/assets/vsphereAccount%20%284%29.png)

#### 1.    vSphere 계정 목록

* 등록 한 vSphere 계정 목록을 보여 준다.

  **2.    등록**

* 실제 vSphere 계정을 인프라 관리 대시보드에 등록하는 기능을 수행 한다.

  **3.    수정**

* 인프라 관리 대시보드에 등록 한 vSphere 계정을 수정하는 기능을 수행 한다.

  **4.    삭제**

* 인프라 관리 대시보드에 등록 한 vSphere 계정을 삭제하는 기능을 수행 한다.

  **5.    계정 관리 화면 이동**

* 선택 한 인프라 환경의 계정 관리 화면으로 이동 하는 기능을 수행 한다.

### 2.7.    인프라 전체 리소스 사용량 조회

등록 한 모든 인프라 계정의 전체 리소스 사용량을 조회 할 수 있는 화면 이다.

![](../../../.gitbook/assets/allResource%20%284%29.png)

#### 1.    인프라 전체 리소스 사용량

* 등록 한 인프라 계정의 전체 리소스 사용량을 보여 준다.

  **2.    AWS 리소스 사용량**

* 등록 한 AWS 계정의 전체 리소스 사용량을 보여 주고 AWS 리소스 사용량 상세 조회 화면 이동 기능을 수행 한다.

  **3.    Openstack 리소스 사용량**

* 등록 한 Openstack 계정의 전체 리소스 사용량을 보여 주고 Openstack 리소스 사용량 상세 조회 화면 이동 기능을 수행 한다.

### 2.8.    AWS 리소스 사용량 조회

등록 한 AWS 계정의 전체 리소스 사용량을 조회 할 수 있는 화면 이다.

![](../../../.gitbook/assets/awsResource%20%284%29.png)

#### 1.    리소스 사용량 조회 화면 이동

* 선택 한 인프라 환경의 리소스 사용량 조회 화면으로 이동 하는 기능을 제공 한다.

  **2.    AWS Region 선택**

* AWS Region 목록을 보여 주고 선택 할 수 있는 기능을 제공 한다.

  **3.    AWS 인스턴스 사용량**

* 등록 한 전체 AWS 계정의 인스턴스 사용량을 보여 준다

  **4.    AWS 네트워크 사용량.**

* 등록 한 전체 AWS 계정의 네트워크 사용량을 보여 준다.

  **5.    AWS 볼륨 사용량**

* 등록 한 전체 AWS 계정의 볼륨 사용량을 보여 준다.

  **6.    AWS 과금 사용량**

* 등록 한 전체 AWS 계정의 과금 사용량을 보여 준다.

### 2.9.    대시보드 -&gt; Openstack 리소스 사용량

등록 한 Openstack 계정의 전체 리소스 사용량을 조회 할 수 있는 화면 이다.

![](../../../.gitbook/assets/openstackResource%20%283%29.png)

#### 1.    리소스 사용량 조회 화면 이동

* 선택 한 인프라 환경의 리소스 사용량 조회 화면으로 이동 하는 기능을 제공 한다.

  **2.    Openstack 인스턴스 사용량**

* 등록 한 전체 Openstack 계정의 인스턴스 사용량을 보여 준다

  **3.    Openstack 네트워크 사용량.**

* 등록 한 전체 Openstack 계정의 네트워크 사용량을 보여 준다.

  **4.    Openstack 볼륨 사용량**

* 등록 한 전체 Openstack 계정의 볼륨 사용량을 보여 준다.

### 2.10.    인프라 관리 -&gt; AWS 관리

기본 AWS 계정의 VPC, Subnet, Internet Gateway, Elastic IP, Security Group, Key Pair 관리 화면으로 이동 할 수 있는 화면이다.

![](../../../.gitbook/assets/AwsMgnt%20%283%29.png)

#### 1.    기본 AWS 계정 변경

* 등록 한 AWS 계정을 기본 AWS 계정으로 선택 할 수 있는 기능을 제공 한다.

  **2.    AWS Security Group 관리**

* AWS Security Group 관리 화면으로 이동 하는 기능을 수행 한다.

  **3.    AWS Key Pair 관리**

* AWS Key Pair 관리 화면으로 이동 하는 기능을 수행 한다.

  **4.    AWS Subnet 관리**

* AWS Subnet 관리 화면으로 이동 하는 기능을 수행 한다.

  **5.    AWS VPC 관리**

* AWS VPC 관리 화면으로 이동 하는 기능을 수행 한다.

  **6.    AWS Internet Gateway 관리**

* AWS Internet Gateway 관리 화면으로 이동 하는 기능을 수행 한다.

  **7.    AWS Elastic IPs 관리**

* AWS Elastic IP 관리 화면으로 이동 하는 기능을 수행 한다.

### 2.11.    인프라 관리 -&gt; AWS 관리 -&gt; VPC 관리

설정 한 AWS 계정의 VPC를 생성/삭제 할 수 있는 기능을 제공 하는 화면 이다.

![](../../../.gitbook/assets/AwsVpcMgnt%20%284%29.png)

#### 1.    AWS 관리 화면 이동

* 선택 한 AWS 관리 화면으로 이동 하는 기능을 제공 한다.

  **2.    AWS Region 선택**

* AWS Region을 선택 할 수 있는 기능을 제공 한다.

  **3.    AWS 계정 명**

* 기본 AWS 계정을 선택 할 수 있는 기능을 제공 한다.

  **4.    AWS VPC 목록**

* 실제 AWS에 생성 한 VPC 목록을 보여 준다.

  **5.    생성**

* AWS VPC를 생성 할 수 있는 기능을 제공 한다.

  **6.    삭제**

* AWS VPC를 삭제 할 수 있는 기능을 제공 한다.

  **7.    AWS VPC 상세 목록**

* 선택 한 AWS VPC에 대한 상세 목록을 보여 준다.

### 2.12.    인프라 관리 -&gt; AWS 관리 -&gt; Subnet 관리

설정 한 AWS 계정의 Subnet을 생성/삭제 할 수 있는 기능을 제공 하는 화면 이다.

![](../../../.gitbook/assets/AwsSubnetMgnt%20%284%29.png)

#### 1.    AWS 관리 화면 이동

* 선택 한 AWS 관리 화면으로 이동 하는 기능을 제공 한다.

  **2.    AWS Region 선택**

* AWS Region을 선택 할 수 있는 기능을 제공 한다.

  **3.    AWS 계정 명**

* 기본 AWS 계정을 선택 할 수 있는 기능을 제공 한다.

  **4.    AWS Subnet 목록**

* 실제 AWS에 생성 한 Subnet 목록을 보여 준다.

  **5.    생성**

* AWS Subnet를 생성 할 수 있는 기능을 제공 한다.

  **6.    삭제**

* AWS Subnet를 삭제 할 수 있는 기능을 제공 한다.

  **7.    AWS Subnet 상세 목록**

* 선택 한 AWS Subnet에 대한 상세 목록을 보여 준다.

### 2.13.    인프라 관리 -&gt; AWS 관리 -&gt; Internet Gateway 관리

설정 한 AWS 계정의 Internet Gateway를 생성/삭제, VPC를 연결/연결 해제 할 수 있는 기능을 제공 하는 화면 이다.

![](../../../.gitbook/assets/AwsInternetGatewayMgnt%20%284%29.png)

#### 1.    AWS 관리 화면 이동

* 선택 한 AWS 관리 화면으로 이동 하는 기능을 제공 한다.

  **2.    AWS Region 선택**

* AWS Region을 선택 할 수 있는 기능을 제공 한다.

  **3.    AWS 계정 명**

* 기본 AWS 계정을 선택 할 수 있는 기능을 제공 한다.

  **4.    AWS Internet Gateway 목록**

* 실제 AWS에 생성 한 Internet Gateway 목록을 보여 준다.

  **5.    생성**

* AWS Internet Gateway를 생성 할 수 있는 기능을 제공 한다.

  **6.    VPC 연결**

* AWS Internet Gateway에 VPC를 연결 할 수 있는 기능을 제공 한다.

  **7.    VPC 연결 해제**

* AWS Internet Gateway에 연결 한 VPC를 연결 해제 할 수 있는 기능을 제공 한다.

  **8.    삭제**

* AWS Internet Gateway를 삭제 할 수 있는 기능을 제공 한다.

### 2.14.    인프라 관리 -&gt; AWS 관리 -&gt; Securty Group 관리

설정 한 AWS 계정의 Security Group를 생성/삭제 할 수 있는 기능을 제공 하는 화면 이다.

![](../../../.gitbook/assets/AwsSecurityGroupMgnt%20%283%29.png)

#### 1.    AWS 관리 화면 이동

* 선택 한 AWS 관리 화면으로 이동 하는 기능을 제공 한다.

  **2.    AWS Region 선택**

* AWS Region을 선택 할 수 있는 기능을 제공 한다.

  **3.    AWS 계정 명**

* 기본 AWS 계정을 선택 할 수 있는 기능을 제공 한다.

  **4.    AWS Security Group 목록**

* 실제 AWS에 생성 한 Security Group 목록을 보여 준다.

  **5.    생성**

* AWS Security Group를 생성 할 수 있는 기능을 제공 한다.

  **6.    삭제**

* AWS Security Group를 삭제 할 수 있는 기능을 제공 한다.

  **7.    Inbound Rule 상세 보기**

* 선택 한 Security Group에 생성 한 Inbound Rule목록을 보여 준다.

### 2.15.    인프라 관리 -&gt; AWS 관리 -&gt; Elastic IPs 관리

설정 한 AWS 계정의 Elastic IP를 할당 할 수 있는 기능을 제공 하는 화면 이다.

![](../../../.gitbook/assets/AwsElasticIpMgnt%20%283%29.png)

#### 1.    AWS 관리 화면 이동

* 선택 한 AWS 관리 화면으로 이동 하는 기능을 제공 한다.

  **2.    AWS Region 선택**

* AWS Region을 선택 할 수 있는 기능을 제공 한다.

  **3.    AWS 계정 명**

* 기본 AWS 계정을 선택 할 수 있는 기능을 제공 한다.

  **4.    AWS Elastic IP 목록**

* 실제 AWS에 할당한 Elastic IP 목록을 보여 준다.

  **5.    할당**

* AWS Elastic IP를 할당 할 수 있는 기능을 제공 한다.

  **6.    Elastic IP 상세 보기**

* 선택 한 Elastic IP 상세 정보를 보여 준다.

### 2.16.    인프라 관리 -&gt; AWS 관리 -&gt; Key Pair 관리

설정 한 AWS 계정의 Key Pair를 생성 할 수 있는 기능을 제공 하는 화면 이다.

![](../../../.gitbook/assets/AwsKeyPairMgnt%20%284%29.png)

#### 1.    AWS 관리 화면 이동

* 선택 한 AWS 관리 화면으로 이동 하는 기능을 제공 한다.

  **2.    AWS Region 선택**

* AWS Region을 선택 할 수 있는 기능을 제공 한다.

  **3.    AWS 계정 명**

* 기본 AWS 계정을 선택 할 수 있는 기능을 제공 한다.

  **4.    AWS Key Pair 목록**

* 실제 AWS에 생성 한 Key Pair 목록을 보여 준다.

  **5.    생성**

* AWS Key Pair를 생성 할 수 있는 기능을 제공 한다.

### 2.17.    인프라 관리 -&gt; Openstack 관리

기본 Openstack 계정의 Network, Router, Key Pair, Floating IP, Security Group관리 화면으로 이동 할 수 있는 화면이다.

![](../../../.gitbook/assets/OpenstackMgnt%20%284%29.png)

#### 1.    기본 Openstack 계정 변경

* 등록 한 Openstack 계정을 기본 Openstack 계정으로 선택 할 수 있는 기능을 제공 한다.

  **2.    Openstack Network 관리**

* Openstack Network 관리 화면으로 이동 하는 기능을 수행 한다.

  **3.    Openstack Router 관리**

* Openstack Router 관리 화면으로 이동 하는 기능을 수행 한다.

  **4.    Openstack Key Pair 관리**

* Openstack Key Pair 관리 화면으로 이동 하는 기능을 수행 한다.

  **5.    Openstack Floating IP 관리**

* Openstack Floating IP 관리 화면으로 이동 하는 기능을 수행 한다.

  **6.    Openstack Security Group 관리**

* Openstack Security Group 관리 화면으로 이동 하는 기능을 수행 한다.

### 2.18.    인프라 관리 -&gt; Openstack 관리 -&gt; Network 관리

설정 한 Openstack 계정의 Network 및 Subnet을 생성/삭제를 할 수 있는 기능을 제공 하는 화면 이다.

![](../../../.gitbook/assets/OpenstackNetworkMgnt%20%283%29.png)

#### 1.    Openstack 관리 화면 이동

* 선택 한 Openstack 관리 화면으로 이동 하는 기능을 제공 한다.

  **2.    Openstack 계정 명**

* 기본 Openstack 계정을 선택 할 수 있는 기능을 제공 한다.

  **3.    Openstack Network 목록**

* 실제 Openstack에 생성 한 Network 목록을 보여 준다.

  **4.    생성**

* Openstack Network를 생성 할 수 있는 기능을 제공 한다.

  **5.    삭제**

* Openstack Network를 삭제 할 수 있는 기능을 제공 한다.

  **6.    서브넷 설정**

* 선택한 Openstack Network에 Subnet을 생성 및 삭제 하는 기능을 제공 한다.

  **7.    Network 상세 보기**

* 선택 한 Openstack Network의 상세 정보를 보여 준다.

### 2.19.    인프라 관리 -&gt; Openstack 관리 -&gt; Router 관리

설정 한 Openstack 계정의 Router를 생성/삭제, 인터페이스 및 게이트웨이를 연결/연결 해제 할 수 있는 기능을 제공 하는 화면 이다.

![](../../../.gitbook/assets/OpenstackRouterMgnt%20%284%29.png)

#### 1.    Openstack 관리 화면 이동

* 선택 한 Openstack 관리 화면으로 이동 하는 기능을 제공 한다.

  **2.    Openstack 계정 명**

* 기본 Openstack 계정을 선택 할 수 있는 기능을 제공 한다.

  **3.    Openstack Router 목록**

* 실제 Openstack에 생성 한 Router 목록을 보여 준다.

  **4.    생성**

* Openstack Router를 생성 할 수 있는 기능을 제공 한다.

  **5.    인터페이스**

* 선택 한 Openstack Router에 인터페이스를 연결/연결 해제 하는 기능을 제공 한다.

  **6.    게이트웨이**

* 선택한 Openstack Router에 게이트웨이를 연결/연결해제 하는 기능을 제공 한다.

  **7.    삭제**

* 선택 한 Openstack Router를 삭제 하는 기능을 제공 한다.

### 2.20.    인프라 관리 -&gt; Openstack 관리 -&gt; Security Group 관리

설정 한 Openstack 계정의 Security Group를 생성/삭제 할 수 있는 기능을 제공 하는 화면 이다.

![](../../../.gitbook/assets/OpenstackSecurityGroupMgnt%20%283%29.png)

#### 1.    Openstack 관리 화면 이동

* 선택 한 Openstack 관리 화면으로 이동 하는 기능을 제공 한다.

  **2.    Openstack 계정 명**

* 기본 Openstack 계정을 선택 할 수 있는 기능을 제공 한다.

  **3.    Openstack Security Group목록**

* 실제 Openstack에 생성 한 Security Group목록을 보여 준다.

  **4.    생성**

* Openstack Security Group를 생성 할 수 있는 기능을 제공 한다.

  **5.    삭제**

* 선택 한 Security Group를 삭제 할 수 있는 기능을 제공 한다.

  **6.    Inbound Rule 정보**

* 선택한 Openstack Security Group의 Inbound Rule정보를 보여 준다.

### 2.21.    인프라 관리 -&gt; Openstack 관리 -&gt; Floating Ips 관리

설정 한 Openstack 계정의 Floating IP를 할당 할 수 있는 기능을 제공 하는 화면 이다.

![](../../../.gitbook/assets/OpenstackFlotingIPMgnt%20%284%29.png)

#### 1.    Openstack 관리 화면 이동

* 선택 한 Openstack 관리 화면으로 이동 하는 기능을 제공 한다.

  **2.    Openstack 계정 명**

* 기본 Openstack 계정을 선택 할 수 있는 기능을 제공 한다.

  **3.    Openstack Floating IP 목록**

* 실제 Openstack에 할당 한 Floating IP 목록을 보여 준다.

  **4.    Openstack Floating IP 할당**

* Openstack Floating IP를 할당 할 수 있는 기능을 제공 한다.

### 2.22.    인프라 관리 -&gt; Openstack 관리 -&gt; Key Pair 관리

설정 한 Openstack 계정의 Floating IP를 할당 할 수 있는 기능을 제공 하는 화면 이다.

![](../../../.gitbook/assets/OpenstackKeyPairMgnt%20%284%29.png)

#### 1.    Openstack 관리 화면 이동

* 선택 한 Openstack 관리 화면으로 이동 하는 기능을 제공 한다.

  **2.    Openstack 계정 명**

* 기본 Openstack 계정을 선택 할 수 있는 기능을 제공 한다.

  **3.    Openstack Key Pair 목록**

* 실제 Openstack에 생성 한 Key Pair 목록을 보여 준다.

  **4.    Openstack Key Pair 생성**

* Openstack Key Pair를 할당 할 수 있는 기능을 제공 한다.

## 3.  인프라 관리 대시보드 활용

플랫폼 설치 자동화를 이용해서 PaaS-TA를 설치 하기 위해서는 설치 할 인프라에 최소한의 환경 설정이 필요하다. 인프라 관리 대시보드는 설치 할 인프라 환경에 PaaS-TA 설치를 하기 위해 환경 설정을 할 수 있는 대시보드이다.

다음 그림은 PaaS-TA를 설치 하기 위해 인프라 대시보드를 이용하여 AWS, Openstack 인프라 환경 설정을 구성하는 절차이다.

### 3.1.    AWS 환경 설정 절차

![](../../../.gitbook/assets/AwsConfigProcess%20%283%29.png)

### 3.2.    Openstack 환경 설정 절차

![](../../../.gitbook/assets/OpenstackConfigProcess%20%283%29.png)

### 3.3.    계정 관리

인프라 관리 대시보드 웹 화면에서 “계정 관리” 메뉴로 이동한다. 인프라 관리 대시보드는 “계정 관리” 메뉴에서 등록 한 전체 인프라 환경의 계정 정보와 각 각의 인프라 계정 관리 화면 이동을 제공 한다. \(계정 관리 화면 설명은 [2.2](paas-ta_-_-_-_-_v1.0.md#7) 참고\)

### 3.4.    AWS 계정 관리

인프라 관리 대시보드 웹 화면에서 “계정 관리” -&gt; “AWS 계정 관리” 메뉴로 이동한다. 인프라 관리 대시보드는 “AWS 계정 관리” 메뉴에서 AWS 계정을 등록/삭제 할 수 있는 기능을 제공 한다. \(AWS 계정 관리 화면 설명은 [2.3](paas-ta_-_-_-_-_v1.0.md#8) 참고\)

#### 1.    AWS 계정 등록

**1.1AWS 계정 목록 화면에서 “등록” 버튼을 클릭하고 AWS 계정 등록 팝업 화면에서 실제 AWS 계정 정보를 입력 후 “확인” 버튼을 클릭한다.**

![](../../../.gitbook/assets/AwsAccountAdd%20%284%29.png)

### 3.5.    Openstack 계정 관리

인프라 관리 대시보드 웹 화면에서 “계정 관리” -&gt; “Openstack 계정 관리” 메뉴로 이동한다. 인프라 관리 대시보드는 “Openstack 계정” 메뉴에서 Openstack 계정 정보를 등록/수정/삭제 할 수 있는 기능을 제공 한다. \(Openstack 계정 관리 화면 설명은 [2.4](paas-ta_-_-_-_-_v1.0.md#9) 참고\)

#### 1.    Openstack 계정 등록

**1.1.    Openstack 계정 목록 화면에서 “등록” 버튼을 클릭하고 Openstack 계정 등록 팝업 화면에서 실제 Openstack 계정 정보를 입력 후 “확인” 버튼을 클릭한다.**

![](../../../.gitbook/assets/OpenstackAccountAdd%20%284%29.png)

#### 2.    Openstack 계정 수정

**2.1.    Openstack 계정 목록 화면에서 Openstack 계정을 선택 하고 “수정” 버튼을 클릭 한다.**

**2.2.    Openstack 계정 수정 팝업 화면에서 Openstack 계정 정보를 수정 하고 “확인” 버튼을 클릭 한다.**

**2.3.    플랫폼 관리자는 Openstack 계정 별칭과 버전, Identity, UserName을 변경 할 수 없지만 패스워드나 프로젝트, 도메인을 변경 할 수 있다.**

![](../../../.gitbook/assets/OpenstackAccountModify%20%283%29.png)

### 3.6.    Google 계정 관리

인프라 관리 대시보드 웹 화면에서 “계정 관리” -&gt; “Google 계정 관리” 메뉴로 이동한다. 인프라 관리 대시보드는 “Google 계정” 메뉴에서 Google 계정 정보를 등록/수정/삭제 할 수 있는 기능을 제공 한다. \(Google 계정 관리 화면 설명은 [2.5](paas-ta_-_-_-_-_v1.0.md#10) 참고\)

#### 1.    Google 계정 등록

**1.1.    Google 계정 목록 화면에서 “등록” 버튼을 클릭하고 Google 계정 등록 팝업 화면에서 실제 Google 계정 정보를 입력 후 “확인” 버튼을 클릭한다.**

![](../../../.gitbook/assets/GoogleAccountAdd%20%283%29.png)

#### 2.    Google 계정 수정

**2.1.    Google 계정 목록 화면에서 Google 계정을 선택 하고 “수정” 버튼을 클릭 한다.**

**2.2.    Google 계정 수정 팝업 화면에서 Google 계정 정보를 수정 하고 “확인” 버튼을 클릭 한다.**

**2.3.    플랫폼 관리자는 Google 계정**

![](../../../.gitbook/assets/GoogleAccountModify%20%284%29.png)

### 3.7.    vSphere 계정 관리

인프라 관리 대시보드 웹 화면에서 “계정 관리” -&gt; “vSphere 계정 관리” 메뉴로 이동한다. 인프라 관리 대시보드는 “vSphere 계정” 메뉴에서 vSphere 계정 정보를 등록/수정/삭제 할 수 있는 기능을 제공 한다. \(vSphere 계정 관리 화면 설명은 [2.6](paas-ta_-_-_-_-_v1.0.md#10) 참고\)

#### 1.    vSphere 계정 등록

**1.1.    vSphere 계정 목록 화면에서 “등록” 버튼을 클릭하고 vSphere 계정 등록 팝업 화면에서 실제 vSphere 계정 정보를 입력 후 “확인” 버튼을 클릭한다.**

![](../../../.gitbook/assets/vSphereAccountAdd%20%284%29.png)

#### 2.    vSphere 계정 수정

**2.1.    vSphere 계정 목록 화면에서 vSphere 계정을 선택 하고 “수정” 버튼을 클릭 한다.**

**2.2.    vSphere 계정 수정 팝업 화면에서 vSphere 계정 정보를 수정 하고 “확인” 버튼을 클릭 한다.**

![](../../../.gitbook/assets/vSphereAccountModify%20%284%29.png)

### 3.8.    AWS 관리

인프라 관리 대시보드 웹 화면에서 “인프라 관리” -&gt; “AWS 관리” 메뉴로 이동한다. 인프라 관리 대시보드는 “AWS 관리” 메뉴에서 PaaS-TA 설치를 위한 AWS 환경 정보를 생성 할 수 있는 화면으로 이동할 수 있는 기능을 제공 한다. \(AWS 관리 화면 설명은 [2.10](paas-ta_-_-_-_-_v1.0.md#15) 참고\)

### 3.9.    AWS VPC 관리

인프라 관리 대시보드 웹 화면에서 “인프라 관리” -&gt; “AWS 관리” -&gt; “VPC 관리” 메뉴로 이동한다. 인프라 관리 대시보드는 “VPC 관리” 메뉴에서 VPC를 생성/삭제 할 수 있는 기능을 제공 한다. \(VPC관리 화면 설명은 [2.11](paas-ta_-_-_-_-_v1.0.md#16) 참고\)

#### 1.    AWS VPC 생성

**1.1.    VPC 목록 화면에서 “생성” 버튼을 클릭하고 VPC 생성 팝업 화면에서 VPC 정보를 입력 후 “확인” 버튼을 클릭한다.**

![](../../../.gitbook/assets/AwsVpcAdd%20%284%29.png)

### 3.10.    AWS Subent 관리

인프라 관리 대시보드 웹 화면에서 “인프라 관리” -&gt; “AWS 관리” -&gt; “Subnet 관리” 메뉴로 이동한다. 인프라 관리 대시보드는 “Subnet 관리” 메뉴에서 Subnet를 생성/삭제 할 수 있는 기능을 제공 한다. \(Subnet 관리 화면 설명은 [2.12](paas-ta_-_-_-_-_v1.0.md#17) 참고\)

#### 1.    AWS Subnet 생성

**1.1.    Subnet 목록 화면에서 “생성” 버튼을 클릭하고 Subnet 생성 팝업 화면에서 Subnet 정보를 입력 후 “확인” 버튼을 클릭한다.**

![](../../../.gitbook/assets/AwsSubnetAdd%20%284%29.png)

### 3.11.    AWS Internet Gateway 관리

인프라 관리 대시보드 웹 화면에서 “인프라 관리” -&gt; “AWS 관리” -&gt; “Internet Gateway 관리” 메뉴로 이동한다. 인프라 관리 대시보드는 “Internet Gateway 관리” 메뉴에서 Internet Gateway를 생성/삭제 및 VPC 연결/연결 해제 할 수 있는 기능을 제공 한다. \(Internet Gateway 관리 화면 설명 [2.13](paas-ta_-_-_-_-_v1.0.md#18) 참고\)

#### 1.    AWS Internet Gateway 생성

**1.1.    Internet Gateway 목록 화면에서 “생성” 버튼을 클릭하고 Internet Gateway 생성 팝업 화면에서 Internet Gateway 정보를 입력 후 “확인” 버튼을 클릭한다.**

![](../../../.gitbook/assets/AwsInternetGatewayAdd%20%284%29.png)

#### 2.    AWS Internet Gateway에 VPC 연결

**2.1.    Internet Gateway 목록 화면에서 생성 한 Internet Gateway를 선택 한다.**

**2.2.    Internet Gateway 목록 화면에서 “VPC 연결” 버튼을 클릭하고 VPC 연결 팝업 화면에서 생성 한 VPC를 선택 한 후 “확인” 버튼을 클릭한다.**

![](../../../.gitbook/assets/AwsVpcAttach%20%283%29.png)

### 3.12.    AWS Security Group 관리

인프라 관리 대시보드 웹 화면에서 “인프라 관리” -&gt; “AWS 관리” -&gt; “Security Group 관리” 메뉴로 이동한다. 인프라 관리 대시보드는 “Security Group 관리” 메뉴에서 Security Group를 생성/삭제 할 수 있는 기능을 제공 한다. \(Security Group 관리 화면 설명 [2.14](paas-ta_-_-_-_-_v1.0.md#19) 참고\)

#### 1.    AWS Security Group 생성

**1.1.    Security Group 목록 화면에서 “생성” 버튼을 클릭하고 Security Group 생성 팝업 화면에서 Security Group 정보와 생성 한 VPC를 선택 및 입력 후 “확인” 버튼을 클릭한다.**

**1.2.    설치 할 PaaS-TA 환경에 맞는 Ingress Rule을 선택 하고 생성 한다.**

![](../../../.gitbook/assets/AwsSecurityGroupAdd%20%283%29.png)

### 3.13.    AWS Elastic Ips 관리

인프라 관리 대시보드 웹 화면에서 “인프라 관리” -&gt; “AWS 관리” -&gt; “Elastic IPs 관리” 메뉴로 이동한다. 인프라 관리 대시보드는 “Elastic IPs 관리” 메뉴에서 Elastic IP를 할당 할 수 있는 기능을 제공 한다. \(Elastic IP 관리 화면 설명 [2.15](paas-ta_-_-_-_-_v1.0.md#20) 참고\)

#### 1.    AWS Elastic IP 할당

**1.1.    Elastic IP 목록 화면에서 “할당” 버튼을 클릭하고 Elastic IP 할당 팝업 화면에서 “확인” 버튼을 클릭 한다.**

![](../../../.gitbook/assets/AwsElassticIpAdd%20%284%29.png)

### 3.14.    AWS Key Pair 관리

인프라 관리 대시보드 웹 화면에서 “인프라 관리” -&gt; “AWS 관리” -&gt; “Key Pair 관리” 메뉴로 이동한다. 인프라 관리 대시보드는 “Key Pair 관리” 메뉴에서 Key Pair를 생성 할 수 있는 기능을 제공 한다. \(Key Pair 관리 화면 설명은 [2.16](paas-ta_-_-_-_-_v1.0.md#21) 참고\)

#### 1.    AWS Key Pair 생성

**1.1.    Key Pair 목록 화면에서 “생성” 버튼을 클릭하고 Key Pair 생성 팝업 화면에서 Key Pair 명을 입력 한 후 “확인” 버튼을 클릭 한다.**

![](../../../.gitbook/assets/AwsKeyPairAdd%20%283%29.png)

### 3.15.    Openstack 관리

인프라 관리 대시보드 웹 화면에서 “인프라 관리” -&gt; “Openstack 관리” 메뉴로 이동한다. 인프라 관리 대시보드는 “Openstack 관리” 메뉴에서 PaaS-TA 설치를 위한 Openstack 환경 정보를 생성 할 수 있는 화면으로 이동할 수 있는 기능을 제공 한다. \(Openstack 관리 화면 설명은 [2.17](paas-ta_-_-_-_-_v1.0.md#22) 참고\)

### 3.16.    Openstack Network 관리

인프라 관리 대시보드 웹 화면에서 “인프라 관리” -&gt; “Openstack 관리” -&gt; “Network 관리” 메뉴로 이동한다. 인프라 관리 대시보드는 “Network 관리” 메뉴에서 Network와 Subnet을 생성/삭제 할 수 있는 기능을 제공 한다. \(Network관리 화면 설명은 [2.18](paas-ta_-_-_-_-_v1.0.md#23) 참고\)

#### 1.    Openstack Network 생성

**1.1.    Network 목록 화면에서 “생성” 버튼을 클릭하고 Network 생성 팝업 화면에서 Network 정보를 입력 한 후 “확인” 버튼을 클릭 한다.**

![](../../../.gitbook/assets/OpenstackNetworkAdd%20%283%29.png)

#### 2.    Openstack Subnet 생성

**2.1.    Network 목록 화면에서 Network를 선택 하고 “서브넷 설정” 버튼을 클릭 한다.**

**2.2.    Subnet 설정 팝업 화면에서 Subnet 정보를 입력 한 후 “생성” 버튼을 클릭 한다.**

![](../../../.gitbook/assets/OpenstackSubnetAdd%20%284%29.png)

### 3.17.    Openstack Router 관리

인프라 관리 대시보드 웹 화면에서 “인프라 관리” -&gt; “Openstack 관리” -&gt; “Router 관리” 메뉴로 이동한다. 인프라 관리 대시보드는 “Router 관리” 메뉴에서 Router를 생성/삭제, 인터페이스, 게이트웨이를 연결/연결 해제 할 수 있는 기능을 제공 한다. \(Router 관리 화면 설명은 [2.19](paas-ta_-_-_-_-_v1.0.md#24) 참고\)

#### 1.    Openstack Router생성

**1.1.    Router목록 화면에서 “생성” 버튼을 클릭 하고 Router 생성 팝업 화면에서 Router 명을 입력 하고 “확인” 버튼을 클릭 한다.**

![](../../../.gitbook/assets/OpenstackRouterAdd%20%284%29.png)

#### 2.    Openstack Router에 인터페이스 연결

**2.1.    Router목록 화면에서 생성한 Router를 선택 하고 “인터페이스” 버튼을 클릭 한다.**

**2.2.    인터페이스 설정 화면에서 Subnet을 선택하고 인터페이스 설정 정보를 입력 후 “연결” 버튼을 클릭 한다.**

![](../../../.gitbook/assets/OpenstackInterFaceAttach%20%284%29.png)

#### 3.    Openstack Router에 게이트웨이 연결

**3.1.    Router목록 화면에서 생성한 Router를 선택 하고 “게이트웨이” 버튼을 클릭 한다.**

**3.2.    게이트웨이 설정 화면에서 External Network를 선택하고 “연결” 버튼을 클릭 한다.**

![](../../../.gitbook/assets/OpenstackGatewayAttach%20%284%29.png)

### 3.18.    Openstack Security Group 관리

인프라 관리 대시보드 웹 화면에서 “인프라 관리” -&gt; “Openstack 관리” -&gt; “Security Group 관리” 메뉴로 이동한다. 인프라 관리 대시보드는 “Security Group 관리” 메뉴에서 Security Group를 생성/삭제 할 수 있는 기능을 제공 한다. \(Security Group 관리 화면 설명은 [2.20](paas-ta_-_-_-_-_v1.0.md#25) 참고\)

#### 1.    Openstack Security Group생성

**1.1.    Security Group목록 화면에서 “생성” 버튼을 클릭 하고 Security Group생성 팝업 화면에서 Security Group 정보를 입력 후 “확인” 버튼을 클릭 한다.**

**1.2.    설치 할 PaaS-TA 환경에 맞는 Ingress Rule을 선택 하고 생성 한다.**

![](../../../.gitbook/assets/OpenstackSecurityGroupAdd%20%283%29.png)

### 3.19.    Openstack Floating IPs 관리

인프라 관리 대시보드 웹 화면에서 “인프라 관리” -&gt; “Openstack 관리” -&gt; “Floating IPs 관리” 메뉴로 이동한다. 인프라 관리 대시보드는 “Floating IPs 관리” 메뉴에서 Floating IP를 할당 할 수 있는 기능을 제공 한다. \(Floating IPs 관리 화면 설명은 [2.21](paas-ta_-_-_-_-_v1.0.md#26) 참고\)

#### 1.    Openstack Floating IP 할당

**1.1.    Floating IP 목록 화면에서 “할당” 버튼을 클릭 하고 Floating IP 할당 팝업 화면에서 Pool 정보를 선택 후 “확인” 버튼을 클릭 한다.**

![](../../../.gitbook/assets/OpenstackFloatingIPAdd%20%283%29.png)

### 3.20.    Openstack Key Pair 관리

인프라 관리 대시보드 웹 화면에서 “인프라 관리” -&gt; “Openstack 관리” -&gt; “Key Pair 관리” 메뉴로 이동한다. 인프라 관리 대시보드는 “Key Pair 관리” 메뉴에서 Key Pair를 생성 할 수 있는 기능을 제공 한다. \(Key Pair관리 화면 설명은 [2.22](paas-ta_-_-_-_-_v1.0.md#27) 참고\)

#### 1.    Openstack Key Pair 생성

**1.1.    Key Pair 목록 화면에서 “생성” 버튼을 클릭 하고 Key Pair 생성 팝업 화면에서 Key Pair 명을 입력 후 “확인” 버튼을 클릭 한다.**

![](../../../.gitbook/assets/OpenstackKeyPairAdd%20%283%29.png)

