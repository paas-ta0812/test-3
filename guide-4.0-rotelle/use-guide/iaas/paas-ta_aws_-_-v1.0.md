# PaaS-TA\_AWS\_환경설정\_가이드v1.0

## 1.    PaaS 플랫폼 배포를 위한 AWS 환경 설정

#### Executive Summary

본 문서는 PaaS-TA가 지원하는 IaaS 환경 중 AWS에 대한 BOSH 설치 환경 설정하는 방법에 대해서 설명하였다.

## 1.1   목적

본 문서에서는 Inception 서버 환경에 BOSH를 설치하여 기능을 테스트할 수 있는 환경을 구축 시 AWS 기본 환경 설정을 하는 데 목적이 있다.

## 1.2   범위

본 가이드에서는 Linux 환경\(Ubuntu 14.04 TRUSTY\) 을 설치환경 구성 기준으로 작성 하였으며, 전제 조건으로는 AWS 계정이 있어야 한다.

## 1.3   참고자료

본 문서는 Cloud Foundry의 BOSH Document와 Openstack Document를 참고로 작성하였다.

* BOSH Document: [http://bosh.io](http://bosh.io)
* Openstack Document: [https://aws.amazon.com/ko/documentation/](https://aws.amazon.com/ko/documentation/)

## 2.1.  AWS 기본 환경설정

BOSH를 배포하기 위한 AWS 사전 준비사항 및 필요한 사항을 아래의 가이드를 참조하여 설정한다.

#### 사전 준비 및 확인 사항

* AWS 서비스 사용 가능 확인
  * IAM: 인증 및 AWS 서비스들의 Endpoint 조회
  * Compute\(EC2\): VPC 생성과 Elastic IPs 할당, 볼륨 생성 및 Attach
  * Image: 이미지 서비스를 이용한 Stemcell 저장
* Network Interface
  * 외부 또는 내부 네트워크의 Subnet
  * Elastic IPs

### -   Access Key 생성

```text
AWS에 계정이 있어야 한다.
```

1. AWS에 로그인: [https://console.aws.amazon.com/console/home](https://console.aws.amazon.com/console/home)

   ![](../../../.gitbook/assets/aws_access1%20%282%29.png)

2. 화면 우측 상단의 계정을 선택하여 My Security Credentials를 선택한다.

   ![](../../../.gitbook/assets/aws_access2%20%282%29.png)

3. \[Access Keys\]를 선택하여 \[Create New Access Key\] 버튼을 눌러 Access Key를 생성한다.

   ![](../../../.gitbook/assets/aws_access3%20%282%29.png)

4. 생성한 키 정보를 확인한다.

   * 화면의 Access Key ID 를 Manifest 의 access\_key\_id 에 설정한다.
   * 화면의 Secret Access Key 를 Manifest 의 Secret\_access\_key 에 설정한다.

   ![](../../../.gitbook/assets/aws_access4%20%282%29.png)

### -   AWS Virtual Private Cloud\(VPC\) 생성

1. 화면 우측 상단의 지역메뉴를 선택한다.

   ![](../../../.gitbook/assets/aws_vpc1%20%282%29.png)

2. AWS 콘솔 화면에서 VPC 메뉴를 선택한다.

   ![](../../../.gitbook/assets/aws_vpc2%20%282%29.png)

3. VPC 마법사를 선택한다.

   ![](../../../.gitbook/assets/aws_vpc3%20%282%29.png)

4. 단일 공용 서브넷이 있는 VPC 탭을 선택하고 \[Select\] 버튼을 클릭한다.

   ![](../../../.gitbook/assets/aws_vpc4%20%282%29.png)

5. 네트워크 정보를 입력하고 VPC 생성 버튼을 눌러 VPC를 생성한다.

   ![](../../../.gitbook/assets/aws_vpc5%20%282%29.png)

6. 아래와 같이 생성한 VPC의 목록이 출력된다.

   ![](../../../.gitbook/assets/aws_vpc6%20%282%29.png)

### -  AWS Elastic IP 생성

1. AWS \[VPC 대시보드\] 에서 \[Elastic IPs\] 버튼과 \[Allocate New Address\] 버튼을 차례로 눌러 Elastic IP를 생성한다.

   ![](../../../.gitbook/assets/aws_elastic1%20%282%29.png)

2. Allocate new Address 화면에서 \[Allocate\] 버튼을 눌러 Elastic IP를 생성한다.

   ![](../../../.gitbook/assets/aws_elastic2%20%282%29.png)

3. 생성한 Elastic IP를 확인 후 Manifest 설정에서 Elastic IP를 사용한다.

   ![](../../../.gitbook/assets/aws_elastic3%20%282%29.png)

### -  AWS Key Pair 생성

1. AWS 콘솔 화면에서 \[EC2\] 메뉴를 선택하여 EC2 대시보드 화면으로 이동한다.

   ![](../../../.gitbook/assets/aws_keypair1%20%282%29.png)

2. \[Key Pairs\] 메뉴를 선택하고 \[Create Key Pair\] 버튼을 선택한다.
3. Key Pair 생성 다이얼로그 화면에서 Key Pair명을 입력하여 Key Pair를 생성하고 다운로드 한다.
   * Key Pair Name 에 입력한 값을 Manifest 의 default\_key\_name 에 설정한다.

     ![](../../../.gitbook/assets/aws_keypair2%20%282%29.png)
4. 다운로드한 Key\(예: bosh.pem\)를 키 보관 디렉토리\(~/.ssh\)에 옮기고 권한을 변경한다.
   * ‘~/.ssh/bosh.pem’ 값을 manifest의 private\_key에 설정한다.

     ![](../../../.gitbook/assets/aws_keypair3%20%282%29.png)

### -  AWS Security Group 생성

1. AWS EC2 대시보드 화면에서 \[Security Groups\] 메뉴를 선택하고 \[Create Security Group\] 버튼을 누른다.

   ![](../../../.gitbook/assets/aws_security1%20%282%29.png)

2. \[Create Security Group\] 팝업 화면에서 값을 입력하고 \[Create\] 버튼을 누른다.

   ![](../../../.gitbook/assets/aws_security2%20%282%29.png)

3. 생성한 시큐리티 그룹에 보안정책을 설정하기 위해 \[Inbound\] 탭의 \[Edit\] 버튼을 클릭한다.

   ![](../../../.gitbook/assets/aws_security3%20%282%29.png)

4. Edit inbound rules 다이얼로그 화면에서 아래와 같이 BOOTSTRAP 보안정책을 설정하고 \[Save\] 버튼을 클릭한다.

   ![](../../../.gitbook/assets/aws_security4%20%282%29.png)

5. Edit inbound rules 다이얼로그 화면에서 아래와 같이 CF 보안정책을 설정하고 \[Save\] 버튼을 클릭한다.

   !\[aws-security5\]

\[aws-security5\]:../images/IaaS/AWS/aws\_security5.png

