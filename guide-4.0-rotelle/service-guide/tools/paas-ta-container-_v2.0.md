# Table of Contents

[1. 문서 개요](paas-ta-container-_v2.0.md#1)

* [1.1. 목적](paas-ta-container-_v2.0.md#11)
* [1.2. 범위](paas-ta-container-_v2.0.md#12)
* [1.3. 시스템 구성도](paas-ta-container-_v2.0.md#13)
* [1.4. 참고 자료](paas-ta-container-_v2.0.md#14)

[2. Container 서비스팩 설치](paas-ta-container-_v2.0.md#2)

* [2.1. 설치 전 준비사항](paas-ta-container-_v2.0.md#21)
* [2.1.1. Deployment 및 Release 파일 다운로드](paas-ta-container-_v2.0.md#211)
* [2.2. Stemcell 업로드](paas-ta-container-_v2.0.md#22)
* [2.3. Container 서비스 릴리즈 Deployment 파일 수정 및 배포](paas-ta-container-_v2.0.md#23)
* [2.4. Container 서비스 브로커 등록](paas-ta-container-_v2.0.md#24)
* [2.5. Container 서비스 UAA Client Id 등록](paas-ta-container-_v2.0.md#25)
* [2.6. Container 서비스 Private image Repository 설정 \(Optional\)](paas-ta-container-_v2.0.md#26)

## 1. 문서 개요

### 1.1. 목적

본 문서\(Container 서비스팩 설치 가이드\)는 개방형 PaaS 플랫폼 고도화 및 개발자 지원 환경 기반의 Open PaaS에서 제공되는 서비스팩인 Container 서비스팩을 Bosh를 이용하여 설치 및 서비스 등록하는 방법을 기술하였다.

PaaS-TA 3.5 버전부터는 Bosh 2.0 기반으로 배포\(deploy\)를 진행한다. 기존 Bosh 1.0 기반으로 설치를 원할 경우에는 PaaS-TA 3.1 이하 버전의 문서를 참고한다.

### 1.2. 범위

설치 범위는 Container 서비스팩을 검증하기 위한 기본 설치를 기준으로 작성하였다.

### 1.3. 시스템 구성도

본 문서의 설치된 시스템 구성도이다. Container 서비스 Server, Container 서비스 브로커로 최소사항을 구성하였다.

![](../../../.gitbook/assets/Container_Service_Architecture%20%285%29.png)

| VM명 | 인스턴스수 | vCPU 수 | 메모리\(GB\) | 디스크\(GB\) |
| :--- | :--- | :--- | :--- | :--- |
| master | 1 | 1 | 4G | Root 4G + 영구디스크 20G |
| worker | N | 8 | 16G | Root 4G + 영구디스크 50G |
| container-service-api  | N | 1 | 1G | Root 4G |
| container-service-common-api | N | 1 | 1G | Root 4G |
| container-service-broker | N | 1 | 1G | Root 4G |
| container-service-dashboard | 1 | 1 | 1G | Root 4G |
| DBMS \(MariaDB\) | 1 | 1 | 2G | Root 4G + 영구디스크 20G |
| HAProxy | 1 | 1 | 2G | Root 4G |

### 1.4. 참고 자료

> [http://bosh.io/docs](http://bosh.io/docs) [http://docs.cloudfoundry.org](http://docs.cloudfoundry.org)

## 2. Container 서비스팩 설치

### 2.1. 설치 전 준비사항

본 설치 가이드는 Linux 환경에서 설치하는 것을 기준으로 하였다. 서비스팩 설치를 위해서는 먼저 BOSH CLI v2가 설치되어 있어야 하고 BOSH에 로그인이 되어 있어야 한다.  
 BOSH CLI v2가 설치 되어 있지 않을 경우, 먼저 BOSH 2.0 설치 가이드 문서를 참고하여 BOSH CLI v2를 설치를 하고 사용법을 숙지해야한다.  


* Container 서비스팩 설치 전 Bosh 2.0 배포 주의사항

> IaaS 환경이 OPENSTACK 인 경우 bosh deploy 시 /home/{user\_name}/workspace/paasta-4.6/deployment/bosh-deployment/openstack/disable-readable-vm-names.yml 파일을 옵션으로 추가한 후 배포한다.

* BOSH 2.0 사용자 가이드

  > BOSH 2 사용자 가이드 : [https://github.com/PaaS-TA/Guide-4.0-ROTELLE/blob/master/PaaS-TA\_BOSH2\_사용자\_가이드v1.0.md](https://github.com/PaaS-TA/Guide-4.0-ROTELLE/blob/master/PaaS-TA_BOSH2_사용자_가이드v1.0.md)
  >
  > BOSH CLI V2 사용자 가이드 : [https://github.com/PaaS-TA/Guide-4.0-ROTELLE/blob/master/Use-Guide/Bosh/PaaS-TA\_BOSH\_CLI\_V2\_사용자\_가이드v1.0.md](https://github.com/PaaS-TA/Guide-4.0-ROTELLE/blob/master/Use-Guide/Bosh/PaaS-TA_BOSH_CLI_V2_사용자_가이드v1.0.md)

#### 2.1.1. Container 서비스 Deployment 및 Release 파일 다운로드

Container 서비스 설치에 필요한 Deployment 및 릴리즈 파일을 다운로드 받아 서비스 설치 작업 경로로 위치시킨다.

* 설치 파일 다운로드 위치 : [https://paas-ta.kr/download/package](https://paas-ta.kr/download/package)
* Release, deployment 파일은 /home/{user\_name}/workspace/paasta-4.6 이하에 다운로드 받아야 한다.
* 설치 작업 경로 생성 및 파일 다운로드

> Deployment 파일
>
> > paasta-container-service-2.0
>
> Release 파일
>
> > bosh-dns-release-1.12.0.tgz  
> >  bpm-release-1.0.4.tgz  
> >  cfcr-etcd-release-1.11.1.tgz  
> >  docker-35.2.1.tgz  
> >  kubo-release-0.34.1.tgz  
> >  paasta-container-service-projects-release-1.0.tgz  
> >  private-image-repository-release-1.0.tgz

```text
- Deployment 다운로드 파일 위치 경로 생성
$ mkdir -p ~/workspace/paasta-4.6/deployment/service-deployment/paasta-container-service-2.0

- 릴리즈 다운로드 파일 위치 경로 생성
$ mkdir -p ~/workspace/paasta-4.6/release/service
```

* Deployment 파일을 다운로드 받아 ~/workspace/paasta-4.6/deployment/service-deployment/paasta-container-service-2.0 이하 디렉토리에 이동한다.
* Release 파일을 다운로드 받아 ~/workspace/paasta-4.6/release/service 이하 디렉토리에 이동한다.

### 2.2. Stemcell 업로드

* Deploy시 사용할 Stemcell을 확인한다.

> Stemcell 목록이 존재 하지 않을 경우, BOSH 설치 가이드 문서를 참고하여 Stemcell을 업로드를 해야 한다. \(Stemcell 315.41 버전 사용, PaaSTA-Stemcell.zip\)

* Stemcell 다운로드 위치

  > [https://paas-ta.kr/download/package](https://paas-ta.kr/download/package)

```text
- **사용 예시**

        $ bosh -e micro-bosh stemcells
        Name                                       Version  OS             CPI  CID  
        bosh-openstack-kvm-ubuntu-xenial-go_agent  315.41*  ubuntu-xenial  -    fb08e389-2350-4091-9b29-41743495e62c  
        ~                                          315.36*  ubuntu-xenial  -    7076cf5d-a473-4c46-b6c1-4a7813911f76  

        (*) Currently deployed

        2 stemcells

        Succeeded
```

### 2.3. Container 서비스 릴리즈 Deployment 파일 수정 및 배포

BOSH Deployment manifest는 Components 요소 및 배포의 속성을 정의한 YAML 파일이다.

Deployment 파일에서 사용하는 network, vm\_type 등은 Cloud config를 활용하고 해당 가이드는 BOSH 2.0 가이드를 참고한다.

* Cloud config 내용 조회

```text
inception@inception:~$ bosh cloud-config
Using environment '10.30.50.1' as client 'admin'

azs:
- cloud_properties:
    datacenters:
    - clusters:
      - BD-HA:
          resource_pool: PaaS_TA_46_Pools
      name: BD-HA
  name: z1
- cloud_properties:
    datacenters:
    - clusters:
      - BD-HA:
          resource_pool: PaaS_TA_46_Pools
      name: BD-HA
  name: z2
- cloud_properties:
    datacenters:
    - clusters:
      - BD-HA:
          resource_pool: PaaS_TA_46_Pools
      name: BD-HA
  name: z3
- cloud_properties:
    datacenters:
    - clusters:
      - BD-HA:
          resource_pool: PaaS_TA_46_Pools
      name: BD-HA
  name: z4
- cloud_properties:
    datacenters:
    - clusters:
      - BD-HA:
          resource_pool: PaaS_TA_46_Pools
      name: BD-HA
  name: z5
- cloud_properties:
    datacenters:
    - clusters:
      - BD-HA:
          resource_pool: PaaS_TA_46_Pools
      name: BD-HA
  name: z6
- cloud_properties:
    datacenters:
    - clusters:
      - BD-HA:
          resource_pool: PaaS_TA_46_Pools
      name: BD-HA
  name: z7
compilation:
  az: z1
  network: default
  reuse_compilation_vms: true
  vm_type: large
  workers: 5
disk_types:
- disk_size: 1024
  name: default
- disk_size: 1024
  name: 1GB
- disk_size: 2048
  name: 2GB
- disk_size: 4096
  name: 4GB
- disk_size: 5120
  name: 5GB
- disk_size: 8192
  name: 8GB
- disk_size: 10240
  name: 10GB
- disk_size: 20480
  name: 20GB
- disk_size: 30720
  name: 30GB
- disk_size: 51200
  name: 50GB
- disk_size: 102400
  name: 100GB
- disk_size: 1048576
  name: 1TB
networks:
- name: default
  subnets:
  - azs:
    - z1
    - z2
    - z3
    - z4
    - z5
    - z6
    - z7
    cloud_properties:
      name: Internal
    dns:
    - 8.8.8.8
    gateway: 10.30.20.23
    range: 10.30.0.0/16
    reserved:
    - 10.30.0.0 - 10.30.50.10
    - 10.30.51.0 - 10.30.54.255
    - 10.30.56.0 - 10.30.255.255
    static:
    - 10.30.55.0 - 10.30.55.255
  type: manual
- name: service_private
  subnets:
  - azs:
    - z1
    - z2
    - z3
    - z4
    - z5
    - z6
    - z7
    cloud_properties:
      name: Internal
    dns:
    - 8.8.8.8
    gateway: 10.30.20.23
    range: 10.30.0.0/16
    reserved:
    - 10.30.0.0 - 10.30.51.255
    - 10.30.55.0 - 10.30.55.255
    - 10.30.150.0 - 10.30.255.255
    static:
    - 10.30.52.0 - 10.30.54.255
  type: manual
- name: service_public
  subnets:
  - azs:
    - z1
    - z2
    - z3
    - z4
    - z5
    - z6
    - z7
    cloud_properties:
      name: External
    dns:
    - 8.8.8.8
    gateway: 115.68.46.177
    range: 115.68.46.176/28
    reserved:
    - 115.68.46.191 - 115.68.46.191
    static:
    - 115.68.46.178 - 115.68.46.190
  type: manual
vm_extensions:
- cloud_properties:
    ports:
    - host: 3306
  name: mysql-proxy-lb
- name: cf-router-network-properties
- name: cf-tcp-router-network-properties
- name: diego-ssh-proxy-network-properties
- name: cf-haproxy-network-properties
- cloud_properties:
    disk: 51200
  name: small-50GB
- cloud_properties:
    disk: 102400
  name: small-highmem-100GB
vm_types:
- cloud_properties:
    cpu: 1
    disk: 8192
    ram: 1024
  name: minimal
- cloud_properties:
    cpu: 1
    disk: 10240
    ram: 2048
  name: default
- cloud_properties:
    cpu: 1
    disk: 20480
    ram: 4096
  name: small
- cloud_properties:
    cpu: 2
    disk: 20480
    ram: 4096
  name: medium
- cloud_properties:
    cpu: 2
    disk: 20480
    ram: 8192
  name: medium-memory-8GB
- cloud_properties:
    cpu: 4
    disk: 20480
    ram: 8192
  name: large
- cloud_properties:
    cpu: 8
    disk: 20480
    ram: 16384
  name: xlarge
- cloud_properties:
    cpu: 2
    disk: 51200
    ram: 4096
  name: small-50GB
- cloud_properties:
    cpu: 2
    disk: 51200
    ram: 4096
  name: small-50GB-ephemeral-disk
- cloud_properties:
    cpu: 4
    disk: 102400
    ram: 8192
  name: small-100GB-ephemeral-disk
- cloud_properties:
    cpu: 4
    disk: 102400
    ram: 8192
  name: small-highmem-100GB-ephemeral-disk
- cloud_properties:
    cpu: 8
    disk: 20480
    ram: 16384
  name: small-highmem-16GB
- cloud_properties:
    cpu: 1
    disk: 4096
    ram: 2048
  name: caas_small
- cloud_properties:
    cpu: 1
    disk: 4096
    ram: 1024
  name: caas_small_api
- cloud_properties:
    cpu: 1
    disk: 4096
    ram: 4096
  name: caas_medium
- cloud_properties:
    cpu: 1
    disk: 4096
    ram: 256
  name: service_tiny
- cloud_properties:
    cpu: 1
    disk: 4096
    ram: 512
  name: service_small
- cloud_properties:
    cpu: 1
    disk: 4096
    ram: 1024
  name: service_medium
- cloud_properties:
    cpu: 1
    disk: 4096
    ram: 2048
  name: service_medium_1CPU_2G
- cloud_properties:
    cpu: 2
    disk: 8192
    ram: 4096
  name: service_medium_4G
- cloud_properties:
    cpu: 2
    disk: 10240
    ram: 2048
  name: service_medium_2G
- cloud_properties:
    cpu: 1
    disk: 4096
    ram: 256
  name: portal_tiny
- cloud_properties:
    cpu: 1
    disk: 4096
    ram: 512
  name: portal_small
- cloud_properties:
    cpu: 1
    disk: 4096
    ram: 1024
  name: portal_medium
- cloud_properties:
    cpu: 1
    disk: 4096
    ram: 2048
  name: portal_large

Succeeded
inception@inception:~$
```

* Deployment 를 하기 전에 remove-all-addons.sh 을 환경에 맞게 수정한다.

  \`\`\`

  $ cd ~/workspace/paasta-4.6/deployment/service-deployment/paasta-container-service-2.0

  $ vi remove-all-addons.sh

## !/bin/bash

director\_name='micro-bosh'

bosh -e ${director\_name} update-runtime-config manifests/ops-files/paasta-container-service/remove-all-addons.yml

```text
- Deployment YAML에서 사용하는 변수들을 서버 환경에 맞게 수정한다.

> vSphere용
```

$ cd ~/workspace/paasta-4.6/deployment/service-deployment/paasta-container-service-2.0 $ vi ./manifests/paasta-container-service-vars-vsphere.yml

## INCEPTION OS USER NAME

inception\_os\_user\_name: "inception"

## RELEASE

caas\_projects\_release\_name: "paasta-container-service-projects-release" caas\_projects\_release\_version: "1.0"

## IAAS

vcenter\_master\_user: "" vcenter\_master\_password: "" vcenter\_ip: "" vcenter\_dc: "" vcenter\_ds: "" vcenter\_vms: ""

## STEMCELL

stemcell\_os: "ubuntu-trusty" stemcell\_version: "315.41" stemcell\_alias: "xenial"

## VM\_TYPE

vm\_type\_small: "small" vm\_type\_small\_highmem\_16GB: "small-highmem-16GB" vm\_type\_caas\_small: "caas\_small" vm\_type\_caas\_small\_api: "caas\_small\_api"

## NETWORK

service\_private\_networks\_name: "service\_private" service\_public\_networks\_name: "service\_public"

## IPS

caas\_master\_public\_url: "115.68.47.178" \# CAAS-MASTER-PUBLIC-URL haproxy\_public\_url: "115.68.47.179" \# HAPROXY-PUBLIC-IPS

## CREDHUB

credhub\_server\_url: "10.30.40.111:8844" \# Bosh credhub server URL credhub\_admin\_client\_secret: ""

## CF

cf\_uaa\_oauth\_uri: "[https://uaa](https://uaa)." cf\_api\_url: "[https://api](https://api)." cf\_uaa\_oauth\_client\_id: "" cf\_uaa\_oauth\_client\_secret: ""

## HAPROXY

haproxy\_http\_port: 8080 haproxy\_azs: \[z1\]

## MARIADB

mariadb\_port: "" mariadb\_azs: \[z2\] mariadb\_persistent\_disk\_type: "10GB" mariadb\_admin\_user\_id: "" mariadb\_admin\_user\_password: "" mariadb\_role\_set\_administrator\_code\_name: "Administrator" mariadb\_role\_set\_administrator\_code: "RS0001" mariadb\_role\_set\_regular\_user\_code\_name: "Regular User" mariadb\_role\_set\_regular\_user\_code: "RS0002" mariadb\_role\_set\_init\_user\_code\_name: "Init User" mariadb\_role\_set\_init\_user\_code: "RS0003"

## DASHBOARD

caas\_dashboard\_instances: 1 caas\_dashboard\_port: 8091 caas\_dashboard\_azs: \[z3\] caas\_dashboard\_management\_security\_enabled: false caas\_dashboard\_logging\_level: "INFO"

## API

caas\_api\_instances: 1 caas\_api\_port: 3333 caas\_api\_azs: \[z1\] caas\_api\_management\_security\_enabled: false caas\_api\_logging\_level: "INFO"

## COMMON API

caas\_common\_api\_instances: 1 caas\_common\_api\_port: 3334 caas\_common\_api\_azs: \[z2\] caas\_common\_api\_logging\_level: "INFO"

## SERVICE BROKER

caas\_service\_broker\_instances: 1 caas\_service\_broker\_port: 8888 caas\_service\_broker\_azs: \[z3\]

## ADDON

caas\_apply\_addons\_azs: \[z1\]

## MASTER

caas\_master\_backend\_port: 8443 caas\_master\_port: 8443 caas\_master\_azs: \[z2\] caas\_master\_persistent\_disk\_type: 5120

## WORKER

caas\_worker\_instances: 3 caas\_worker\_azs: \[z1,z2,z3\]

```text
> AWS용
```

$ cd ~/workspace/paasta-4.6/deployment/service-deployment/paasta-container-service-2.0 $ vi ./manifests/paasta-container-service-vars-aws.yml

## INCEPTION OS USER NAME

inception\_os\_user\_name: "ubuntu"

## RELEASE

caas\_projects\_release\_name: "paasta-container-service-projects-release" caas\_projects\_release\_version: "1.0"

## IAAS

aws\_access\_key\_id\_master: '' aws\_secret\_access\_key\_master: '' aws\_access\_key\_id\_worker: '' aws\_secret\_access\_key\_worker: '' kubernetes\_cluster\_tag: 'kubernetes' \# Do not update!

## STEMCELL

stemcell\_os: "ubuntu-trusty" stemcell\_version: "315.41" stemcell\_alias: "xenial"

## VM\_TYPE

vm\_type\_small: "caas\_small" vm\_type\_small\_highmem\_16GB: "caas\_small\_highmem" vm\_type\_caas\_small: "small" vm\_type\_caas\_small\_api: "minimal"

## NETWORK

service\_private\_nat\_networks\_name: "default" service\_private\_networks\_name: "service\_private" service\_public\_networks\_name: "service\_public"

## IPS

caas\_master\_public\_url: "52.78.21.76" \# CAAS-MASTER-PUBLIC-URL haproxy\_public\_url: "54.180.13.40" \# HAPROXY-PUBLIC-IPS

## CREDHUB

credhub\_server\_url: "10.0.1.6:8844" \# Bosh credhub server URL credhub\_admin\_client\_secret: ""

## CF

cf\_uaa\_oauth\_uri: "[https://uaa](https://uaa)." cf\_api\_url: "[https://api](https://api)." cf\_uaa\_oauth\_client\_id: "" cf\_uaa\_oauth\_client\_secret: ""

## HAPROXY

haproxy\_http\_port: 8080 haproxy\_azs: \[z1\]

## MARIADB

mariadb\_port: "" mariadb\_azs: \[z2\] mariadb\_persistent\_disk\_type: "10GB" mariadb\_admin\_user\_id: "" mariadb\_admin\_user\_password: "" mariadb\_role\_set\_administrator\_code\_name: "Administrator" mariadb\_role\_set\_administrator\_code: "RS0001" mariadb\_role\_set\_regular\_user\_code\_name: "Regular User" mariadb\_role\_set\_regular\_user\_code: "RS0002" mariadb\_role\_set\_init\_user\_code\_name: "Init User" mariadb\_role\_set\_init\_user\_code: "RS0003"

## DASHBOARD

caas\_dashboard\_instances: 1 caas\_dashboard\_port: 8091 caas\_dashboard\_azs: \[z3\] caas\_dashboard\_management\_security\_enabled: false caas\_dashboard\_logging\_level: "INFO"

## API

caas\_api\_instances: 1 caas\_api\_port: 3333 caas\_api\_azs: \[z1\] caas\_api\_management\_security\_enabled: false caas\_api\_logging\_level: "INFO"

## COMMON API

caas\_common\_api\_instances: 1 caas\_common\_api\_port: 3334 caas\_common\_api\_azs: \[z2\] caas\_common\_api\_logging\_level: "INFO"

## SERVICE BROKER

caas\_service\_broker\_instances: 1 caas\_service\_broker\_port: 8888 caas\_service\_broker\_azs: \[z3\]

## ADDON

caas\_apply\_addons\_azs: \[z1\]

## MASTER

caas\_master\_backend\_port: 8443 caas\_master\_port: 8443 caas\_master\_azs: \[z2\] caas\_master\_persistent\_disk\_type: 5120

## WORKER

caas\_worker\_instances: 3 caas\_worker\_azs: \[z1,z2,z3\]

```text
> OpenStack용
```

$ cd ~/workspace/paasta-4.6/deployment/service-deployment/paasta-container-service-2.0 $ vi ./manifests/paasta-container-service-vars-openstack.yml

## INCEPTION OS USER NAME

inception\_os\_user\_name: "ubuntu"

## RELEASE

caas\_projects\_release\_name: "paasta-container-service-projects-release" caas\_projects\_release\_version: "1.0"

## IAAS

auth\_url: 'http://:5000/v3' openstack\_domain: '' openstack\_username: '' openstack\_password: '' openstack\_project\_id: '' region: '' ignore-volume-az: true

## STEMCELL

stemcell\_os: "ubuntu-trusty" stemcell\_version: "315.41" stemcell\_alias: "xenial"

## VM\_TYPE

vm\_type\_small: "small" vm\_type\_small\_highmem\_16GB: "small-highmem-16GB" vm\_type\_caas\_small: "small" vm\_type\_caas\_small\_api: "minimal"

## NETWORK

service\_private\_networks\_name: "default" service\_public\_networks\_name: "vip"

## IPS

caas\_master\_public\_url: "115.68.151.178" \# CAAS-MASTER-PUBLIC-URL haproxy\_public\_url: "115.68.151.177" \# HAPROXY-PUBLIC-IPS

## CREDHUB

credhub\_server\_url: "10.20.0.7:8844" \# Bosh credhub server URL credhub\_admin\_client\_secret: ""

## CF

cf\_uaa\_oauth\_uri: "[https://uaa](https://uaa)." cf\_api\_url: "[https://api](https://api)." cf\_uaa\_oauth\_client\_id: "" cf\_uaa\_oauth\_client\_secret: ""

## HAPROXY

haproxy\_http\_port: 8080 haproxy\_azs: \[z1\]

## MARIADB

mariadb\_port: "" mariadb\_azs: \[z2\] mariadb\_persistent\_disk\_type: "10GB" mariadb\_admin\_user\_id: "" mariadb\_admin\_user\_password: "" mariadb\_role\_set\_administrator\_code\_name: "Administrator" mariadb\_role\_set\_administrator\_code: "RS0001" mariadb\_role\_set\_regular\_user\_code\_name: "Regular User" mariadb\_role\_set\_regular\_user\_code: "RS0002" mariadb\_role\_set\_init\_user\_code\_name: "Init User" mariadb\_role\_set\_init\_user\_code: "RS0003"

## DASHBOARD

caas\_dashboard\_instances: 1 caas\_dashboard\_port: 8091 caas\_dashboard\_azs: \[z3\] caas\_dashboard\_management\_security\_enabled: false caas\_dashboard\_logging\_level: "INFO"

## API

caas\_api\_instances: 1 caas\_api\_port: 3333 caas\_api\_azs: \[z1\] caas\_api\_management\_security\_enabled: false caas\_api\_logging\_level: "INFO"

## COMMON API

caas\_common\_api\_instances: 1 caas\_common\_api\_port: 3334 caas\_common\_api\_azs: \[z2\] caas\_common\_api\_logging\_level: "INFO"

## SERVICE BROKER

caas\_service\_broker\_instances: 1 caas\_service\_broker\_port: 8888 caas\_service\_broker\_azs: \[z3\]

## ADDON

caas\_apply\_addons\_azs: \[z1\]

## MASTER

caas\_master\_backend\_port: 8443 caas\_master\_port: 8443 caas\_master\_azs: \[z2\] caas\_master\_persistent\_disk\_type: 5120

## WORKER

caas\_worker\_instances: 3 caas\_worker\_azs: \[z1,z2,z3\]

```text
> GCP용
```

$ cd ~/workspace/paasta-4.6/deployment/service-deployment/paasta-container-service-2.0 $ vi ./manifests/paasta-container-service-vars-gcp.yml

## INCEPTION OS USER NAME

inception\_os\_user\_name: "inception"

## REQUIRED FILE PATH VARIABLE

paasta\_version: "4.6"

## RELEASE

caas\_projects\_release\_name: "paasta-container-service-projects-release" caas\_projects\_release\_version: "1.0" cfcr\_release\_name: "kubo-release" cfcr\_release\_version: "0.34.1"

## IAAS

project\_id: "" network: "" director\_name: "" deployment\_name: ""

## STEMCELL

stemcell\_os: "ubuntu-xenial" stemcell\_version: "315.41" stemcell\_alias: "xenial"

## VM\_TYPE

vm\_type\_small: "small" vm\_type\_small\_highmem\_16GB: "small-highmem-16GB" vm\_type\_caas\_small: "small" vm\_type\_caas\_small\_api: "minimal"

## NETWORK

service\_private\_nat\_networks\_name: "default" service\_private\_networks\_name: "default" service\_public\_networks\_name: "vip"

## IPS

caas\_master\_private\_url: "10.174.6.10" \#"10.174.1.12" \#"10.174.5.10" "10.174.0.11" caas\_master\_public\_url: "34.97.75.151" haproxy\_public\_url: "34.97.167.130" \# HAPROXY-PUBLIC-IPS \(public ip : 34.97.167.130,34.97.75.151\)

## CREDHUB

credhub\_server\_url: "10.174.0.3:8844" \# Bosh credhub server URL credhub\_admin\_client\_secret: ""

## CF

cf\_uaa\_oauth\_uri: "" cf\_api\_url: "" cf\_uaa\_oauth\_client\_id: "" cf\_uaa\_oauth\_client\_secret: ""

## HAPROXY

haproxy\_http\_port: 8080 haproxy\_azs: \[z7\]

## MARIADB

mariadb\_port: "" mariadb\_azs: \[z7\] mariadb\_persistent\_disk\_type: "10GB" mariadb\_admin\_user\_id: "" mariadb\_admin\_user\_password: "" mariadb\_role\_set\_administrator\_code\_name: "Administrator" mariadb\_role\_set\_administrator\_code: "RS0001" mariadb\_role\_set\_regular\_user\_code\_name: "Regular User" mariadb\_role\_set\_regular\_user\_code: "RS0002" mariadb\_role\_set\_init\_user\_code\_name: "Init User" mariadb\_role\_set\_init\_user\_code: "RS0003"

## DASHBOARD

caas\_dashboard\_instances: 1 caas\_dashboard\_port: 8091 caas\_dashboard\_azs: \[z7\] caas\_dashboard\_management\_security\_enabled: false caas\_dashboard\_logging\_level: "INFO"

## API

caas\_api\_instances: 1 caas\_api\_port: 3333 caas\_api\_azs: \[z7\] caas\_api\_management\_security\_enabled: false caas\_api\_logging\_level: "INFO"

## COMMON API

caas\_common\_api\_instances: 1 caas\_common\_api\_port: 3334 caas\_common\_api\_azs: \[z7\] caas\_common\_api\_logging\_level: "INFO"

## SERVICE BROKER

caas\_service\_broker\_instances: 1 caas\_service\_broker\_port: 8888 caas\_service\_broker\_azs: \[z7\]

## ADDON

caas\_apply\_addons\_azs: \[z6\]

## MASTER

caas\_master\_backend\_port: 8443 caas\_master\_port: 8443 caas\_master\_azs: \[z5\] caas\_master\_persistent\_disk\_type: 5120

## WORKER

caas\_worker\_instances: 3 caas\_worker\_azs: \[z5,z6,z7\]

```text
> Azure용
```

$ cd ~/workspace/paasta-4.6/deployment/service-deployment/paasta-container-service-2.0 $ vi ./manifests/paasta-container-service-vars-azure.yml

## INCEPTION OS USER NAME

inception\_os\_user\_name: "ubuntu"

## REQUIRED FILE PATH VARIABLE

paasta\_version: "4.6"

## RELEASE

caas\_projects\_release\_name: "paasta-container-service-projects-release" caas\_projects\_release\_version: "1.0"

## IAAS

azure\_cloud\_name: "" location: "" primary\_availability\_set: "" resource\_group\_name: "" default\_security\_group: "" subnet\_name: "" subscription\_id: "" tenant\_id: "" vnet\_name: "" vnet\_resource\_group\_name: ""

## kubernetes\_cluster\_tag: ""

## STEMCELL

stemcell\_os: "ubuntu-xenial" stemcell\_version: "315.41" stemcell\_alias: "xenial"

## VM\_TYPE

vm\_type\_small: "small" vm\_type\_small\_highmem\_16GB: "small-highmem-16GB" vm\_type\_caas\_small: "small" \#"caas\_small" vm\_type\_caas\_small\_api: "small" \#"caas\_small"

## NETWORK

service\_private\_nat\_networks\_name: "default" service\_private\_networks\_name: "default" \# "service\_private" service\_public\_networks\_name: "vip" \# "service\_public"

## IPS

caas\_master\_private\_url: "10.0.0.201" caas\_master\_public\_url: "52.141.1.239" haproxy\_public\_url: "52.231.8.122"

## CREDHUB

credhub\_server\_url: "10.0.1.6:8844" credhub\_admin\_client\_secret: ""

## CF

cf\_uaa\_oauth\_uri: "[https://uaa](https://uaa)." cf\_api\_url: "[https://api](https://api)." cf\_uaa\_oauth\_client\_id: "" cf\_uaa\_oauth\_client\_secret: ""

## HAPROXY

haproxy\_http\_port: 8080 haproxy\_azs: \[z7\]

## MARIADB

mariadb\_port: "" mariadb\_azs: \[z5\] mariadb\_persistent\_disk\_type: "10GB" mariadb\_admin\_user\_id: "" mariadb\_admin\_user\_password: "" mariadb\_role\_set\_administrator\_code\_name: "Administrator" mariadb\_role\_set\_administrator\_code: "RS0001" mariadb\_role\_set\_regular\_user\_code\_name: "Regular User" mariadb\_role\_set\_regular\_user\_code: "RS0002" mariadb\_role\_set\_init\_user\_code\_name: "Init User" mariadb\_role\_set\_init\_user\_code: "RS0003"

## DASHBOARD

caas\_dashboard\_instances: 1 caas\_dashboard\_port: 8091 caas\_dashboard\_azs: \[z6\] caas\_dashboard\_management\_security\_enabled: false caas\_dashboard\_logging\_level: "INFO"

## API

caas\_api\_instances: 1 caas\_api\_port: 3333 caas\_api\_azs: \[z6\] caas\_api\_management\_security\_enabled: false caas\_api\_logging\_level: "INFO"

## COMMON API

caas\_common\_api\_instances: 1 caas\_common\_api\_port: 3334 caas\_common\_api\_azs: \[z6\] caas\_common\_api\_logging\_level: "INFO"

## SERVICE BROKER

caas\_service\_broker\_instances: 1 caas\_service\_broker\_port: 8888 caas\_service\_broker\_azs: \[z6\]

## ADDON

caas\_apply\_addons\_azs: \[z5\]

## MASTER

caas\_master\_backend\_port: "8443" caas\_master\_port: "8443" caas\_master\_azs: \[z7\] caas\_master\_persistent\_disk\_type: 5120

## WORKER

caas\_worker\_instances: 3 caas\_worker\_azs: \[z4,z5,z6\]

```text
- Deploy 스크립트 파일을 서버 환경에 맞게 수정한다.
  - vSphere : **deploy_vsphere.sh**
  - AWS : **deploy_aws.sh**
  - OpenStack : **deploy_openstack.sh**
  - GCP : **deploy_gcp.sh**
  - Azure : **deploy_azure.sh**
```

$ cd ~/workspace/paasta-4.6/deployment/service-deployment/paasta-container-service-2.0 $ vi deploy-vsphere.sh

## !/bin/bash

## SET VARIABLES

export CAAS\_DEPLOYMENT\_NAME='paasta-container-service' export CAAS\_BOSH2\_NAME='micro-bosh' export CAAS\_BOSH2\_UUID=`bosh int <(bosh -e ${CAAS_BOSH2_NAME} environment --json) --path=/Tables/0/Rows/0/uuid`

## DEPLOY

bosh -e ${CAAS\_BOSH2\_NAME} -n -d ${CAAS\_DEPLOYMENT\_NAME} deploy --no-redact manifests/paasta-container-service-deployment-vsphere.yml  -l manifests/paasta-container-service-vars-vsphere.yml  -o manifests/ops-files/paasta-container-service/network-vsphere.yml  -o manifests/ops-files/paasta-container-service/add-private-image-repository.yml  -o manifests/ops-files/iaas/vsphere/cloud-provider.yml  -o manifests/ops-files/iaas/vsphere/set-working-dir-no-rp.yml  -o manifests/ops-files/rename.yml  -o manifests/ops-files/misc/single-master.yml  -o manifests/ops-files/misc/first-time-deploy.yml  -v director\_uuid=${CAAS\_BOSH2\_UUID}  -v director\_name=${CAAS\_BOSH2\_NAME}  -v deployment\_name=${CAAS\_DEPLOYMENT\_NAME}

```text
**※    Private Image Repository 를 사용할 경우**
```

* 아래 명령어를 추가한다.
  * network 및 dns, gateway 설정은 각 IaaS 환경의 네트워크 설정에 맞추어 수정한다.

manifests/ops-files/paasta-container-service/add-private-image-repository.yml

```text
- Container 서비스팩을 배포한다.
```

$ cd ~/workspace/paasta-4.6/deployment/service-deployment/paasta-container-service-2.0 $ ./remove-all-addons.sh $ ./deploy-vsphere.sh

Using environment '10.30.50.1' as client 'admin'

Using deployment 'paasta-container-service-02'

Release 'cfcr-etcd/1.11.1' already exists.

Release 'docker/35.2.1' already exists.

Release 'bpm/1.0.4' already exists.

Release 'bosh-dns/1.12.0' already exists.

**\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\# 100.00% 109.40 KiB/s 0s**

Task 140366

Task 140366 \| 08:21:36 \| Extracting release: Extracting release \(00:00:00\) Task 140366 \| 08:21:36 \| Verifying manifest: Verifying manifest \(00:00:00\) Task 140366 \| 08:21:36 \| Resolving package dependencies: Resolving package dependencies \(00:00:00\) Task 140366 \| 08:21:36 \| Processing 1 existing package: Processing 1 existing package \(00:00:00\) Task 140366 \| 08:21:36 \| Processing 1 existing job: Processing 1 existing job \(00:00:00\) Task 140366 \| 08:21:36 \| Release has been created: private-image-repository-release/1.0 \(00:00:00\)

Task 140366 Started Fri Sep 27 08:21:36 UTC 2019 Task 140366 Finished Fri Sep 27 08:21:36 UTC 2019 Task 140366 Duration 00:00:00 Task 140366 done

**\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\# 100.00% 724.79 KiB/s 0s**

Task 140367

Task 140367 \| 08:21:39 \| Extracting release: Extracting release \(00:00:00\) Task 140367 \| 08:21:39 \| Verifying manifest: Verifying manifest \(00:00:00\) Task 140367 \| 08:21:39 \| Resolving package dependencies: Resolving package dependencies \(00:00:00\) Task 140367 \| 08:21:39 \| Processing 13 existing packages: Processing 13 existing packages \(00:00:00\) Task 140367 \| 08:21:39 \| Processing 14 existing jobs: Processing 14 existing jobs \(00:00:00\) Task 140367 \| 08:21:39 \| Release has been created: kubo/0.34.1 \(00:00:00\)

Task 140367 Started Fri Sep 27 08:21:39 UTC 2019 Task 140367 Finished Fri Sep 27 08:21:39 UTC 2019 Task 140367 Duration 00:00:00 Task 140367 done

**\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\# 100.00% 488.36 KiB/s 0s**

Task 140368

Task 140368 \| 08:21:41 \| Extracting release: Extracting release \(00:00:00\) Task 140368 \| 08:21:41 \| Verifying manifest: Verifying manifest \(00:00:00\) Task 140368 \| 08:21:41 \| Resolving package dependencies: Resolving package dependencies \(00:00:00\) Task 140368 \| 08:21:41 \| Processing 7 existing packages: Processing 7 existing packages \(00:00:00\) Task 140368 \| 08:21:41 \| Processing 6 existing jobs: Processing 6 existing jobs \(00:00:00\) Task 140368 \| 08:21:41 \| Release has been created: paasta-container-service-projects-release/1.0 \(00:00:00\)

Task 140368 Started Fri Sep 27 08:21:41 UTC 2019 Task 140368 Finished Fri Sep 27 08:21:41 UTC 2019 Task 140368 Duration 00:00:00 Task 140368 done

* azs:
* * cloud\_properties:
* datacenters:
* * clusters:
* * BD-HA:
* resource\_pool: PaaS\_TA\_46\_Pools
* name: BD-HA
* name: z1
* * cloud\_properties:
* datacenters:
* * clusters:
* * BD-HA:
* resource\_pool: PaaS\_TA\_46\_Pools
* name: BD-HA
* name: z2
* * cloud\_properties:
* datacenters:
* * clusters:
* * BD-HA:
* resource\_pool: PaaS\_TA\_46\_Pools
* name: BD-HA
* name: z3
* * cloud\_properties:
* datacenters:
* * clusters:
* * BD-HA:
* resource\_pool: PaaS\_TA\_46\_Pools
* name: BD-HA
* name: z4
* * cloud\_properties:
* datacenters:
* * clusters:
* * BD-HA:
* resource\_pool: PaaS\_TA\_46\_Pools
* name: BD-HA
* name: z5
* * cloud\_properties:
* datacenters:
* * clusters:
* * BD-HA:
* resource\_pool: PaaS\_TA\_46\_Pools
* name: BD-HA
* name: z6
* * cloud\_properties:
* datacenters:
* * clusters:
* * BD-HA:
* resource\_pool: PaaS\_TA\_46\_Pools
* name: BD-HA
* name: z7
* vm\_types:
* * cloud\_properties:
* cpu: 1
* disk: 8192
* ram: 1024
* name: minimal
* * cloud\_properties:
* cpu: 1
* disk: 10240
* ram: 2048
* name: default
* * cloud\_properties:
* cpu: 1
* disk: 20480
* ram: 4096
* name: small
* * cloud\_properties:
* cpu: 2
* disk: 20480
* ram: 4096
* name: medium
* * cloud\_properties:
* cpu: 2
* disk: 20480
* ram: 8192
* name: medium-memory-8GB
* * cloud\_properties:
* cpu: 4
* disk: 20480
* ram: 8192
* name: large
* * cloud\_properties:
* cpu: 8
* disk: 20480
* ram: 16384
* name: xlarge
* * cloud\_properties:
* cpu: 2
* disk: 51200
* ram: 4096
* name: small-50GB
* * cloud\_properties:
* cpu: 2
* disk: 51200
* ram: 4096
* name: small-50GB-ephemeral-disk
* * cloud\_properties:
* cpu: 4
* disk: 102400
* ram: 8192
* name: small-100GB-ephemeral-disk
* * cloud\_properties:
* cpu: 4
* disk: 102400
* ram: 8192
* name: small-highmem-100GB-ephemeral-disk
* * cloud\_properties:
* cpu: 8
* disk: 20480
* ram: 16384
* name: small-highmem-16GB
* * cloud\_properties:
* cpu: 1
* disk: 4096
* ram: 2048
* name: caas\_small
* * cloud\_properties:
* cpu: 1
* disk: 4096
* ram: 1024
* name: caas\_small\_api
* * cloud\_properties:
* cpu: 1
* disk: 4096
* ram: 4096
* name: caas\_medium
* * cloud\_properties:
* cpu: 1
* disk: 4096
* ram: 256
* name: service\_tiny
* * cloud\_properties:
* cpu: 1
* disk: 4096
* ram: 512
* name: service\_small
* * cloud\_properties:
* cpu: 1
* disk: 4096
* ram: 1024
* name: service\_medium
* * cloud\_properties:
* cpu: 1
* disk: 4096
* ram: 2048
* name: service\_medium\_1CPU\_2G
* * cloud\_properties:
* cpu: 2
* disk: 8192
* ram: 4096
* name: service\_medium\_4G
* * cloud\_properties:
* cpu: 2
* disk: 10240
* ram: 2048
* name: service\_medium\_2G
* * cloud\_properties:
* cpu: 1
* disk: 4096
* ram: 256
* name: portal\_tiny
* * cloud\_properties:
* cpu: 1
* disk: 4096
* ram: 512
* name: portal\_small
* * cloud\_properties:
* cpu: 1
* disk: 4096
* ram: 1024
* name: portal\_medium
* * cloud\_properties:
* cpu: 1
* disk: 4096
* ram: 2048
* name: portal\_large
* vm\_extensions:
* * cloud\_properties:
* ports:
* * host: 
* name: mysql-proxy-lb
* * name: cf-router-network-properties
* * name: cf-tcp-router-network-properties
* * name: diego-ssh-proxy-network-properties
* * name: cf-haproxy-network-properties
* * cloud\_properties:
* disk: 51200
* name: small-50GB
* * cloud\_properties:
* disk: 102400
* name: small-highmem-100GB
* compilation:
* az: z1
* network: default
* reuse\_compilation\_vms: true
* vm\_type: large
* workers: 5
* networks:
* * name: default
* subnets:
* * azs:
* * z1
* * z2
* * z3
* * z4
* * z5
* * z6
* * z7
* cloud\_properties:
* name: Internal
* dns:
* * 8.8.8.8
* gateway: 10.30.20.23
* range: 10.30.0.0/16
* reserved:
* * 10.30.0.0 - 10.30.50.10
* * 10.30.51.0 - 10.30.54.255
* * 10.30.56.0 - 10.30.255.255
* static:
* * 10.30.55.0 - 10.30.55.255
* type: manual
* * name: service\_private
* subnets:
* * azs:
* * z1
* * z2
* * z3
* * z4
* * z5
* * z6
* * z7
* cloud\_properties:
* name: Internal
* dns:
* * 8.8.8.8
* gateway: 10.30.20.23
* range: 10.30.0.0/16
* reserved:
* * 10.30.0.0 - 10.30.51.255
* * 10.30.55.0 - 10.30.55.255
* * 10.30.150.0 - 10.30.255.255
* static:
* * 10.30.52.0 - 10.30.54.255
* type: manual
* * name: service\_public
* subnets:
* * azs:
* * z1
* * z2
* * z3
* * z4
* * z5
* * z6
* * z7
* cloud\_properties:
* name: External
* dns:
* * 8.8.8.8
* gateway: 115.68.46.177
* range: 115.68.46.176/28
* reserved:
* * 115.68.46.191 - 115.68.46.191
* static:
* * 115.68.46.178 - 115.68.46.190
* type: manual
* disk\_types:
* * disk\_size: 1024
* name: default
* * disk\_size: 1024
* name: 1GB
* * disk\_size: 2048
* name: 2GB
* * disk\_size: 4096
* name: 4GB
* * disk\_size: 5120
* name: 5GB
* * disk\_size: 8192
* name: 8GB
* * disk\_size: 10240
* name: 10GB
* * disk\_size: 20480
* name: 20GB
* * disk\_size: 30720
* name: 30GB
* * disk\_size: 51200
* name: 50GB
* * disk\_size: 102400
* name: 100GB
* * disk\_size: 1048576
* name: 1TB
* stemcells:
* * alias: xenial
* os: ubuntu-xenial
* version: '315.41'
* releases:
* * name: kubo
* url: file:///home/inception/workspace/paasta-4.6/release/service/kubo-release-0.34.1.tgz
* version: 0.34.1
* * name: cfcr-etcd
* sha1: 6cffe156fc2eff27805c0129a6763ddad4803947
* url: file:///home/inception/workspace/paasta-4.6/release/service/cfcr-etcd-1.11.1.tgz
* version: 1.11.1
* * name: docker
* sha1: 97a6fba0668e521a5f6dfffa14010ccc0c9308b1
* url: file:///home/inception/workspace/paasta-4.6/release/service/docker-35.2.1.tgz
* version: 35.2.1
* * name: bpm
* sha1: bc1cea52a4e44303ec818e07be4a356ee7da8b1d
* url: file:///home/inception/workspace/paasta-4.6/release/service/bpm-1.0.4.tgz
* version: 1.0.4
* * name: bosh-dns
* sha1: fe0bd8641b29cb78977cb5d4494943138a6067f2
* url: file:///home/inception/workspace/paasta-4.6/release/service/bosh-dns-release-1.12.0.tgz
* version: 1.12.0
* * name: paasta-container-service-projects-release
* url: file:///home/inception/workspace/paasta-4.6/release/service/paasta-container-service-projects-release-1.0.tgz
* version: '1.0'
* * name: private-image-repository-release
* url: file:///home/inception/workspace/paasta-4.6/release/service/private-image-repository-release-1.0.tgz
* version: '1.0'
* update:
* canaries: 1
* canary\_watch\_time: 10000-300000
* max\_in\_flight: 100%
* update\_watch\_time: 10000-300000
* addons:
* * include:
* stemcells:
* * os: ubuntu-xenial
* jobs:
* * name: bosh-dns
* properties:
* api:
* client:
* tls: "\(\(/dns\_api\_client\_tls\)\)"
* server:
* tls: "\(\(/dns\_api\_server\_tls\)\)"
* cache:
* enabled: true
* health:
* client:
* tls: "\(\(/dns\_healthcheck\_client\_tls\)\)"
* enabled: true
* server:
* tls: "\(\(/dns\_healthcheck\_server\_tls\)\)"
* release: bosh-dns
* name: bosh-dns
* * jobs:
* * name: kubo-dns-aliases
* release: kubo
* name: bosh-dns-aliases
* variables:
* * name: kubo-admin-password
* type: password
* * name: kubelet-password
* type: password
* * name: kubelet-drain-password
* type: password
* * name: kube-proxy-password
* type: password
* * name: kube-controller-manager-password
* type: password
* * name: kube-scheduler-password
* type: password
* * name: etcd\_user\_root\_password
* type: password
* * name: etcd\_user\_flanneld\_password
* type: password
* * name: kubo\_ca
* options:
* common\_name: ca
* is\_ca: true
* type: certificate
* * name: tls-kubelet
* options:
* alternative\_names: \[\]
* ca: kubo\_ca
* common\_name: kubelet.cfcr.internal
* organization: system:nodes
* type: certificate
* * name: tls-kubelet-client
* options:
* ca: kubo\_ca
* common\_name: kube-apiserver.cfcr.internal
* extended\_key\_usage:
* * client\_auth
* organization: system:masters
* type: certificate
* * name: tls-kubernetes
* options:
* alternative\_names:
* * 115.68.46.188
* * 10.100.200.1
* * localhost
* * kubernetes
* * kubernetes.default
* * kubernetes.default.svc
* * kubernetes.default.svc.cluster.local
* * master.cfcr.internal
* ca: kubo\_ca
* common\_name: master.cfcr.internal
* organization: system:masters
* type: certificate
* * name: service-account-key
* type: rsa
* * name: tls-kube-controller-manager
* options:
* alternative\_names:
* * localhost
* * 127.0.0.1
* ca: kubo\_ca
* common\_name: kube-controller-manager
* extended\_key\_usage:
* * server\_auth
* key\_usage:
* * digital\_signature
* * key\_encipherment
* type: certificate
* * name: etcd\_ca
* options:
* common\_name: etcd.ca
* is\_ca: true
* type: certificate
* * name: tls-etcd-v0-29-0
* options:
* ca: etcd\_ca
* common\_name: "\*.etcd.cfcr.internal"
* extended\_key\_usage:
* * client\_auth
* * server\_auth
* type: certificate
* * name: tls-etcdctl-v0-29-0
* options:
* ca: etcd\_ca
* common\_name: etcdClient
* extended\_key\_usage:
* * client\_auth
* type: certificate
* * name: tls-etcdctl-root
* options:
* ca: etcd\_ca
* common\_name: root
* extended\_key\_usage:
* * client\_auth
* type: certificate
* * name: tls-etcdctl-flanneld
* options:
* ca: etcd\_ca
* common\_name: flanneld
* extended\_key\_usage:
* * client\_auth
* type: certificate
* * name: tls-metrics-server
* options:
* alternative\_names:
* * metrics-server.kube-system.svc
* ca: kubo\_ca
* common\_name: metrics-server
* type: certificate
* * name: kubernetes-dashboard-ca
* options:
* common\_name: ca
* is\_ca: true
* type: certificate
* * name: tls-kubernetes-dashboard
* options:
* alternative\_names: \[\]
* ca: kubernetes-dashboard-ca
* common\_name: kubernetesdashboard.n
* type: certificate
* * name: "/dns\_healthcheck\_tls\_ca"
* options:
* common\_name: dns-healthcheck-tls-ca
* is\_ca: true
* type: certificate
* * name: "/dns\_healthcheck\_server\_tls"
* options:
* ca: "/dns\_healthcheck\_tls\_ca"
* common\_name: health.bosh-dns
* extended\_key\_usage:
* * server\_auth
* type: certificate
* * name: "/dns\_healthcheck\_client\_tls"
* options:
* ca: "/dns\_healthcheck\_tls\_ca"
* common\_name: health.bosh-dns
* extended\_key\_usage:
* * client\_auth
* type: certificate
* * name: "/dns\_api\_tls\_ca"
* options:
* common\_name: dns-api-tls-ca
* is\_ca: true
* type: certificate
* * name: "/dns\_api\_server\_tls"
* options:
* ca: "/dns\_api\_tls\_ca"
* common\_name: api.bosh-dns
* extended\_key\_usage:
* * server\_auth
* type: certificate
* * name: "/dns\_api\_client\_tls"
* options:
* ca: "/dns\_api\_tls\_ca"
* common\_name: api.bosh-dns
* extended\_key\_usage:
* * client\_auth
* type: certificate
* features:
* use\_dns\_addresses: true
* instance\_groups:
* * azs:
* * z2
* instances: 1
* jobs:
* * consumes:
* cloud-provider:
* from: master-cloud-provider
* name: apply-specs
* properties:
* addons:
* * coredns
* * metrics-server
* * kubernetes-dashboard
* admin-password: "\(\(kubo-admin-password\)\)"
* admin-username: 
* api-token: "\(\(kubelet-password\)\)"
* tls:
* kubernetes: "\(\(tls-kubernetes\)\)"
* kubernetes-dashboard: "\(\(tls-kubernetes-dashboard\)\)"
* metrics-server: "\(\(tls-metrics-server\)\)"
* release: kubo
* lifecycle: errand
* name: apply-addons
* networks:
* * name: default
* stemcell: xenial
* vm\_type: small
* * azs:
* * z3
* instances: 1
* jobs:
* * name: bpm
* release: bpm
* * name: flanneld
* properties:
* tls:
* etcdctl:
* ca: "\(\(tls-etcdctl-flanneld.ca\)\)"
* certificate: "\(\(tls-etcdctl-flanneld.certificate\)\)"
* private\_key: "\(\(tls-etcdctl-flanneld.private\_key\)\)"
* release: kubo
* * consumes:
* cloud-provider:
* from: master-cloud-provider
* name: kube-apiserver
* properties:
* admin-password: "\(\(kubo-admin-password\)\)"
* admin-username: 
* audit-policy:
* apiVersion: audit.k8s.io/v1beta1
* kind: Policy
* rules:
* * level: None
* resources:
* * group: ''
* resources:
* * endpoints
* * services
* * services/status
* users:
* * system:kube-proxy
* verbs:
* * watch
* * level: None
* resources:
* * group: ''
* resources:
* * nodes
* * nodes/status
* users:
* * kubelet
* verbs:
* * get
* * level: None
* resources:
* * group: ''
* resources:
* * nodes
* * nodes/status
* userGroups:
* * system:nodes
* verbs:
* * get
* * level: None
* namespaces:
* * kube-system
* resources:
* * group: ''
* resources:
* * endpoints
* users:
* * system:kube-controller-manager
* * system:kube-scheduler
* * system:serviceaccount:kube-system:endpoint-controller
* verbs:
* * get
* * update
* * level: None
* resources:
* * group: ''
* resources:
* * namespaces
* * namespaces/status
* * namespaces/finalize
* users:
* * system:apiserver
* verbs:
* * get
* * level: None
* resources:
* * group: metrics.k8s.io
* users:
* * system:kube-controller-manager
* verbs:
* * get
* * list
* * level: None
* nonResourceURLs:
* * "/healthz\*"
* * "/version"
* * "/swagger\*"
* * level: None
* resources:
* * group: ''
* resources:
* * events
* * level: Request
* omitStages:
* * RequestReceived
* resources:
* * group: ''
* resources:
* * nodes/status
* * pods/status
* userGroups:
* * system:nodes
* verbs:
* * update
* * patch
* * level: Request
* omitStages:
* * RequestReceived
* users:
* * system:serviceaccount:kube-system:namespace-controller
* verbs:
* * deletecollection
* * level: Metadata
* omitStages:
* * RequestReceived
* resources:
* * group: ''
* resources:
* * secrets
* * configmaps
* * group: authentication.k8s.io
* resources:
* * tokenreviews
* * level: Request
* omitStages:
* * RequestReceived
* resources:
* * group: ''
* * group: admissionregistration.k8s.io
* * group: apiextensions.k8s.io
* * group: apiregistration.k8s.io
* * group: apps
* * group: authentication.k8s.io
* * group: authorization.k8s.io
* * group: autoscaling
* * group: batch
* * group: certificates.k8s.io
* * group: extensions
* * group: metrics.k8s.io
* * group: networking.k8s.io
* * group: policy
* * group: rbac.authorization.k8s.io
* * group: settings.k8s.io
* * group: storage.k8s.io
* verbs:
* * get
* * list
* * watch
* * level: RequestResponse
* omitStages:
* * RequestReceived
* resources:
* * group: ''
* * group: admissionregistration.k8s.io
* * group: apiextensions.k8s.io
* * group: apiregistration.k8s.io
* * group: apps
* * group: authentication.k8s.io
* * group: authorization.k8s.io
* * group: autoscaling
* * group: batch
* * group: certificates.k8s.io
* * group: extensions
* * group: metrics.k8s.io
* * group: networking.k8s.io
* * group: policy
* * group: rbac.authorization.k8s.io
* * group: settings.k8s.io
* * group: storage.k8s.io
* * level: Metadata
* omitStages:
* * RequestReceived
* k8s-args:
* audit-log-maxage: 0
* audit-log-maxbackup: 7
* audit-log-maxsize: 49
* audit-log-path: "/var/vcap/sys/log/kube-apiserver/audit.log"
* audit-policy-file: "/var/vcap/jobs/kube-apiserver/config/audit\_policy.yml"
* authorization-mode: RBAC
* client-ca-file: "/var/vcap/jobs/kube-apiserver/config/kubernetes-ca.pem"
* disable-admission-plugins: \[\]
* enable-admission-plugins: \[\]
* enable-aggregator-routing: true
* enable-bootstrap-token-auth: true
* enable-swagger-ui: true
* etcd-cafile: "/var/vcap/jobs/kube-apiserver/config/etcd-ca.crt"
* etcd-certfile: "/var/vcap/jobs/kube-apiserver/config/etcd-client.crt"
* etcd-keyfile: "/var/vcap/jobs/kube-apiserver/config/etcd-client.key"
* kubelet-client-certificate: "/var/vcap/jobs/kube-apiserver/config/kubelet-client-cert.pem"
* kubelet-client-key: "/var/vcap/jobs/kube-apiserver/config/kubelet-client-key.pem"
* proxy-client-cert-file: "/var/vcap/jobs/kube-apiserver/config/kubernetes.pem"
* proxy-client-key-file: "/var/vcap/jobs/kube-apiserver/config/kubernetes-key.pem"
* requestheader-allowed-names: aggregator
* requestheader-client-ca-file: "/var/vcap/jobs/kube-apiserver/config/kubernetes-ca.pem"
* requestheader-extra-headers-prefix: X-Remote-Extra-
* requestheader-group-headers: X-Remote-Group
* requestheader-username-headers: X-Remote-User
* runtime-config: api/v1
* secure-port: 8443
* service-account-key-file: "/var/vcap/jobs/kube-apiserver/config/service-account-public-key.pem"
* service-cluster-ip-range: 10.100.200.0/24
* storage-media-type: application/json
* tls-cert-file: "/var/vcap/jobs/kube-apiserver/config/kubernetes.pem"
* tls-private-key-file: "/var/vcap/jobs/kube-apiserver/config/kubernetes-key.pem"
* token-auth-file: "/var/vcap/jobs/kube-apiserver/config/tokens.csv"
* v: 2
* kube-controller-manager-password: "\(\(kube-controller-manager-password\)\)"
* kube-proxy-password: "\(\(kube-proxy-password\)\)"
* kube-scheduler-password: "\(\(kube-scheduler-password\)\)"
* kubelet-drain-password: "\(\(kubelet-drain-password\)\)"
* kubelet-password: "\(\(kubelet-password\)\)"
* service-account-public-key: "\(\(service-account-key.public\_key\)\)"
* tls:
* kubelet-client: "\(\(tls-kubelet-client\)\)"
* kubernetes:
* ca: "\(\(tls-kubernetes.ca\)\)"
* certificate: "\(\(tls-kubernetes.certificate\)\)\(\(tls-kubernetes.ca\)\)"
* private\_key: "\(\(tls-kubernetes.private\_key\)\)"
* release: kubo
* * consumes:
* cloud-provider:
* from: master-cloud-provider
* name: kube-controller-manager
* properties:
* api-token: "\(\(kube-controller-manager-password\)\)"
* cluster-signing: "\(\(kubo\_ca\)\)"
* k8s-args:
* cluster-signing-cert-file: "/var/vcap/jobs/kube-controller-manager/config/cluster-signing-ca.pem"
* cluster-signing-key-file: "/var/vcap/jobs/kube-controller-manager/config/cluster-signing-key.pem"
* kubeconfig: "/var/vcap/jobs/kube-controller-manager/config/kubeconfig"
* root-ca-file: "/var/vcap/jobs/kube-controller-manager/config/ca.pem"
* service-account-private-key-file: "/var/vcap/jobs/kube-controller-manager/config/service-account-private-key.pem"
* terminated-pod-gc-threshold: 100
* tls-cert-file: "/var/vcap/jobs/kube-controller-manager/config/kube-controller-manager-cert.pem"
* tls-private-key-file: "/var/vcap/jobs/kube-controller-manager/config/kube-controller-manager-private-key.pem"
* use-service-account-credentials: true
* v: 2
* service-account-private-key: "\(\(service-account-key.private\_key\)\)"
* tls:
* kube-controller-manager: "\(\(tls-kube-controller-manager\)\)"
* kubernetes: "\(\(tls-kubernetes\)\)"
* release: kubo
* * name: kube-scheduler
* properties:
* api-token: "\(\(kube-scheduler-password\)\)"
* kube-scheduler-configuration:
* apiVersion: kubescheduler.config.k8s.io/v1alpha1
* clientConnection:
* kubeconfig: "/var/vcap/jobs/kube-scheduler/config/kubeconfig"
* disablePreemption: false
* kind: KubeSchedulerConfiguration
* tls:
* kubernetes: "\(\(tls-kubernetes\)\)"
* release: kubo
* * consumes:
* cloud-provider:
* from: master-cloud-provider
* name: kubernetes-roles
* properties:
* admin-password: "\(\(kubo-admin-password\)\)"
* admin-username: 
* tls:
* kubernetes: "\(\(tls-kubernetes\)\)"
* release: kubo
* * name: etcd
* properties:
* etcd:
* dns\_suffix: etcd.cfcr.internal
* tls:
* etcd:
* ca: "\(\(etcd\_ca.certificate\)\)"
* certificate: "\(\(tls-etcd-v0-29-0.certificate\)\)"
* private\_key: "\(\(tls-etcd-v0-29-0.private\_key\)\)"
* etcdctl:
* ca: "\(\(tls-etcdctl-v0-29-0.ca\)\)"
* certificate: "\(\(tls-etcdctl-v0-29-0.certificate\)\)"
* private\_key: "\(\(tls-etcdctl-v0-29-0.private\_key\)\)"
* etcdctl-root:
* ca: "\(\(tls-etcdctl-v0-29-0.ca\)\)"
* certificate: "\(\(tls-etcdctl-root.certificate\)\)"
* private\_key: "\(\(tls-etcdctl-root.private\_key\)\)"
* peer:
* ca: "\(\(tls-etcd-v0-29-0.ca\)\)"
* certificate: "\(\(tls-etcd-v0-29-0.certificate\)\)"
* private\_key: "\(\(tls-etcd-v0-29-0.private\_key\)\)"
* users:
* * name: root
* password: "\(\(etcd\_user\_root\_password\)\)"
* versions:
* * v2
* * name: flanneld
* password: "\(\(etcd\_user\_flanneld\_password\)\)"
* permissions:
* read:
* * "/coreos.com/network/\*"
* write:
* * "/coreos.com/network/\*"
* versions:
* * v2
* release: cfcr-etcd
* * name: smoke-tests
* release: kubo
* * name: cloud-provider
* properties:
* cloud-config:
* Disk:
* scsicontrollertype: pvscsi
* Global:
* datacenter: 
* datastore: 
* password: 
* server: 
* user: 
* working-dir: "/BD-DC/vm/PaaS\_TA\_46\_VMs/34bd6ba1-d6cf-41d3-ad52-ee09981671cb"
* cloud-provider:
* type: vsphere
* vsphere:
* working-dir: "/BD-DC/vm/PaaS\_TA\_46\_VMs"
* provides:
* cloud-provider:
* as: master-cloud-provider
* release: kubo
* name: master
* networks:
* * name: service\_private
* * default:
* * dns
* * gateway
* name: service\_public
* static\_ips: 115.68.46.188
* persistent\_disk: 5120
* stemcell: xenial
* vm\_type: small
* * azs:
* * z1
* * z2
* * z3
* instances: 3
* jobs:
* * name: flanneld
* properties:
* tls:
* etcdctl:
* ca: "\(\(tls-etcdctl-flanneld.ca\)\)"
* certificate: "\(\(tls-etcdctl-flanneld.certificate\)\)"
* private\_key: "\(\(tls-etcdctl-flanneld.private\_key\)\)"
* release: kubo
* * name: docker
* properties:
* bridge: cni0
* default\_ulimits:
* * nofile=1048576
* env: {}
* flannel: true
* ip\_masq: false
* iptables: false
* live\_restore: true
* log\_level: error
* log\_options:
* * max-size=128m
* * max-file=2
* storage\_driver: overlay2
* store\_dir: "/var/vcap/data"
* release: docker
* * name: kubernetes-dependencies
* release: kubo
* * name: kubelet
* properties:
* api-token: "\(\(kubelet-password\)\)"
* cloud-provider: vsphere
* drain-api-token: "\(\(kubelet-drain-password\)\)"
* k8s-args:
* cni-bin-dir: "/var/vcap/jobs/kubelet/packages/cni/bin"
* container-runtime: docker
* docker: unix:///var/vcap/sys/run/docker/docker.sock
* docker-endpoint: unix:///var/vcap/sys/run/docker/docker.sock
* kubeconfig: "/var/vcap/jobs/kubelet/config/kubeconfig"
* network-plugin: cni
* root-dir: "/var/vcap/data/kubelet"
* kubelet-configuration:
* apiVersion: kubelet.config.k8s.io/v1beta1
* authentication:
* anonymous:
* enabled: true
* x509:
* clientCAFile: "/var/vcap/jobs/kubelet/config/kubelet-client-ca.pem"
* authorization:
* mode: Webhook
* clusterDNS:
* * 10.100.200.10
* clusterDomain: cluster.local
* failSwapOn: false
* kind: KubeletConfiguration
* serializeImagePulls: false
* tlsCertFile: "/var/vcap/jobs/kubelet/config/kubelet.pem"
* tlsPrivateKeyFile: "/var/vcap/jobs/kubelet/config/kubelet-key.pem"
* tls:
* kubelet:
* ca: "\(\(tls-kubelet.ca\)\)"
* certificate: "\(\(tls-kubelet.certificate\)\)\(\(tls-kubelet.ca\)\)"
* private\_key: "\(\(tls-kubelet.private\_key\)\)"
* kubelet-client-ca:
* certificate: "\(\(tls-kubelet-client.ca\)\)"
* kubernetes: "\(\(tls-kubernetes\)\)"
* release: kubo
* * name: kube-proxy
* properties:
* api-token: "\(\(kube-proxy-password\)\)"
* cloud-provider: vsphere
* kube-proxy-configuration:
* apiVersion: kubeproxy.config.k8s.io/v1alpha1
* clientConnection:
* kubeconfig: "/var/vcap/jobs/kube-proxy/config/kubeconfig"
* clusterCIDR: 10.200.0.0/16
* iptables:
* masqueradeAll: false
* masqueradeBit: 14
* minSyncPeriod: 0s
* syncPeriod: 30s
* kind: KubeProxyConfiguration
* mode: iptables
* portRange: ''
* tls:
* kubernetes: "\(\(tls-kubernetes\)\)"
* release: kubo
* name: worker
* networks:
* * name: service\_private
* stemcell: xenial
* vm\_type: small-highmem-16GB
* * azs:
* * z1
* instances: 1
* jobs:
* * name: haproxy
* properties:
* http\_port: 8080
* public\_ip: 115.68.46.189
* release: paasta-container-service-projects-release
* name: haproxy
* networks:
* * default:
* * dns
* * gateway
* name: service\_public
* static\_ips: 115.68.46.189
* * name: service\_private
* stemcell: xenial
* update:
* max\_in\_flight: 1
* serial: true
* vm\_type: caas\_small
* * azs:
* * z2
* instances: 1
* jobs:
* * name: mariadb
* properties:
* admin\_user:
* id: 
* password: 
* port: ''
* role\_set:
* administrator\_code: RS0001
* administrator\_code\_name: Administrator
* init\_user\_code: RS0003
* init\_user\_code\_name: Init User
* regular\_user\_code: RS0002
* regular\_user\_code\_name: Regular User
* release: paasta-container-service-projects-release
* name: mariadb
* networks:
* * name: service\_private
* persistent\_disk\_type: 10GB
* stemcell: xenial
* update:
* max\_in\_flight: 1
* serial: true
* vm\_type: caas\_small
* * azs:
* * z3
* instances: 1
* jobs:
* * name: container-service-dashboard
* properties:
* cf:
* api:
* url: [https://api](https://api).
* uaa:
* oauth:
* authorization:
* uri: [https://uaa](https://uaa)./oauth/authorize
* client:
* id: ' '
* secret: 
* info:
* uri: [https://uaa](https://uaa)./userinfo
* logout:
* url: [https://uaa](https://uaa)./logout
* token:
* access:
* uri: [https://uaa](https://uaa)./oauth/token
* check:
* uri: [https://uaa](https://uaa)./check\_token
* java\_opts: "-XX:MaxMetaspaceSize=104857K -Xss349K -Xms681574K -XX:MetaspaceSize=104857K
* -Xmx681574K"
* logging:
* file: logs/application.log
* level:
* ROOT: INFO
* path: classpath:logback-spring.xml
* management:
* security:
* enabled: false
* server:
* port: 8091
* spring:
* freemarker:
* template-loader-path: classpath:/templates/
* release: paasta-container-service-projects-release
* name: container-service-dashboard
* networks:
* * name: service\_private
* stemcell: xenial
* update:
* max\_in\_flight: 1
* serial: true
* vm\_type: caas\_small
* * azs:
* * z1
* instances: 1
* jobs:
* * name: container-service-api
* properties:
* authorization:
* id: 
* password: 
* java\_opts: "-XX:MaxMetaspaceSize=104857K -Xss349K -Xms681574K -XX:MetaspaceSize=104857K
* -Xmx681574K"
* logging:
* file: logs/application.log
* level:
* ROOT: INFO
* path: classpath:logback-spring.xml
* management:
* security:
* enabled: false
* server:
* port: 3333
* release: paasta-container-service-projects-release
* name: container-service-api
* networks:
* * name: service\_private
* stemcell: xenial
* update:
* max\_in\_flight: 1
* serial: true
* vm\_type: caas\_small\_api
* * azs:
* * z2
* instances: 1
* jobs:
* * name: container-service-common-api
* properties:
* authorization:
* id: 
* password: 
* java\_opts: "-XX:MaxMetaspaceSize=104857K -Xss349K -Xms681574K -XX:MetaspaceSize=104857K
* -Xmx681574K"
* logging:
* file: logs/application.log
* level:
* ROOT: INFO
* path: classpath:logback-spring.xml
* server:
* port: 3334
* spring:
* datasource:
* driver\_class\_name: com.mysql.cj.jdbc.Driver
* password: 
* username: 
* validationQuery: SELECT 1
* jpa:
* database: mysql
* generate-ddl: false
* hibernate:
* ddl-auto: none
* naming:
* strategy: org.hibernate.cfg.EJB3NamingStrategy
* properties:
* hibernate:
* dialect: org.hibernate.dialect.MySQLInnoDBDialect
* format\_sql: true
* show\_sql: true
* use\_sql\_comments: true
* release: paasta-container-service-projects-release
* name: container-service-common-api
* networks:
* * name: service\_private
* stemcell: xenial
* update:
* max\_in\_flight: 1
* serial: true
* vm\_type: caas\_small\_api
* * azs:
* * z3
* instances: 1
* jobs:
* * name: container-service-broker
* properties:
* auth:
* id: 
* password: 
* caas:
* api\_server\_url: [https://115.68.46.188:8443](https://115.68.46.188:8443)
* cluster\_name: micro-bosh/paasta-container-service-02
* exit\_code: caas\_exit
* init\_command: "/var/vcap/jobs/container-service-broker/script/set\_caas\_service\_info.sh"
* service\_broker\_auth\_secret: 
* credhub:
* admin\_client\_secret: 
* server\_url: 10.30.50.1:8844
* dashboard:
* url: [http://115.68.46.189:8091/caas/intro/overview/](http://115.68.46.189:8091/caas/intro/overview/)
* datasource:
* driver\_class\_name: com.mysql.jdbc.Driver
* password: 
* username: 
* freemarker:
* template-loader-path: classpath:/templates/
* java\_opts: "-XX:MaxMetaspaceSize=104857K -Xss349K -Xms681574K -XX:MetaspaceSize=104857K
* -Xmx681574K"
* jpa:
* database\_platform: org.hibernate.dialect.MySQL5InnoDBDialect
* hibernate\_ddl\_auto: none
* show\_sql: true
* logging:
* config: classpath:logback.xml
* level:
* org:
* hibernate: info
* openpaas:
* servicebroker: INFO
* server:
* port: 8888
* serviceDefinition:
* bindable: false
* desc: For Container Service Plans, You can choose plan about CPU, Memory,
* disk.
* id: 8a3f2d14-5283-487f-b6c8-6663639ad8b1\_test
* image\_url: data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAABkCAMAAABHPGVmAAAC/VBMVEVHcEwxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQxbOQ1/RrDAAAA/nRSTlMAA/wI/SAC/gHfBvoEBwsJ6BD3GPUK+QXt7vMS4fIT+A3wFOQl+93sGhUb8Tnr6sX2Dir0g+Dm2+fXDBa23JQXuxEPzx8dVrpH4lfl7zJEOhnp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
* name: container-service
* plan1:
* cpu: 2
* desc: 2 CPUs, 2GB Memory \(free\)
* disk: 10GB
* id: f02690d6-6965-4756-820e-3858111ed674test
* memory: 2GB
* name: Micro
* type: A
* weight: 1
* plan2:
* cpu: 4
* desc: 4 CPUs, 6GB Memory \(free\)
* disk: 20GB
* id: a5213929-885f-414a-801f-c66ddb5e48f1test
* memory: 6GB
* name: Small
* type: B
* weight: 2
* plan3:
* cpu: 8
* desc: 8 CPUs, 12GB Memory \(free\)
* disk: 40GB
* id: 056d05b6-4039-40ec-8619-e68490b79255test
* memory: 12GB
* name: Advanced
* type: C
* weight: 3
* planupdatable: 'true'
* tags: Container Service,Containers as a Service
* release: paasta-container-service-projects-release
* name: container-service-broker
* networks:
* * name: service\_private
* stemcell: xenial
* update:
* max\_in\_flight: 1
* serial: true
* vm\_type: caas\_small\_api
* * azs:
* * z1
* instances: 1
* jobs:
* * instances: 1
* name: private-image-repository
* properties:
* image\_repository:
* auth:
* enabled: true
* password: "$2y$05$4l7G8WyToNODwYwjHyXDnu5aB3wvKuIeipgoF.CUuGLzsaZkUEsxS"
* username: admin
* http:
* http2\_disabled: false
* port: 5000
* storage:
* delete\_enabled: true
* filesystem:
* rootdirectory: "/var/lib/docker-registry"
* release: private-image-repository-release
* name: private-image-repository
* networks:
* * default:
* * dns
* * gateway
* name: service\_private
* * name: service\_public
* static\_ips: 115.68.46.187
* persistent\_disk\_type: 10GB
* stemcell: xenial
* update:
* max\_in\_flight: 1
* serial: true
* vm\_type: small
* name: paasta-container-service-02

Task 140369

Task 140369 \| 08:21:48 \| Preparing deployment: Preparing deployment \(00:00:29\) Task 140369 \| 08:22:17 \| Preparing deployment: Rendering templates \(00:00:14\) Task 140369 \| 08:22:31 \| Preparing package compilation: Finding packages to compile \(00:00:01\) Task 140369 \| 08:22:32 \| Creating missing vms: master/bbb11117-8b7a-4d77-a318-d7dd56618c42 \(0\) Task 140369 \| 08:22:32 \| Creating missing vms: mariadb/58f821e0-9bf0-41b7-9bd7-eb8e7fd1b5d8 \(0\) Task 140369 \| 08:22:32 \| Creating missing vms: worker/6408743f-0fe7-41ac-9efa-7e4b5aa86059 \(1\) Task 140369 \| 08:22:32 \| Creating missing vms: worker/d3abc471-3fc5-44c6-8f62-bb0483ed2a1f \(0\) Task 140369 \| 08:22:32 \| Creating missing vms: container-service-common-api/5c9d89da-7b1f-464e-92c4-a5b80faa4ea2 \(0\) Task 140369 \| 08:22:32 \| Creating missing vms: worker/f7700e0c-4e33-44c2-b95e-c9e5ff3e1649 \(2\) Task 140369 \| 08:22:32 \| Creating missing vms: container-service-api/ca6019dc-fa2b-474e-b3cd-ab5bcd3e2a5a \(0\) Task 140369 \| 08:22:32 \| Creating missing vms: container-service-dashboard/75d0740e-fbfa-43a3-a83d-bbb53ce17b7f \(0\) Task 140369 \| 08:22:32 \| Creating missing vms: haproxy/d328463f-b8f2-4434-977b-14d9866c52fd \(0\) Task 140369 \| 08:22:32 \| Creating missing vms: private-image-repository/beb6b02e-7fd4-41e9-b86a-c716a0252e79 \(0\) Task 140369 \| 08:22:32 \| Creating missing vms: container-service-broker/1af9d8c2-7350-4355-9ce3-4bc0549bdf62 \(0\) Task 140369 \| 08:28:25 \| Creating missing vms: container-service-common-api/5c9d89da-7b1f-464e-92c4-a5b80faa4ea2 \(0\) \(00:05:53\) Task 140369 \| 08:28:27 \| Creating missing vms: worker/f7700e0c-4e33-44c2-b95e-c9e5ff3e1649 \(2\) \(00:05:55\) Task 140369 \| 08:28:28 \| Creating missing vms: haproxy/d328463f-b8f2-4434-977b-14d9866c52fd \(0\) \(00:05:56\) Task 140369 \| 08:28:29 \| Creating missing vms: mariadb/58f821e0-9bf0-41b7-9bd7-eb8e7fd1b5d8 \(0\) \(00:05:57\) Task 140369 \| 08:28:29 \| Creating missing vms: container-service-dashboard/75d0740e-fbfa-43a3-a83d-bbb53ce17b7f \(0\) \(00:05:57\) Task 140369 \| 08:28:30 \| Creating missing vms: container-service-api/ca6019dc-fa2b-474e-b3cd-ab5bcd3e2a5a \(0\) \(00:05:58\) Task 140369 \| 08:28:32 \| Creating missing vms: private-image-repository/beb6b02e-7fd4-41e9-b86a-c716a0252e79 \(0\) \(00:06:00\) Task 140369 \| 08:28:33 \| Creating missing vms: worker/6408743f-0fe7-41ac-9efa-7e4b5aa86059 \(1\) \(00:06:01\) Task 140369 \| 08:28:33 \| Creating missing vms: container-service-broker/1af9d8c2-7350-4355-9ce3-4bc0549bdf62 \(0\) \(00:06:01\) Task 140369 \| 08:28:34 \| Creating missing vms: worker/d3abc471-3fc5-44c6-8f62-bb0483ed2a1f \(0\) \(00:06:02\) Task 140369 \| 08:28:36 \| Creating missing vms: master/bbb11117-8b7a-4d77-a318-d7dd56618c42 \(0\) \(00:06:04\) Task 140369 \| 08:28:38 \| Updating instance master: master/bbb11117-8b7a-4d77-a318-d7dd56618c42 \(0\) \(canary\) \(00:03:47\) Task 140369 \| 08:32:25 \| Updating instance worker: worker/d3abc471-3fc5-44c6-8f62-bb0483ed2a1f \(0\) \(canary\) \(00:02:09\) Task 140369 \| 08:34:34 \| Updating instance worker: worker/6408743f-0fe7-41ac-9efa-7e4b5aa86059 \(1\) \(00:02:39\) Task 140369 \| 08:37:13 \| Updating instance worker: worker/f7700e0c-4e33-44c2-b95e-c9e5ff3e1649 \(2\) \(00:02:38\) Task 140369 \| 08:39:51 \| Updating instance haproxy: haproxy/d328463f-b8f2-4434-977b-14d9866c52fd \(0\) \(canary\) \(00:00:37\) Task 140369 \| 08:40:28 \| Updating instance mariadb: mariadb/58f821e0-9bf0-41b7-9bd7-eb8e7fd1b5d8 \(0\) \(canary\) \(00:03:21\) Task 140369 \| 08:43:49 \| Updating instance container-service-dashboard: container-service-dashboard/75d0740e-fbfa-43a3-a83d-bbb53ce17b7f \(0\) \(canary\) \(00:00:38\) Task 140369 \| 08:44:27 \| Updating instance container-service-api: container-service-api/ca6019dc-fa2b-474e-b3cd-ab5bcd3e2a5a \(0\) \(canary\) \(00:00:37\) Task 140369 \| 08:45:04 \| Updating instance container-service-common-api: container-service-common-api/5c9d89da-7b1f-464e-92c4-a5b80faa4ea2 \(0\) \(canary\) \(00:00:39\) Task 140369 \| 08:45:43 \| Updating instance container-service-broker: container-service-broker/1af9d8c2-7350-4355-9ce3-4bc0549bdf62 \(0\) \(canary\) \(00:00:44\) Task 140369 \| 08:46:27 \| Updating instance private-image-repository: private-image-repository/beb6b02e-7fd4-41e9-b86a-c716a0252e79 \(0\) \(canary\) \(00:02:41\)

Task 140369 Started Fri Sep 27 08:21:48 UTC 2019 Task 140369 Finished Fri Sep 27 08:49:08 UTC 2019 Task 140369 Duration 00:27:20 Task 140369 done

Succeeded

```text
- 업로드된 Container 서비스 릴리즈를 확인한다.
```

$ bosh -e micro-bosh releases Using environment '10.30.40.111' as user 'admin' \(openid, bosh.admin\)

Name Version Commit Hash bosh-dns 1.12.0 _5d607ed bpm 1.1.0_ 27e1c8f cfcr-etcd 1.11.1 _d398cd0 docker 35.2.1_ 0b69b44 kubo 0.34.1 _non-git paasta-container-service-projects-release 1.0_ ced4610+

\(\*\) Currently deployed \(+\) Uncommitted changes

6 releases

Succeeded

```text
- 배포된 Container 서비스팩을 확인한다.
```

$ bosh -e micro-bosh -d paasta-container-service vms Using environment '10.0.1.6' as client 'admin'

Task 902411. Done

Deployment 'paasta-container-service'

Instance Process State AZ IPs VM CID VM Type Active  
container-service-api/0b525103-0a28-4c29-aad6-f86c7b3f4e7a running z6 10.0.201.248 i-0bf34d32bf90dbc40 caas\_small true  
container-service-broker/1a1cbdb6-dc1f-4e4a-a53e-9278c4606835 running z6 10.0.201.250 i-0de66c10a5b2c0d58 caas\_small true  
container-service-common-api/7ca49bb4-b960-4968-8449-f5837d10e7b5 running z6 10.0.201.249 i-0840cffb57fe72616 caas\_small true  
container-service-dashboard/c524cb45-a100-47f3-a79c-ec84340df5b8 running z6 10.0.201.247 i-0556030057a755167 caas\_small true  
haproxy/ac9ca293-493a-4f48-9b62-9648c0bd0157 running z7 10.0.0.234 i-05f9ad9aa9d35c2a2 caas\_small true  
13.124.44.34  
mariadb/a8e69fca-dc75-4e1f-91a1-aa8a79af33da running z5 10.0.161.236 i-0766e96e35c77ccbd caas\_small true  
master/e4e93b25-3c6f-4d40-830d-06c0579fb3bb running z7 10.0.0.233 i-029ee435bba1fc5e9 small true  
13.209.173.193  
private-image-repository/b9b14aab-abed-441e-8bf6-9524c3360093 running z7 10.0.0.237 i-0a5ecd4d78ef0505f small true  
13.209.105.53  
worker/49057e0f-c1d1-4f60-9084-0b8073121dfc running z6 10.0.201.246 i-0d6bd35c8563ace6d small-highmem-16GB true  
worker/4a1ed9d9-498d-419b-8a2f-aa9ca87d05cc running z4 10.0.121.232 i-0942056c138054670 small-highmem-16GB true  
worker/94977b27-ca5a-4793-8aff-b853ee86341a running z5 10.0.161.235 i-08510826a9eca0a99 small-highmem-16GB true

11 vms

Succeeded

```text
### <div id='24'/> 2.4. Container 서비스 브로커 등록
Container 서비스팩 배포가 완료되었으면 PaaS-TA 포탈에서 서비스 팩을 사용하기 위해서 먼저 Container 서비스 브로커를 등록해 주어야 한다. 서비스 브로커 등록 시 개방형 클라우드 플랫폼에서 서비스 브로커를 등록할 수 있는 사용자로 로그인이 되어있어야 한다.

- 서비스 브로커 목록을 확인한다.
```

$ cf service-brokers Getting service brokers as admin...

name url delivery-pipeline-service-broker [http://10.30.107.64:8080](http://10.30.107.64:8080)

```text
- Container 서비스 브로커를 등록한다.


>$ cf create-service-broker {서비스팩 이름} {서비스팩 사용자ID} {서비스팩 사용자비밀번호} http://{서비스팩 URL}
> - 서비스팩 이름 : 서비스 팩 관리를 위해 개방형 클라우드 플랫폼에서 보여지는 명칭
> - 서비스팩 사용자 ID/비밀번호 : 서비스팩에 접근할 수 있는 사용자 ID/비밀번호
> - 서비스팩 URL : 서비스팩이 제공하는 API를 사용할 수 있는 URL
```

$ cf create-service-broker container-service-broker admin cloudfoundry [http://13.124.44.34:8888](http://13.124.44.34:8888)

```text
- 등록된 Container 서비스 브로커를 확인한다.
```

$ cf service-brokers Getting service brokers as admin...

name url container-service-broker [http://13.124.44.34:8888](http://13.124.44.34:8888) delivery-pipeline-service-broker [http://10.30.107.64:8080](http://10.30.107.64:8080)

```text
- 접근 가능한 서비스 목록을 확인한다.
```

$ cf service-access Getting service access as admin... broker: container-service-broker service plan access orgs container-service Micro none container-service Small none container-service Advanced none

broker: delivery-pipeline-service-broker service plan access orgs delivery-pipeline-v2 delivery-pipeline-shared all delivery-pipeline-v2 delivery-pipeline-dedicated all

```text
- 특정 조직에 해당 서비스 접근 허용을 할당한다.
```

$ cf enable-service-access container-service Enabling access to all plans of service container-service for all orgs as admin... OK

```text
- 접근 가능한 서비스 목록을 확인한다.
```

$ cf service-access Getting service access as admin... broker: container-service-broker service plan access orgs container-service Micro all container-service Small all container-service Advanced all

broker: delivery-pipeline-service-broker service plan access orgs delivery-pipeline-v2 delivery-pipeline-shared all delivery-pipeline-v2 delivery-pipeline-dedicated all

```text
### <div id='25'/> 2.5. Container 서비스 UAA Client Id 등록
UAA 포털 계정 등록 절차에 대한 순서를 확인한다.

- Container 서비스 대시보드에 접근이 가능한 IP를 알기 위해 **haproxy IP** 를 확인한다.
```

$ bosh -e micro-bosh -d paasta-container-service vms Using environment '10.0.1.6' as client 'admin'

Task 902428. Done

Deployment 'paasta-container-service'

Instance Process State AZ IPs VM CID VM Type Active  
container-service-api/0b525103-0a28-4c29-aad6-f86c7b3f4e7a running z6 10.0.201.248 i-0bf34d32bf90dbc40 caas\_small true  
container-service-broker/1a1cbdb6-dc1f-4e4a-a53e-9278c4606835 running z6 10.0.201.250 i-0de66c10a5b2c0d58 caas\_small true  
container-service-common-api/7ca49bb4-b960-4968-8449-f5837d10e7b5 running z6 10.0.201.249 i-0840cffb57fe72616 caas\_small true  
container-service-dashboard/c524cb45-a100-47f3-a79c-ec84340df5b8 running z6 10.0.201.247 i-0556030057a755167 caas\_small true  
haproxy/ac9ca293-493a-4f48-9b62-9648c0bd0157 running z7 10.0.0.234 i-05f9ad9aa9d35c2a2 caas\_small true  
13.124.44.34  
mariadb/a8e69fca-dc75-4e1f-91a1-aa8a79af33da running z5 10.0.161.236 i-0766e96e35c77ccbd caas\_small true  
master/e4e93b25-3c6f-4d40-830d-06c0579fb3bb running z7 10.0.0.233 i-029ee435bba1fc5e9 small true  
13.209.173.193  
private-image-repository/b9b14aab-abed-441e-8bf6-9524c3360093 running z7 10.0.0.237 i-0a5ecd4d78ef0505f small true  
13.209.105.53  
worker/49057e0f-c1d1-4f60-9084-0b8073121dfc running z6 10.0.201.246 i-0d6bd35c8563ace6d small-highmem-16GB true  
worker/4a1ed9d9-498d-419b-8a2f-aa9ca87d05cc running z4 10.0.121.232 i-0942056c138054670 small-highmem-16GB true  
worker/94977b27-ca5a-4793-8aff-b853ee86341a running z5 10.0.161.235 i-08510826a9eca0a99 small-highmem-16GB true

11 vms

Succeeded

```text
- uaac server의 endpoint를 설정한다.
```

$ uaac target

Target: [https://uaa.15.164.20.58.xip.io](https://uaa.15.164.20.58.xip.io) Context: admin, from client admin

```text
- URL을 변경하고 싶을 경우 아래와 같이 입력하여 변경 가능하다. <br>
```

uaac target [https://uaa](https://uaa).

```text
- UAAC 로그인을 한다.
```

$ uaac token client get Client ID: _**\*\***_ Client secret: _**\*\***_

Successfully fetched token via client credentials grant. Target: [https://uaa](https://uaa). Context: admin, from client admin

```text
- Container 서비스 계정 생성을 한다.

> $ uaac client add caasclient -s {클라이언트 비밀번호} --redirect_uri {컨테이너 서비스 대시보드 URI} --scope {퍼미션 범위} --authorized_grant_types {권한 타입} --authorities={권한 퍼미션} --autoapprove={자동승인권한}
> - 클라이언트 비밀번호 : uaac 클라이언트 비밀번호를 입력한다.
> - 컨테이너 서비스 대시보드 URI : 성공적으로 리다이렉션 할 컨테이너 서비스 대시보드 URI를 입력한다.
> - 퍼미션 범위: 클라이언트가 사용자를 대신하여 얻을 수있는 허용 범위 목록을 입력한다.
> - 권한 타입 : 서비스팩이 제공하는 API를 사용할 수 있는 권한 목록을 입력한다.
> - 권한 퍼미션 : 클라이언트에 부여 된 권한 목록을 입력한다.
> - 자동승인권한: 사용자 승인이 필요하지 않은 권한 목록을 입력한다.
```

$ uaac client add caasclient -s clientsecret --redirect\_uri "[http://localhost:8091](http://localhost:8091), [http://13.124.44.34:8091](http://13.124.44.34:8091)" --scope "cloud\_controller\_service\_permissions.read , openid , cloud\_controller.read , cloud\_controller.write , cloud\_controller.admin" --authorized\_grant\_types "authorization\_code , client\_credentials , refresh\_token" --authorities="uaa.resource" --autoapprove="openid , cloud\_controller\_service\_permissions.read"

```text
- Container 서비스 계정 수정을 한다. (이미 uaac client가 등록되어 있는 경우)

> $ uaac client update caasclient --redirect_uri={컨테이너 서비스 대시보드 URI}
>
> - 컨테이너 서비스 대시보드 URI : 성공적으로 리다이렉션 할 컨테이너 서비스 대시보드 URI를 입력한다.
```

$ uaac client update caasclient --redirect\_uri="[http://13.124.44.34:8091](http://13.124.44.34:8091) [http://localhost:8091](http://localhost:8091) http://:8091 http://:8091"

```text
### <div id='26'/> 2.6. Container 서비스 Private Image Repository 설정 (Optional)

해당 설정은 Container 서비스에서 설치된 Private Image Repository에 저장된 Image를 참조하기 위해
각 Work Node Host의 Private Image Repository와 Docker 간의 액세스를 위한 설정이다.

-    insecure registry 항목 등록(모든 Work Node마다 수정 필요)
```

$ vi /etc/docker/daemon.json  
{ "insecure-registries": \[ "10.0.0.237:5000" \] }

$ sudo /var/vcap/jobs/docker/bin/ctl stop $ sudo /var/vcap/jobs/docker/bin/ctl start

\`\`\`

