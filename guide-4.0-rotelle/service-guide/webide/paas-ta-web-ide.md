# Table of Contents

1. [문서 개요](paas-ta-web-ide.md#1)
   * 1.1. [목적](paas-ta-web-ide.md#2)
   * 1.2. [범위](paas-ta-web-ide.md#3)
   * 1.3. [시스템 구성도](paas-ta-web-ide.md#4)
   * 1.4. [참고자료](paas-ta-web-ide.md#5)
2. [WEB IDE 설치](paas-ta-web-ide.md#6)
   * 2.1. [설치전 준비사항](paas-ta-web-ide.md#7)
   * 2.2. [WEB-IDE 릴리즈 업로드](paas-ta-web-ide.md#8)
   * 2.3. [WEB-IDE Deployment 파일 수정 및 배포](paas-ta-web-ide.md#9)
3. [WEB-IDE의 PaaS-TA 포털사이트 연동](paas-ta-web-ide.md#10)
   * 3.1. [WEB-IDE 대시보드 화면](paas-ta-web-ide.md#16)
4. [WEB-IDE 에서 CF CLI 사용법](paas-ta-web-ide.md#17)
   * 4.1. [WEB-IDE New Project 화면](paas-ta-web-ide.md#18)
   * 4.2. [WEB-IDE Workspace 화면](paas-ta-web-ide.md#19)
   * 4.3. [WEB-IDE Teminal에서의 CF CLI 실행](paas-ta-web-ide.md#20)

## 1. 문서 개요

### 1.1. 목적

본 문서\(WEB-IDE 설치 가이드\)는 PaaS-TA에서 사용할 수 있는 WEB-IDE의 설치를 Bosh를 이용하여 설치 하는 방법과 PaaS-TA 포털에서 WEB-IDE 서비스를 사용하는 방법을 기술하였다. PaaS-TA 3.5 버전부터는 Bosh2.0 기반으로 deploy를 진행하며 기존 Bosh1.0 기반으로 설치를 원할경우에는 PaaS-TA 3.1 이하 버전의 문서를 참고한다.

### 1.2. 범위

설치 범위는 WEB-IDE 사용을 검증하기 위한 기본 설치를 기준으로 작성하였다.

### 1.3. 시스템 구성도

본 문서의 설치된 시스템 구성도입니다. Browser\(paasta Portal\), WEB IDE Server, Workspace, Desktop IDE로 최소사항을 구성하였다.

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/webide/web-ide-01.png)

| 구분 | Resource Pool | 스펙 |
| :--- | :--- | :--- |
| paasta-web-ide1 | resource\_pools | 1vCPU / 2GB RAM / 10GB Disk |
| paasta-web-ide1 | resource\_pools | 1vCPU / 2GB RAM / 10GB Disk |

### 1.4. 참고자료

> [**http://bosh.io/docs**](http://bosh.io/docs)   
>  [**http://docs.cloudfoundry.org/**](http://docs.cloudfoundry.org/)   
>  [**https://www.eclipse.org/che/technology/**](https://www.eclipse.org/che/technology/)

## 2.WEB IDE 설치

### 2.1. 설치전 준비사항

본 설치 가이드는 Linux 환경에서 설치하는 것을 기준으로 하였다. 서비스팩 설치를 위해서는 먼저 BOSH CLI v2 가 설치 되어 있어야 하고 BOSH 에 로그인이 되어 있어야 한다.  
 BOSH CLI v2 가 설치 되어 있지 않을 경우 먼저 BOSH2.0 설치 가이드 문서를 참고 하여 BOSH CLI v2를 설치를 하고 사용법을 숙지 해야 한다.  


* BOSH2.0 사용자 가이드

  > BOSH2 사용자 가이드 : [https://github.com/PaaS-TA/Guide-4.0-ROTELLE/blob/master/PaaS-TA\_BOSH2\_사용자\_가이드v1.0.md](https://github.com/PaaS-TA/Guide-4.0-ROTELLE/blob/master/PaaS-TA_BOSH2_사용자_가이드v1.0.md)

> BOSH CLI V2 사용자 가이드 : [https://github.com/PaaS-TA/Guide-4.0-ROTELLE/blob/master/Use-Guide/Bosh/PaaS-TA\_BOSH\_CLI\_V2\_사용자\_가이드v1.0.md](https://github.com/PaaS-TA/Guide-4.0-ROTELLE/blob/master/Use-Guide/Bosh/PaaS-TA_BOSH_CLI_V2_사용자_가이드v1.0.md)

* PaaS-TA에서 제공하는 압축된 릴리즈 파일들을 다운받는다. \(PaaSTA-Deployment.zip, PaaSTA-Sample-Apps.zip, PaaSTA-Services.zip\)
* 다운로드 위치

  > Download : [https://paas-ta.kr/download/package](https://paas-ta.kr/download/package)

### 2.2. WEB-IDE 릴리즈 업로드

* 업로드 되어 있는 릴리즈 목록을 확인한다.
* **사용 예시**

  ```text
    $ bosh -e micro-bosh releases
        Using environment '10.30.40.111' as user 'admin' (openid, bosh.admin)

    Name                              Version   Commit Hash  
        binary-buildpack                  1.0.21*   d714741  
        bpm                               0.9.0*    c9b7136  
        caas-release                      1.0*      empty+  
        capi                              1.62.0*   22a608c  
        cf-networking                     2.8.0*    479f4a66  
        cf-smoke-tests                    40.0.5*   d6aaf1f  
        cf-syslog-drain                   7.0*      71b995a  
        cflinuxfs2                        1.227.0*  60128e1  
        consul                            195*      67cdbcd  
        diego                             2.13.0*   b5644d9  
        dotnet-core-buildpack             2.1.3*    46a41cd  
        garden-runc                       1.15.1*   75107e7+  
        go-buildpack                      1.8.25*   40c60a0  
        haproxy                           8.8.0*    9292573  
        java-buildpack                    4.13*     c2749d3  
        loggregator                       103.0*    05da4e3d  
        loggregator-agent                 2.0*      2382c90  
        nats                              24*       30e7a82  
        nodejs-buildpack                  1.6.28*   4cfdb7b  
        paas-ta-portal-release            2.0*      non-git  
        paasta-delivery-pipeline-release  1.0*      b3ee8f48+  
        paasta-pinpoint                   2.0*      2dbb8bf3+  
        php-buildpack                     4.3.57*   efc48f3  
        postgres                          29*       5de4d63d+  
        python-buildpack                  1.6.18*   bcc4f26  
        routing                           0.179.0*  18155a5  
        ruby-buildpack                    1.7.21*   9d69600  
        silk                              2.9.0*    eebed55  
        staticfile-buildpack              1.4.29*   8a82e63  
        statsd-injector                   1.3.0*    39e5179  
        uaa                               60.2*     ebb5895  

        (*) Currently deployed
        (+) Uncommitted changes

        31 releases

        Succeeded
  ```

* WEB-IDE 서비스 릴리즈가 업로드 되어 있지 않은 것을 확인
* WEB-IDE 서비스 릴리즈 파일을 업로드한다.
* **사용 예시**

  ```text
    $ bosh -e micro-bosh upload-release paasta-web-ide-2.0.tgz
        Using environment '10.30.40.111' as user 'admin' (openid, bosh.admin)

    ######################################################## 100.00% 160.45 MiB/s 1s
    Task 7866

    Task 7866 | 02:21:08 | Extracting release: Extracting release (00:00:03)
    Task 7866 | 02:21:11 | Verifying manifest: Verifying manifest (00:00:00)
    Task 7866 | 02:21:11 | Resolving package dependencies: Resolving package dependencies (00:00:00)
    Task 7866 | 02:21:11 | Creating new packages: eclipse-che/eff6040fd5ed2a30190955140bb58f892ff830ec (00:00:03)
    Task 7866 | 02:21:14 | Creating new packages: bosh-helpers/2b45cec940a80e582427f61c460269c6ccb031c8 (00:00:01)
    Task 7866 | 02:21:15 | Creating new packages: docker/8da016ec9d1b172b779d5ff0a9fbbfc4973ea734 (00:00:00)
    Task 7866 | 02:21:15 | Creating new packages: java/b74e140053eddb6a3a958568d66f801686d09e04 (00:00:02)
    Task 7866 | 02:21:17 | Creating new jobs: eclipse-che/2f368c268ee821488f04f4b05a25eba963cda484 (00:00:00)
    Task 7866 | 02:21:17 | Release has been created: paasta-web-ide/2.0 (00:00:00)

    Task 7866 Started  Thu Sep 13 02:21:08 UTC 2018
    Task 7866 Finished Thu Sep 13 02:21:17 UTC 2018
    Task 7866 Duration 00:00:09
    Task 7866 done

    Succeeded
  ```

* 업로드 된 WEB-IDE 릴리즈를 확인한다.
* **사용 예시**

  ```text
    $ bosh -e micro-bosh releases
        Using environment '10.30.40.111' as user 'admin' (openid, bosh.admin)

    Name                              Version   Commit Hash  
        binary-buildpack                  1.0.21*   d714741  
    bpm                               0.9.0*    c9b7136  
    caas-release                      1.0*      empty+  
    capi                              1.62.0*   22a608c  
    cf-networking                     2.8.0*    479f4a66  
    cf-smoke-tests                    40.0.5*   d6aaf1f  
    cf-syslog-drain                   7.0*      71b995a  
    cflinuxfs2                        1.227.0*  60128e1  
    consul                            195*      67cdbcd  
    diego                             2.13.0*   b5644d9  
    dotnet-core-buildpack             2.1.3*    46a41cd  
    garden-runc                       1.15.1*   75107e7+  
    go-buildpack                      1.8.25*   40c60a0  
    haproxy                           8.8.0*    9292573  
    java-buildpack                    4.13*     c2749d3  
    loggregator                       103.0*    05da4e3d  
    loggregator-agent                 2.0*      2382c90  
    nats                              24*       30e7a82  
    nodejs-buildpack                  1.6.28*   4cfdb7b  
    paas-ta-portal-release            2.0*      non-git  
    paasta-delivery-pipeline-release  1.0*      b3ee8f48+  
    paasta-web-ide                    2.0       00000000  
    paasta-pinpoint                   2.0*      2dbb8bf3+  
    php-buildpack                     4.3.57*   efc48f3  
    postgres                          29*       5de4d63d+  
    python-buildpack                  1.6.18*   bcc4f26  
    routing                           0.179.0*  18155a5  
    ruby-buildpack                    1.7.21*   9d69600  
    silk                              2.9.0*    eebed55  
    staticfile-buildpack              1.4.29*   8a82e63  
    statsd-injector                   1.3.0*    39e5179  
    uaa                               60.2*     ebb5895  

        (*) Currently deployed
        (+) Uncommitted changes

        32 releases

        Succeeded
  ```

* WEB-IDE 서비스 릴리즈가 업로드 되어 있는 것을 확인
* Deploy시 사용할 Stemcell을 확인한다.
* **사용 예시**

  ```text
    $ bosh -e micro-bosh stemcells
    Name                                       Version  OS             CPI  CID  
    bosh-openstack-kvm-ubuntu-xenial-go_agent  315.41*  ubuntu-xenial  -    fb08e389-2350-4091-9b29-41743495e62c  
    ~                                          315.36*  ubuntu-xenial  -    7076cf5d-a473-4c46-b6c1-4a7813911f76  

    (*) Currently deployed

    2 stemcells

    Succeeded
  ```

### 2.3.WEB-IDE Deployment 파일 수정 및 배포

BOSH Deployment manifest 는 components 요소 및 배포의 속성을 정의한 YAML 파일이다. Deployment manifest 에는 sotfware를 설치 하기 위해서 어떤 Stemcell \(OS, BOSH agent\) 을 사용할것이며 Release \(Software packages, Config templates, Scripts\) 이름과 버전, VMs 용량, Jobs params 등을 정의가 되어 있다.

deployment 파일에서 사용하는 network, vm\_type 등은 cloud config 를 활용하고 해당 가이드는 Bosh2.0 가이드를 참고한다.

* cloud config 내용 조회
* **사용 예시**

  ```text
    bosh -e micro-bosh cloud-config
    Using environment '10.30.40.111' as user 'admin' (openid, bosh.admin)

    azs:
    - cloud_properties:
        datacenters:
        - clusters:
          - BD-HA:
          resource_pool: CF_BOSH2_Pool
          name: BD-HA
      name: z1
    - cloud_properties:
        datacenters:
        - clusters:
          - BD-HA:
          resource_pool: CF_BOSH2_Pool
          name: BD-HA
      name: z2
    - cloud_properties:
        datacenters:
        - clusters:
          - BD-HA:
          resource_pool: CF_BOSH2_Pool
          name: BD-HA
      name: z3
    - cloud_properties:
        datacenters:
        - clusters:
          - BD-HA:
          resource_pool: CF_BOSH2_Pool
          name: BD-HA
      name: z4
    - cloud_properties:
        datacenters:
        - clusters:
          - BD-HA:
          resource_pool: CF_BOSH2_Pool
          name: BD-HA
      name: z5
    - cloud_properties:
        datacenters:
        - clusters:
          - BD-HA:
          resource_pool: CF_BOSH2_Pool
          name: BD-HA
      name: z6
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
        cloud_properties:
          name: Internal
        dns:
        - 8.8.8.8
        gateway: 10.30.20.23
        range: 10.30.0.0/16
        reserved:
        - 10.30.0.0 - 10.30.111.40
    - name: public
      subnets:
      - azs:
        - z1
        - z2
        - z3
        - z4
        - z5
        - z6
        cloud_properties:
          name: External
        dns:
        - 8.8.8.8
        gateway: 115.68.46.177
        range: 115.68.46.176/28
        reserved:
        - 115.68.46.176 - 115.68.46.188
        static:
        - 115.68.46.189 - 115.68.46.190
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
        cloud_properties:
          name: Internal
        dns:
        - 8.8.8.8
        gateway: 10.30.20.23
        range: 10.30.0.0/16
        reserved:
        - 10.30.0.0 - 10.30.106.255
        static:
        - 10.30.107.1 - 10.30.107.255
    - name: service_public
      subnets:
      - azs:
        - z1
        - z2
        - z3
        - z4
        - z5
        - z6
        cloud_properties:
          name: External
        dns:
        - 8.8.8.8
        gateway: 115.68.47.161
        range: 115.68.47.160/24
        reserved:
        - 115.68.47.161 - 115.68.47.174
        static:
        - 115.68.47.175 - 115.68.47.185
      type: manual
    - name: portal_service_public
      subnets:
      - azs:
        - z1
        - z2
        - z3
        - z4
        - z5
        - z6
        cloud_properties:
          name: External
        dns:
        - 8.8.8.8
        gateway: 115.68.46.209
        range: 115.68.46.208/28
        reserved:
        - 115.68.46.216 - 115.68.46.222
        static:
        - 115.68.46.214
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
        disk: 30720
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
        cpu: 2
        disk: 8192
        ram: 4096
      name: service_medium
    - cloud_properties:
        cpu: 2
        disk: 10240
        ram: 2048
      name: service_medium_2G

    Succeeded
  ```

* Deployment 파일을 서버 환경에 맞게 수정한다.

```text
# paasta-web-ide-aws 설정 파일 내용
---
name: paasta-web-ide  # 서비스 배포이름(필수)

stemcells:
  - alias: ((stemcell_alias))
    os: ((stemcell_os))
    version: "((stemcell_version))"

releases:
  - name: "((releases_name))"                   # 서비스 릴리즈 이름(필수) bosh releases로 확인 가능
    version: latest                                             # 서비스 릴리즈 버전(필수):latest 시 업로드된 서비스 릴리즈 최신버전

update:
  canaries: 1                                               # canary 인스턴스 수(필수)
  canary_watch_time: 5000-120000                            # canary 인스턴스가 수행하기 위한 대기 시간(필수)
  update_watch_time: 5000-120000                            # non-canary 인스턴스가 수행하기 위한 대기 시간(필수)
  max_in_flight: 1                                          # non-canary 인스턴스가 병렬로 update 하는 최대 개수(필수)
  serial: false


instance_groups:
- name: eclipse-che  #작업 이름(필수)
  azs:
    - z7
  instances: "((eclipse_che_instances))"
  vm_type: "((web_ide_vm_type))"
  stemcell: "((stemcell_alias))"
  networks:
  - name: ((internal_networks_name))
  - name: ((external_networks_name))
    static_ips: ((eclipse_che_public_ip))
  jobs:
  - name: eclipse-che
    release: "((releases_name))"


- name: mariadb
  azs:
    - z3
  instances: 1
  vm_type: small
  stemcell: "((stemcell_alias))"
  persistent_disk_type: "((mariadb_disk_type))"
  networks:
  - name: ((internal_networks_name))
  jobs:
  - name: mariadb
    release: "((releases_name))"
  syslog_aggregator: null
  properties:
    mariadb:                                                # MARIA DB SERVER 설정 정보
      port: ((mariadb_port))                                            # MARIA DB PORT 번호
      admin_user:
        password: '((mariadb_user_password))'                             # MARIA DB ROOT 계정 비밀번호
      host_names:
        - mariadb0
  ########## INFRA ##########

  ######## BROKER ########

- name: webide-broker
  azs:
    - z3
  instances: 1
  vm_type: medium
  stemcell: "((stemcell_alias))"
  networks:
  - name: ((internal_networks_name))
  jobs:
  - name: web-ide-broker
    release: "((releases_name))"
  syslog_aggregator: null
  properties:
    server:
      port: ((server_port))
    datasource:
      password: "((mariadb_user_password))"
    serviceDefinition:
      id: ((serviceDefinition_id))
      plan1:
        id: ((serviceDefinition_plan1_id))
```

* deploy-web-ide-bosh2.0.sh 파일을 서버 환경에 맞게 수정한다.

```bash
#!/bin/bash

bosh -d paasta-web-ide deploy paasta_web_ide.yml \
   -o use-public-network-aws.yml \
   -v releases_name="paas-ta-webide-release" \
   -v stemcell_os="ubuntu-xenial" \
   -v stemcell_version="315.36" \
   -v stemcell_alias="default" \
   -v web_ide_vm_type="large" \
   -v vm_type_tiny="minimal" \
   -v vm_type_small="small" \
   -v internal_networks_name=default \
   -v external_networks_name=vip \
   -v eclipse_che_instances=1 \
   -v eclipse_che_public_ip=["xx.xxx.xx.xxx"] \
   -v server_port="8080" \
   -v serviceDefinition_id="af86588c-6212-11e7-907b-b6006ad3webide0" \
   -v serviceDefinition_plan1_id="a5930564-6212-11e7-907b-b6006ad3webide1" \
   -v mariadb_disk_type="10GB" \
   -v mariadb_port="3306" \
   -v mariadb_user_password="Paasta@2018" \
```

* WEB-IDE 서비스팩을 배포한다.
* **사용 예시**

  ```text
    $ ./deploy-web-ide-bosh2.0.sh
    # paasta-web-ide-service 설정 파일 내용
    ---
    name: paasta-web-ide-service  # 서비스 배포이름(필수)

    release:
      name: paasta-web-ide  #서비스 릴리즈 이름(필수)
      version: "2.0"   #서비스 릴리즈 버전(필수):latest 시 업로드된 서비스 릴리즈 최신버전

    stemcells:
    - alias: default
      os: ((stemcell_os))
      version: "((stemcell_version))"

    update:
      canaries: 1                                          # canary 인스턴스 수(필수)
      canary_watch_time: 30000-180000                      # canary 인스턴스가 수행하기 위한 대기 시간(필수)
      max_in_flight: 1                                      # non-canary 인스턴스가 병렬로 update 하는 최대 개수(필수)
      update_watch_time: 30000-180000                      # non-canary 인스턴스가 수행하기 위한 대기 시간(필수)

    instance_groups:
    - name: paasta-web-ide1 #작업 이름(필수)
      azs:
      - z5
      instances: 1          # job 인스턴스 수(필수)
      vm_type: ((vm_type_medium))            # cloud config 에 정의한 vm_type
      stemcell: default
      networks:
      - name: ((default_network_name))       # cloud config 에 정의한 network 이름
      - name: ((public_network_name))
        static_ips: xxx.xx.xx.xx1
      properties:
        che:
          ip: xxx.xx.xx.xx1
          port: 8080
      templates:
      - name: eclipse-che                # job template 이름(필수)
        release: paasta-web-ide

    - name: paasta-web-ide2 #작업 이름(필수)
      azs:
      - z5
      instances: 1          # job 인스턴스 수(필수)
      vm_type: ((vm_type_medium))            # cloud config 에 정의한 vm_type
      stemcell: default
      networks:
      - name: ((default_network_name))       # cloud config 에 정의한 network 이름
      - name: ((public_network_name))
        static_ips: xxx.xx.xx.xx2
      properties:
        che:
          ip: xxx.xx.xx.xx2
          port: 8080
      templates:
      - name: eclipse-che                # job template 이름(필수)
        release: paasta-web-ide
  ```

* WEB IDE 서비스팩을 배포한다.

  inception@inception:~/workspace/servicepacks/paasta-web-ide-2.0$ ./deploy-web-ide-bosh2.0.sh Using environment '10.0.1.6' as client 'admin'

  Using deployment 'paasta-web-ide'

  * stemcells:
  * * alias: default
  * os: ubuntu-xenial
  * version: '315.36'

    releases:

  * * name: paas-ta-webide-release
  * version: '1.0'
  * update:
  * canaries: 1
  * canary\_watch\_time: 5000-120000
  * max\_in\_flight: 1
  * serial: false
  * update\_watch\_time: 5000-120000
  * instance\_groups:
  * * azs:
  * * z7
  * instances: 1
  * jobs:
  * * name: eclipse-che
  * release: paas-ta-webide-release
  * name: eclipse-che
  * networks:
  * * default:
  * * dns
  * * gateway
  * name: default
  * * name: vip
  * static\_ips:
  * * 13.209.28.254
  * stemcell: default
  * vm\_type: large
  * * azs:
  * * z3
  * instances: 1
  * jobs:
  * * name: mariadb
  * release: paas-ta-webide-release
  * name: mariadb
  * networks:
  * * name: default
  * persistent\_disk\_type: 10GB
  * properties:
  * mariadb:
  * admin\_user:
  * password: ""
  * host\_names:
  * * ""
  * port: ""
  * stemcell: default
  * syslog\_aggregator: 
  * vm\_type: small
  * * azs:
  * * z3
  * instances: 1
  * jobs:
  * * name: web-ide-broker
  * release: paas-ta-webide-release
  * name: webide-broker
  * networks:
  * * name: default
  * properties:
  * datasource:
  * password: ""
  * server:
  * port: ""
  * serviceDefinition:
  * id: ""
  * plan1:
  * id: ""
  * stemcell: default
  * syslog\_aggregator: 
  * vm\_type: medium
  * name: paasta-web-ide Continue? \[yN\]: y

    Task 7867

    Task 7867 \| 02:29:25 \| Preparing deployment: Preparing deployment \(00:00:02\) Task 7867 \| 02:29:27 \| Preparing package compilation: Finding packages to compile \(00:00:00\) Task 7867 \| 02:29:27 \| Compiling packages: bosh-helpers/2b45cec940a80e582427f61c460269c6ccb031c8 Task 7867 \| 02:29:27 \| Compiling packages: docker/8da016ec9d1b172b779d5ff0a9fbbfc4973ea734 Task 7867 \| 02:29:27 \| Compiling packages: java/b74e140053eddb6a3a958568d66f801686d09e04 Task 7867 \| 02:31:36 \| Compiling packages: bosh-helpers/2b45cec940a80e582427f61c460269c6ccb031c8 \(00:02:09\) Task 7867 \| 02:31:38 \| Compiling packages: docker/8da016ec9d1b172b779d5ff0a9fbbfc4973ea734 \(00:02:11\) Task 7867 \| 02:31:59 \| Compiling packages: java/b74e140053eddb6a3a958568d66f801686d09e04 \(00:02:32\) Task 7867 \| 02:31:59 \| Compiling packages: eclipse-che/eff6040fd5ed2a30190955140bb58f892ff830ec \(00:00:55\) Task 7867 \| 02:33:27 \| Creating missing vms: paasta-web-ide1/dfa63633-f846-48a4-9ea8-c23291fe0ea0 \(0\) Task 7867 \| 02:33:27 \| Creating missing vms: paasta-web-ide2/9a1e6f85-a8d5-41c0-96f7-56ddb8bce657 \(0\) \(00:01:18\) Task 7867 \| 02:34:46 \| Creating missing vms: paasta-web-ide1/dfa63633-f846-48a4-9ea8-c23291fe0ea0 \(0\) \(00:01:19\) Task 7867 \| 02:34:47 \| Updating instance paasta-web-ide1: paasta-web-ide1/dfa63633-f846-48a4-9ea8-c23291fe0ea0 \(0\) \(canary\) \(00:01:29\) Task 7867 \| 02:36:16 \| Updating instance paasta-web-ide2: paasta-web-ide2/9a1e6f85-a8d5-41c0-96f7-56ddb8bce657 \(0\) \(canary\) \(00:01:30\)

    Task 7867 Started Thu Sep 13 02:29:25 UTC 2018 Task 7867 Finished Thu Sep 13 02:37:46 UTC 2018 Task 7867 Duration 00:08:21 Task 7867 done

    Succeeded

* 배포된 WEB-IDE 서비스팩을 확인한다.
* **사용 예시**

  ```text
    $bosh -e micro-bosh -d paasta-web-ide-service vms
    Using environment '10.30.40.111' as user 'admin' (openid, bosh.admin)

    Task 7872. Done

    Deployment 'paasta-web-ide-service'

    Instance                                              Process State  AZ  IPs            VM CID                                   VM Type            Active  
    paasta-web-ide1/dfa63633-f846-48a4-9ea8-c23291fe0ea0  running        z5  10.30.107.0    vm-4811a409-e48b-44c1-976c-268fe0af0697  service_medium_2G  true  
                                         xxx.xx.xx.xx1                                                                
    paasta-web-ide2/9a1e6f85-a8d5-41c0-96f7-56ddb8bce657  running        z5  10.30.108.0    vm-ba125e95-804a-4272-8cda-e3bfa38be98f  service_medium_2G  true  
                                         xxx.xx.xx.xx2                                                                

    2 vms

    Succeeded
  ```

## 3. WEB-IDE의 PaaS-TA 포털사이트 연동

### 3.1. WEB-IDE 서비스 브로커를 등록한다.

> `$ cf create-service-broker {서비스팩 이름} {서비스팩 사용자ID} {서비스팩 사용자비밀번호} http://{서비스팩 URL(IP)}`

**서비스팩 이름** : 서비스 팩 관리를 위해 PaaS-TA에서 보여지는 명칭이다. 서비스 Marketplace에서는 각각의 API 서비스 명이 보여지니 여기서 명칭은 서비스팩 리스트의 명칭이다.  
 **서비스팩 사용자ID** / 비밀번호 : 서비스팩에 접근할 수 있는 사용자 ID입니다. 서비스팩도 하나의 API 서버이기 때문에 아무나 접근을 허용할 수 없어 접근이 가능한 ID/비밀번호를 입력한다.  
 **서비스팩 URL** : 서비스팩이 제공하는 API를 사용할 수 있는 URL을 입력한다.

> `$ cf create-service-broker webide-service-broker admin cloudfoundry http://10.30.56.33:8080`

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/webide/web-ide-15.png)

**등록된 RabbitMQ 서비스 브로커를 확인한다.**

> `$ cf service-brokers`

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/webide/web-ide-16.png)

#### 접근 가능한 서비스 목록을 확인한다.

> `$ cf service-access`

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/webide/web-ide-17.png)

* 서비스 브로커 등록시 최초에는 접근을 허용하지 않는다. 따라서 access는 none으로 설정된다.

#### 특정 조직에 해당 서비스 접근 허용을 할당하고 접근 서비스 목록을 다시 확인한다. \(전체 조직\)

> `$ cf enable-service-access webide`  
>  `$ cf service-access`

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/webide/web-ide-18.png)

#### PaaS-TA Marketplace에서 서비스가 있는지 확인을 한다.

> `$ cf marketplace`

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/webide/web-ide-19.png)

#### Marketplace에서 원하는 서비스가 있으면 서비스 신청\(Provision\)을 한다.

> `$ cf create-service {서비스명} {서비스 플랜} {내 서비스명}`
>
> * **서비스명** : p-rabbitmq로 Marketplace에서 보여지는 서비스 명칭이다.
> * **서비스플랜** : 서비스에 대한 정책으로 plans에 있는 정보 중 하나를 선택한다. RabbitMQ 서비스는 standard plan만 지원한다.
> * **내 서비스명** : 내 서비스에서 보여지는 명칭이다. 이 명칭을 기준으로 환경 설정 정보를 가져온다.
>
> `$ cf create-service webide webide-shared webide-service`

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/webide/web-ide-20.png)

#### 생성된 rabbitmq 서비스 인스턴스를 확인한다.

> `$ cf services`

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/webide/web-ide-21.png)

## 4. WEB-IDE 에서 CF CLI 사용법

### 4.1. WEB-IDE New Project 화면

* 사용할 언어를 선택하고 Create workspace and project 로 새로운 프로젝트를 시작한다.

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/webide/web-ide-08-1.png)

* Workspace를 구성하기 위해 Docker 관련 자료를 다운로드한다.

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/webide/web-ide-09.png)

### 4.2. WEB-IDE Workspace 화면

* Open Project를 누르면 Workspace 화면이 열린다.

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/webide/web-ide-10.png)

* 실제로 소스를 개발해서 빌드하거나 GIT이나 SVN에서 IMPORT 한다.

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/webide/web-ide-11.png)

### 4.3. WEB-IDE Teminal에서의 CF CLI 실행

**-cf api 명령을 이용해 endpoint를 지정한다.**

> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/webide/web-ide-12.png)

**cf login 명령어로 로그인하고 조직과 공간을 선택한다.**

> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/webide/web-ide-13.png)

**cf push 를 이용해 cf에 앱을 업로드한다.**

> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/webide/web-ide-14.png)

