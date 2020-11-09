# Table of Contents

1. [문서 개요](paas-ta-_v1.0.md#1)
   * [1.1. 목적](paas-ta-_v1.0.md#11)
   * [1.2. 범위](paas-ta-_v1.0.md#12)
   * [1.3. 시스템 구성도](paas-ta-_v1.0.md#13)
   * [1.4. 참고 자료](paas-ta-_v1.0.md#14)
2. [배포 파이프라인 서비스팩 설치](paas-ta-_v1.0.md#2)
   * [2.1. 설치 전 준비사항](paas-ta-_v1.0.md#21)  
   * [2.2. 배포 파이프라인 서비스 릴리즈 업로드](paas-ta-_v1.0.md#22)
   * [2.3. 배포 파이프라인 서비스 릴리즈 Deployment 파일 수정 및 배포](paas-ta-_v1.0.md#23)
   * [2.4. 배포 파이프라인 서비스 브로커 등록](paas-ta-_v1.0.md#24)
   * [2.5. 배포 파이프라인 UAA Client Id 등록](paas-ta-_v1.0.md#25)

## 1. 문서 개요

### 1.1 목적

본 문서\(배포 파이프라인 서비스팩 설치 가이드\)는 개방형 PaaS 플랫폼 고도화 및 개발자 지원 환경 기반의 Open PaaS에서 제공되는 서비스팩인 배포 파이프라인 서비스팩을 Bosh를 이용하여 설치 및 서비스 등록하는 방법을 기술하였다. PaaS-TA 3.5 버전부터는 Bosh2.0 기반으로 deploy를 진행하며 기존 Bosh1.0 기반으로 설치를 원할경우에는 PaaS-TA 3.1 이하 버전의 문서를 참고한다.

### 1.2 범위

설치 범위는 배포 파이프라인 서비스팩을 검증하기 위한 기본 설치를 기준으로 작성하였다.

### 1.3 시스템 구성도

본 문서의 설치된 시스템 구성도입니다. 배포 파이프라인 Server, 형상관리 서비스 브로커로 최소사항을 구성하였다. ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/pipeline/Delivery_Pipeline_Architecture.jpg)

| VM 명 | 인스턴스 수 | vCPU 수 | 메모리\(GB\) | 디스크\(GB\) |
| :--- | :--- | :--- | :--- | :--- |
| HAProxy | 1 | 1 | 2 | Root 4G |
| WEB UI | N | 1 | 2 | Root 4G |
| Service broker | 1 | 1 | 2 | Root 4G |
| Common API | N | 1 | 2 | Root 4G |
| DeliveryPipeline API | N | 1 | 2 | Root 4G |
| Inspection API | N | 1 | 2 | Root 4G |
| Storage API | 1 | 1 | 2 | Root 4G |
| Scheduler | 1 | 1 | 2 | Root 4G |
| DeliveryPipeline | N | 1 | 2 | Root 8G + 영구디스크 10G |
| Inspection | 1 | 1 | 2 | Root 4G |
| Storage | 1 | 1 | 4 | Root 4G + 영구디스크 50G |
| DBMS\(mariadb\) | 1 | 1 | 4 | Root 6G + 영구디스크 4G |
| Postgres | 1 | 1 | 2 | Root 6G + 영구디스크 4G |

### 1.4 참고 자료

> [http://bosh.io/docs](http://bosh.io/docs)   
>  [http://docs.cloudfoundry.org/](http://docs.cloudfoundry.org/)

## 2. 배포 파이프라인 서비스팩 설치

### 2.1 설치 전 준비사항

본 설치 가이드는 Linux 환경에서 설치하는 것을 기준으로 하였다. 서비스팩 설치를 위해서는 먼저 BOSH CLI v2 가 설치 되어 있어야 하고 BOSH 에 로그인이 되어 있어야 한다.  
 BOSH CLI v2 가 설치 되어 있지 않을 경우 먼저 BOSH2.0 설치 가이드 문서를 참고 하여 BOSH CLI v2를 설치를 하고 사용법을 숙지 해야 한다.  


* BOSH2.0 사용자 가이드

  > BOSH2 사용자 가이드 : [https://github.com/PaaS-TA/Guide-4.0-ROTELLE/blob/master/PaaS-TA\_BOSH2\_사용자\_가이드v1.0.md](https://github.com/PaaS-TA/Guide-4.0-ROTELLE/blob/master/PaaS-TA_BOSH2_사용자_가이드v1.0.md)

> BOSH CLI V2 사용자 가이드 : [https://github.com/PaaS-TA/Guide-4.0-ROTELLE/blob/master/Use-Guide/Bosh/PaaS-TA\_BOSH\_CLI\_V2\_사용자\_가이드v1.0.md](https://github.com/PaaS-TA/Guide-4.0-ROTELLE/blob/master/Use-Guide/Bosh/PaaS-TA_BOSH_CLI_V2_사용자_가이드v1.0.md)

* PaaS-TA에서 제공하는 압축된 릴리즈 파일들을 다운받는다. \(PaaSTA-Deployment.zip, PaaSTA-Sample-Apps.zip, PaaSTA-Services.zip\)
* 다운로드 위치

  > Download : [https://paas-ta.kr/download/package](https://paas-ta.kr/download/package)

### 2.2 배포 파이프라인 서비스 릴리즈 업로드

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

        30 releases

        Succeeded
  ```

* paasta-delivery-pipeline-release 서비스 릴리즈가 업로드 되어 있지 않은 것을 확인
* paasta-delivery-pipeline-release 서비스 릴리즈 파일을 업로드한다.
* **사용 예시**

  ```text
    $ bosh -e micro-bosh upload-release paasta-delivery-pipeline-release-1.0.tgz
        Using environment '10.30.40.111' as user 'admin' (openid, bosh.admin)
    Using environment '10.30.40.111' as user 'admin' (openid, bosh.admin)

    ######################################################## 100.00% 144.14 MiB/s 2s
    Task 4460

    Task 4460 | 04:31:41 | Extracting release: Extracting release (00:00:04)
    Task 4460 | 04:31:45 | Verifying manifest: Verifying manifest (00:00:00)
    Task 4460 | 04:31:45 | Resolving package dependencies: Resolving package dependencies (00:00:00)
    Task 4460 | 04:31:45 | Creating new packages: gra-log-purger/f02fa5774ab54dbb1b1c3702d03cb929b85d60e6 (00:00:00)
    Task 4460 | 04:31:45 | Creating new packages: cf-mysql-broker/250c6466bdaff96677e501ed5219d92ce4e61bd8 (00:00:00)
    Task 4460 | 04:31:45 | Creating new packages: mysqlclient/ce95f8ac566f76b650992987d5282ee473356e43 (00:00:00)
    Task 4460 | 04:31:45 | Creating new packages: acceptance-tests/1cb3ce7e20f5a8395b43fc6f0e3f2e92b0dc27bd (00:00:00)
    Task 4460 | 04:31:45 | Creating new packages: galera/d15a1d2d15e5e7417278d4aa1b908566022b9623 (00:00:01)
    Task 4460 | 04:31:46 | Creating new packages: galera-healthcheck/3da4dedbcd7d9f404a19e7720e226fd472002266 (00:00:00)
    Task 4460 | 04:31:46 | Creating new packages: quota-enforcer/e2c4c9e7d7bbbe4bfdc0866962461b00e654cca3 (00:00:00)
    Task 4460 | 04:31:46 | Creating new packages: python/4e255efa754d91b825476b57e111345f200944e1 (00:00:01)
    Task 4460 | 04:31:47 | Creating new packages: ruby/ff79c965224b4160c1526bd704b3b21e4ad7c362 (00:00:00)
    Task 4460 | 04:31:47 | Creating new packages: route-registrar/f3fdfb8c940e7227a96c06e413ae6827aba8eeda (00:00:00)
    Task 4460 | 04:31:47 | Creating new packages: check/d6811f25e9d56428a9b942631c27c9b24f5064dc (00:00:01)
    Task 4460 | 04:31:48 | Creating new packages: cli/24305e50a638ece2cace4ef4803746c0c9fe4bb0 (00:00:00)
    Task 4460 | 04:31:48 | Creating new packages: mariadb/43aa3547bc5a01dd51f1501e6b93c215dd7255e9 (00:00:01)
    Task 4460 | 04:31:49 | Creating new packages: openjdk-1.8.0_45/57e0ee876ea9d90f5470e3784ae1171bccee850a (00:00:02)
    Task 4460 | 04:31:51 | Creating new packages: mariadb_ctrl/7658290da98e2cad209456f174d3b9fa143c87fc (00:00:01)
    Task 4460 | 04:31:52 | Creating new packages: scons/11e7ad3b28b43a96de3df7aa41afddde582fcc38 (00:00:00)
    Task 4460 | 04:31:52 | Creating new packages: syslog_aggregator/078da6dcb999c1e6f5398a6eb739182ccb4aba25 (00:00:00)
    Task 4460 | 04:31:52 | Creating new packages: xtrabackup/2e701e7a9e4241b28052d984733de36aae152275 (00:00:01)
    Task 4460 | 04:31:53 | Creating new packages: boost/3eb8bdb1abb7eff5b63c4c5bdb41c0a778925c31 (00:00:01)
    Task 4460 | 04:31:54 | Creating new packages: common/ba480a46c4b2aa9484fb24ed01a8649453573e6f (00:00:00)
    Task 4460 | 04:31:54 | Creating new packages: switchboard/fad565dadbb37470771801952001c7071e55a364 (00:00:01)
    Task 4460 | 04:31:55 | Creating new packages: op-mysql-java-broker/3bf47851b2c0d3bea63a0c58452df58c14a15482 (00:00:01)
    Task 4460 | 04:31:56 | Creating new packages: golang/f57ddbc8d55d7a0f08775bf76bb6a27dc98c7ea7 (00:00:01)
    Task 4460 | 04:31:57 | Creating new jobs: cf-mysql-broker/9828ead15eabdc33b2c27fe275b463735edb115d (00:00:00)
    Task 4460 | 04:31:57 | Creating new jobs: acceptance-tests/48c00c36ec5210cbdd3b125ae6a72cfdf6eaf4e2 (00:00:00)
    Task 4460 | 04:31:57 | Creating new jobs: broker-deregistrar/b5f6f776d46eb1ac561ab1e8f58d8ddedb97f86e (00:00:00)
    Task 4460 | 04:31:57 | Creating new jobs: proxy/7907d8759aa11dfcbbe79220dc945c96b5562ac1 (00:00:00)
    Task 4460 | 04:31:57 | Creating new jobs: mysql/078561f02f2516212ed59c48e1dd45360f93871c (00:00:00)
    Task 4460 | 04:31:57 | Creating new jobs: op-mysql-java-broker/6e47c9ea6fbe0867d4a476af5abf157830c03024 (00:00:00)
    Task 4460 | 04:31:57 | Creating new jobs: broker-registrar/e1f5e30b87e70e916ea74ea8eb63a7b6ff6ff643 (00:00:00)
    Task 4460 | 04:31:57 | Release has been created: paasta-mysql/2.0 (00:00:00)

    Task 4460 Started  Fri Aug 31 04:31:41 UTC 2018
    Task 4460 Finished Fri Aug 31 04:31:57 UTC 2018
    Task 4460 Duration 00:00:16
    Task 4460 done

    Succeeded
  ```

* 업로드 된 배포 파이프라인 서비스 릴리즈를 확인한다.
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
    paasta-delivery-pipeline-release  1.0       b3ee8f48+  
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

* 배포 파이프라인 서비스 릴리즈가 업로드 되어 있는 것을 확인
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

> Stemcell 목록이 존재 하지 않을 경우 BOSH 설치 가이드 문서를 참고 하여 Stemcell을 업로드를 해야 한다.

### 2.3 배포 파이프라인 서비스 Deployment 파일 수정 및 배포

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
# paasta-delivery-pipeline-service 설정 파일 내용
name: paasta-delivery-pipeline-service                      # 서비스 배포이름(필수)

releases:
- name: paasta-delivery-pipeline-release                    # 서비스 릴리즈 이름(필수)
  version: "latest"                                        # 서비스 릴리즈 버전(필수):latest 시 업로드된 서비스 릴리즈 최신버전

stemcells:
- alias: default
  os: ((stemcell_os))
  version: "((stemcell_version))"

update:
  canaries: 1                            # canary 인스턴스 수(필수)
  canary_watch_time: 30000-120000        # canary 인스턴스가 수행하기 위한 대기 시간(필수)
  max_in_flight: 1                       # non-canary 인스턴스가 병렬로 update 하는 최대 개수(필수)
  update_watch_time: 30000-120000        # non-canary 인스턴스가 수행하기 위한 대기 시간(필수)

instance_groups:
- name: mariadb
  azs:
  - z5
  instances: 1
  persistent_disk_type: 2GB
  vm_type: ((vm_type_small))
  stemcell: default
  networks:
  - name: ((default_network_name))
    static_ips:
    - 10.30.107.68
  templates:
  - name: mariadb
    release: paasta-delivery-pipeline-release

- name: postgres
  azs:
  - z5
  instances: 1
  persistent_disk_type: 2GB
  vm_type: ((vm_type_small))
  stemcell: default
  networks:
  - name: ((default_network_name))
    static_ips:
    - 10.30.107.82
  templates:
  - name: postgres
    release: paasta-delivery-pipeline-release

- name: inspection
  azs:
  - z5
  instances: 1
  vm_type: ((vm_type_small))
  stemcell: default
  networks:
  - name: ((default_network_name))
    static_ips:
    - 10.30.107.69
  templates:
  - name: inspection
    release: paasta-delivery-pipeline-release

- name: haproxy
  azs:
  - z5
  instances: 1
  vm_type: ((vm_type_small))
  stemcell: default
  networks:
  - name: ((default_network_name))
    static_ips:
    - 10.30.107.70
  - name: ((public_network_name))
    static_ips:
    - 115.68.47.175
  templates:
  - name: haproxy
    release: paasta-delivery-pipeline-release

- name: ci_server
  azs:
  - z5
  instances: 2
  persistent_disk_type: 4GB
  vm_type: ((vm_type_small))
  stemcell: default
  networks:
  - name: ((default_network_name))
    static_ips:
    - 10.30.107.71
    - 10.30.107.72
  templates:
  - name: ci_server
    release: paasta-delivery-pipeline-release
  env:
    bosh:
      password: $6$4gDD3aV0rdqlrKC$2axHCxGKIObs6tAmMTqYCspcdvQXh3JJcvWOY2WGb4SrdXtnCyNaWlrf3WEqvYR2MYizEGp3kMmbpwBC6jsHt0

- name: binary_storage
  azs:
  - z5
  instances: 1
  persistent_disk_type: 4GB
  vm_type: ((vm_type_small))
  stemcell: default
  networks:
  - name: ((default_network_name))
    static_ips:
    - 10.30.107.39
  templates:
  - name: binary_storage
    release: paasta-delivery-pipeline-release

- name: delivery-pipeline-common-api
  azs:
  - z5
  instances: 1
  vm_type: ((vm_type_small))
  stemcell: default
  networks:
  - name: ((default_network_name))
    static_ips:
    - 10.30.107.66
  templates:
  - name: delivery-pipeline-common-api
    release: paasta-delivery-pipeline-release

- name: delivery-pipeline-inspection-api
  azs:
  - z5
  instances: 1
  vm_type: ((vm_type_small))
  stemcell: default
  networks:
  - name: ((default_network_name))
    static_ips:
    - 10.30.107.62
  templates:
  - name: delivery-pipeline-inspection-api
    release: paasta-delivery-pipeline-release

- name: delivery-pipeline-binary-storage-api
  azs:
  - z5
  instances: 1
  vm_type: ((vm_type_small))
  stemcell: default
  networks:
  - name: ((default_network_name))
    static_ips:
    - 10.30.107.61
  templates:
  - name: delivery-pipeline-binary-storage-api
    release: paasta-delivery-pipeline-release

- name: delivery-pipeline-api
  azs:
  - z5
  instances: 1
  vm_type: ((vm_type_small))
  stemcell: default
  networks:
  - name: ((default_network_name))
    static_ips:
    - 10.30.107.65
  templates:
  - name: delivery-pipeline-api
    release: paasta-delivery-pipeline-release

- name: delivery-pipeline-service-broker
  azs:
  - z5
  instances: 1
  vm_type: ((vm_type_small))
  stemcell: default
  networks:
  - name: ((default_network_name))
    static_ips:
    - 10.30.107.64
  templates:
  - name: delivery-pipeline-service-broker
    release: paasta-delivery-pipeline-release

- name: delivery-pipeline-ui
  azs:
  - z5
  instances: 1
  vm_type: ((vm_type_small))
  stemcell: default
  networks:
  - name: ((default_network_name))
    static_ips:
    - 10.30.107.67
  templates:
  - name: delivery-pipeline-ui
    release: paasta-delivery-pipeline-release

- name: delivery-pipeline-scheduler
  azs:
  - z5
  instances: 1
  vm_type: ((vm_type_small))
  stemcell: default
  networks:
  - name: ((default_network_name))
    static_ips:
    - 10.30.107.63
  templates:
  - name: delivery-pipeline-scheduler
    release: paasta-delivery-pipeline-release

properties:
  cf:                                                      # CLOUD FOUNDRY 설정 정보
    uaa:
      oauth:
        info:
          uri: https://uaa.115.68.46.189.xip.io/userinfo
        token:
          check:
            uri: https://uaa.115.68.46.189.xip.io/check_token
          access:
            uri: https://uaa.115.68.46.189.xip.io/oauth/token
        logout:
          uri: https://uaa.115.68.46.189.xip.io/logout.do
        authorization:
          uri: https://uaa.115.68.46.189.xip.io/oauth/authorize
        client:
          id: pipeclient
          secret: clientsecret
    api:
      url: https://api.115.68.46.189.xip.io/v2/service_instances/[SUID]/permissions

  ci_server:                                               # CI SERVER 설정 정보
    password: '!paas_ta202'
    admin_user:
      username: 'admin'
      password: '!paas_ta202'
    http_url: '10.30'                                      #내부 IP주소의 앞의 두자리수를 입력해야한다. 예 10.110.10.10 일경우 10.110을 입력해줘야한다. 
    http_port: 8088
    ajp13_port: 8009
    ssh:
      username: vcap
      password: c1oudc0w
      port: 22
      identity: "null"                                     # PERM KEY 경로
      key: "null"
    credentials:
      scope: "GLOBAL"
      url: "/credentials/store/system/domain/_"
      class_name: "com.cloudbees.plugins.credentials.impl.UsernamePasswordCredentialsImpl"
    job:
      name: "ci_server"                                    # JOB 이름과 동일하게 설정
    shared:                                                # Shared 서비스 설정 정보
      urls:
        - http://10.30.107.71:8088
    dedicated:                                             # Dedicated 서비스 설정 정보
      urls:
        - http://10.30.107.72:8088


  mariadb:                                                 # MARIA DB SERVER 설정 정보
    port: 3306
    admin_user:
      password: "!paas_ta202"
    host: 10.30.107.68
    host_names:
    - mariadb0
    host_ips:
    - 10.30.107.68
    datasource:
      url: jdbc:mysql://10.30.107.68:3306/delivery_pipeline?autoReconnect=true&useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Seoul&useLegacyDatetimeCode=false
      username: root
      password: "!paas_ta202"
      driver_class_name: com.mysql.cj.jdbc.Driver
    jpa:
      database:
        name: mysql

  postgres:                                                # POSTGRESQL SERVER 설정 정보
    port: 5432
    host: 10.30.107.82
    vcap_password: c1oudc0w
    datasource:
      url: jdbc:postgresql://10.30.107.82:5432/sonar
      username: "sonar"
      password: "sonar"
      database: "sonar"

  inspection:                                              # INSPECTION SERVER 설정 정보
    url: 10.30.107.69
    http_url: 'http://10.30.107.69'
    http_port: 9000
    admin:
      username: admin
      password: admin

  binary_storage:                                          # BINARY STORAGE SERVER 설정 정보
    proxy_ip: 10.30.107.39                                 # 프록시 서버 IP(swift-keystone job의 static_ip, Object Storage 접속 IP)
    proxy_port: 10008                                      # 프록시 서버 Port(Object Storage 접속 Port)
    default_username: paasta-pipeline                      # 최초 생성되는 유저이름(Object Storage 접속 유저이름)
    default_password: paasta-pipeline                      # 최초 생성되는 유저 비밀번호(Object Storage 접속 유저 비밀번호)
    default_tenantname: paasta-pipeline                    # 최초 생성되는 테넌트 이름(Object Storage 접속 테넌트 이름)
    default_email: email@email.com                         # 최소 생성되는 유저의 이메일
    container: delivery-pipeline-container
    auth_port: 5000

  common_api:                                              # COMMON API 설정 정보
    url: http://10.30.107.70
    server:
      port: 8081
    authorization:
      id: admin
      password: PaaS-TA
    logging:
      level: INFO
    haproxy:
      urls:
        - 10.30.107.66
    java_opts: '-XX:MaxMetaspaceSize=104857K -Xss349K -Xms681574K -XX:MetaspaceSize=104857K -Xmx681574K'

  pipeline_api:                                            # CI API 설정 정보
    url: http://10.30.107.70
    server:
      port: 8082
    authorization:
      id: admin
      password: PaaS-TA
    http:
      multipart:
        max_file_size: 1000Mb
        max_request_size: 1000Mb
    logging:
      level: INFO
    haproxy:
      urls:
        - 10.30.107.65
    java_opts: '-XX:MaxMetaspaceSize=104857K -Xss349K -Xms681574K -XX:MetaspaceSize=104857K -Xmx681574K'

  inspection_api:                                          # INSPECTION API 설정 정보
    url: http://10.30.107.70
    server:
      port: 8083
    http:
      multipart:
        max_file_size: 1000Mb
        max_request_size: 1000Mb
    logging:
      level: INFO
    authorization:
      id: admin
      password: PaaS-TA
    haproxy:
      urls:
        - 10.30.107.62
    java_opts: '-XX:MaxMetaspaceSize=104857K -Xss349K -Xms681574K -XX:MetaspaceSize=104857K -Xmx681574K'

  binary_storage_api:                                      # BINARY STORAGE API 설정 정보
    http:
      multipart:
        max_file_size: 1000Mb
        max_request_size: 1000Mb
    server:
      port: 8080
    logging:
      level: INFO
    url: http://10.30.107.61
    authorization:
      id: admin
      password: PaaS-TA
    java_opts: '-XX:MaxMetaspaceSize=104857K -Xss349K -Xms681574K -XX:MetaspaceSize=104857K -Xmx681574K'

  pipeline_ui:                                              # UI 설정 정보
    url: http://10.30.107.70
    server:
      port: 8084
    http:
      multipart:
        max_file_size: 1000Mb
        max_request_size: 1000Mb
    logging:
      level: INFO
    haproxy:
      urls:
        - 10.30.107.67
    java_opts: '-XX:MaxMetaspaceSize=104857K -Xss349K -Xms681574K -XX:MetaspaceSize=104857K -Xmx681574K'

  pipeline_scheduler:                                      # SCHEDULER 설정 정보
    server:
      port: 8080
    logging:
      level: INFO
    quartz:
      scheduler:
        instance_name: paastaDeliveryPipelineScheduler
        instance_id: AUTO
      thread_pool:
        thread_count: 5
    job:
      start_delay: 0
      repeat_interval: 5000
      description: PaaS-TA Delivery Pipeline Scheduler
      key: StatisticsJob
    java_opts: '-XX:MaxMetaspaceSize=104857K -Xss349K -Xms681574K -XX:MetaspaceSize=104857K -Xmx681574K'

  pipeline_service_broker:                                 # SERVICE BROKER 설정 정보
    server:
      port: 8080
    logging:
      controller:
        level: INFO
      service:
        level: INFO
    dashboard:
      url: http://115.68.47.175:8084/dashboard/[SUID]/
    java_opts: '-XX:MaxMetaspaceSize=104857K -Xss349K -Xms681574K -XX:MetaspaceSize=104857K -Xmx681574K'

  haproxy:                                                 # HAPROXY 설정 정보
    url: 10.30.107.70
    http_port: 8080
```

* deploy-delivery\_pipeline-bosh2.0.sh 파일을 서버 환경에 맞게 수정한다.

```bash
#!/bin/bash

bosh -e micro-bosh -d paasta-delivery-pipeline-service deploy paasta_delivery_pipeline_bosh2.0.yml \
   -o use-public-network-vsphere.yml \
   -v default_network_name=service_private \
   -v public_network_name=service_public \
   -v stemcell_os=ubuntu-xenial \
   -v stemcell_version=315.41 \
   -v vm_type_small=minimal
```

* 배포 파이프라인 서비스팩을 배포한다.
* **사용 예시**

  ```text
    $ ./deploy-delivery_pipeline-bosh2.0.sh
    Using environment '10.30.40.111' as user 'admin' (openid, bosh.admin)

    Using deployment 'paasta-delivery-pipeline-service'
    Task 4506
  ```

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
* * host: 3306
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
* * alias: default
* os: ubuntu-xenial
* version: '315.41'
* releases:
* * name: paasta-delivery-pipeline-release
* version: '1.0'
* update:
* canaries: 1
* canary\_watch\_time: 30000-120000
* max\_in\_flight: 1
* update\_watch\_time: 30000-120000
* variables:
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
* instance\_groups:
* * azs:
* * z5
* instances: 1
* jobs:
* * name: mariadb
* release: paasta-delivery-pipeline-release
* name: mariadb
* networks:
* * name: service\_private
* static\_ips:
* * 10.30.54.68
* persistent\_disk\_type: 2GB
* stemcell: default
* vm\_type: minimal
* * azs:
* * z5
* instances: 1
* jobs:
* * name: postgres
* release: paasta-delivery-pipeline-release
* name: postgres
* networks:
* * name: service\_private
* static\_ips:
* * 10.30.54.82
* persistent\_disk\_type: 2GB
* stemcell: default
* vm\_type: minimal
* * azs:
* * z5
* instances: 1
* jobs:
* * name: inspection
* release: paasta-delivery-pipeline-release
* name: inspection
* networks:
* * name: service\_private
* static\_ips:
* * 10.30.54.69
* stemcell: default
* vm\_type: minimal
* * azs:
* * z5
* instances: 1
* jobs:
* * name: haproxy
* release: paasta-delivery-pipeline-release
* name: haproxy
* networks:
* * name: service\_private
* static\_ips:
* * 10.30.54.70
* * default:
* * dns
* * gateway
* name: service\_public
* static\_ips:
* * 115.68.46.182
* stemcell: default
* vm\_type: minimal
* * azs:
* * z5
* env:
* bosh:
* password: ""
* instances: 2
* jobs:
* * name: ci\_server
* release: paasta-delivery-pipeline-release
* name: ci\_server
* networks:
* * name: service\_private
* static\_ips:
* * 10.30.54.71
* * 10.30.54.72
* persistent\_disk\_type: 4GB
* stemcell: default
* vm\_type: minimal
* * azs:
* * z5
* instances: 1
* jobs:
* * name: binary\_storage
* release: paasta-delivery-pipeline-release
* name: binary\_storage
* networks:
* * name: service\_private
* static\_ips:
* * 10.30.54.39
* persistent\_disk\_type: 4GB
* stemcell: default
* vm\_type: minimal
* * azs:
* * z5
* instances: 1
* jobs:
* * name: delivery-pipeline-common-api
* release: paasta-delivery-pipeline-release
* name: delivery-pipeline-common-api
* networks:
* * name: service\_private
* static\_ips:
* * 10.30.54.66
* stemcell: default
* vm\_type: minimal
* * azs:
* * z5
* instances: 1
* jobs:
* * name: delivery-pipeline-inspection-api
* release: paasta-delivery-pipeline-release
* name: delivery-pipeline-inspection-api
* networks:
* * name: service\_private
* static\_ips:
* * 10.30.54.62
* stemcell: default
* vm\_type: minimal
* * azs:
* * z5
* instances: 1
* jobs:
* * name: delivery-pipeline-binary-storage-api
* release: paasta-delivery-pipeline-release
* name: delivery-pipeline-binary-storage-api
* networks:
* * name: service\_private
* static\_ips:
* * 10.30.54.61
* stemcell: default
* vm\_type: minimal
* * azs:
* * z5
* instances: 1
* jobs:
* * name: delivery-pipeline-api
* release: paasta-delivery-pipeline-release
* name: delivery-pipeline-api
* networks:
* * name: service\_private
* static\_ips:
* * 10.30.54.65
* stemcell: default
* vm\_type: minimal
* * azs:
* * z5
* instances: 1
* jobs:
* * name: delivery-pipeline-service-broker
* release: paasta-delivery-pipeline-release
* name: delivery-pipeline-service-broker
* networks:
* * name: service\_private
* static\_ips:
* * 10.30.54.64
* stemcell: default
* vm\_type: minimal
* * azs:
* * z5
* instances: 1
* jobs:
* * name: delivery-pipeline-ui
* release: paasta-delivery-pipeline-release
* name: delivery-pipeline-ui
* networks:
* * name: service\_private
* static\_ips:
* * 10.30.54.67
* stemcell: default
* vm\_type: minimal
* * azs:
* * z5
* instances: 1
* jobs:
* * name: delivery-pipeline-scheduler
* release: paasta-delivery-pipeline-release
* name: delivery-pipeline-scheduler
* networks:
* * name: service\_private
* static\_ips:
* * 10.30.54.63
* stemcell: default
* vm\_type: minimal
* name: paasta-delivery-pipeline-service
* properties:
* binary\_storage:
* auth\_port: ""
* binary\_desc: ""
* container: ""
* email: ""
* password: ""
* proxy\_ip: ""
* proxy\_port: ""
* tenantname: ""
* username: ""
* binary\_storage\_api:
* authorization:
* id: ""
* password: ""
* http:
* multipart:
* max\_file\_size: ""
* max\_request\_size: ""
* java\_opts: ""
* logging:
* level: ""
* server:
* port: ""
* url: ""
* cf:
* api:
* url: ""
* uaa:
* oauth:
* authorization:
* uri: ""
* client:
* id: ""
* secret: ""
* info:
* uri: ""
* logout:
* uri: ""
* token:
* access:
* uri: ""
* check:
* uri: ""
* ci\_server:
* admin\_user:
* password: ""
* username: ""
* ajp13\_port: ""
* credentials:
* class\_name: ""
* scope: ""
* url: ""
* dedicated:
* urls:
* * ""
* http\_port: ""
* http\_url: ""
* job:
* name: ""
* password: ""
* shared:
* urls:
* * ""
* ssh:
* identity: ""
* key: ""
* password: ""
* port: ""
* username: ""
* common\_api:
* authorization:
* id: ""
* password: ""
* haproxy:
* urls:
* * ""
* java\_opts: ""
* logging:
* level: ""
* server:
* port: ""
* url: ""
* haproxy:
* http\_port: ""
* url: ""
* inspection:
* admin:
* password: ""
* username: ""
* http\_port: ""
* http\_url: ""
* url: ""
* inspection\_api:
* authorization:
* id: ""
* password: ""
* haproxy:
* urls:
* * ""
* http:
* multipart:
* max\_file\_size: ""
* max\_request\_size: ""
* java\_opts: ""
* logging:
* level: ""
* server:
* port: ""
* url: ""
* mariadb:
* admin\_user:
* password: ""
* datasource:
* driver\_class\_name: ""
* password: ""
* url: ""
* username: ""
* host: ""
* host\_ips:
* * ""
* host\_names:
* * ""
* jpa:
* database:
* name: ""
* port: ""
* pipeline\_api:
* authorization:
* id: ""
* password: ""
* haproxy:
* urls:
* * ""
* http:
* multipart:
* max\_file\_size: ""
* max\_request\_size: ""
* java\_opts: ""
* logging:
* level: ""
* server:
* port: ""
* url: ""
* pipeline\_scheduler:
* java\_opts: ""
* job:
* description: ""
* key: ""
* repeat\_interval: ""
* start\_delay: ""
* logging:
* level: ""
* quartz:
* scheduler:
* instance\_id: ""
* instance\_name: ""
* thread\_pool:
* thread\_count: ""
* server:
* port: ""
* pipeline\_service\_broker:
* dashboard:
* url: ""
* java\_opts: ""
* logging:
* controller:
* level: ""
* service:
* level: ""
* server:
* port: ""
* pipeline\_ui:
* haproxy:
* urls:
* * ""
* http:
* multipart:
* max\_file\_size: ""
* max\_request\_size: ""
* java\_opts: ""
* logging:
* level: ""
* server:
* port: ""
* url: ""
* postgres:
* datasource:
* database: ""
* password: ""
* url: ""
* username: ""
* host: ""
* port: ""
* vcap\_password: "" Continue? \[yN\]: Y

  Task 4506 \| 06:04:10 \| Preparing deployment: Preparing deployment \(00:00:01\) Task 4506 \| 06:04:12 \| Preparing package compilation: Finding packages to compile \(00:00:00\) Task 4506 \| 06:04:12 \| Compiling packages: delivery-pipeline-scheduler/01dea40e305407b6b107a8fe230bb37dadadca87 Task 4506 \| 06:04:12 \| Compiling packages: openjdk-1.8.0\_45/57e0ee876ea9d90f5470e3784ae1171bccee850a Task 4506 \| 06:04:12 \| Compiling packages: delivery-pipeline-service-broker/3bf47851b2c0d3bea63a0c58452df58c14a15482 Task 4506 \| 06:04:12 \| Compiling packages: delivery-pipeline-api/078da6dcb999c1e6f5398a6eb739182ccb4aba25 Task 4506 \| 06:04:12 \| Compiling packages: delivery-pipeline-binary-storage-api/ba480a46c4b2aa9484fb24ed01a8649453573e6f Task 4506 \| 06:06:53 \| Compiling packages: syslog\_aggregator/078da6dcb999c1e6f5398a6eb739182ccb4aba25 \(00:02:41\) Task 4506 \| 06:06:53 \| Compiling packages: golang/f57ddbc8d55d7a0f08775bf76bb6a27dc98c7ea7 Task 4506 \| 06:06:55 \| Compiling packages: common/ba480a46c4b2aa9484fb24ed01a8649453573e6f \(00:02:43\) Task 4506 \| 06:06:55 \| Compiling packages: python/4e255efa754d91b825476b57e111345f200944e1 Task 4506 \| 06:06:55 \| Compiling packages: cli/24305e50a638ece2cace4ef4803746c0c9fe4bb0 \(00:02:43\) Task 4506 \| 06:06:55 \| Compiling packages: check/d6811f25e9d56428a9b942631c27c9b24f5064dc Task 4506 \| 06:07:05 \| Compiling packages: op-mysql-java-broker/3bf47851b2c0d3bea63a0c58452df58c14a15482 \(00:02:53\) Task 4506 \| 06:07:05 \| Compiling packages: boost/3eb8bdb1abb7eff5b63c4c5bdb41c0a778925c31 Task 4506 \| 06:07:10 \| Compiling packages: openjdk-1.8.0\_45/57e0ee876ea9d90f5470e3784ae1171bccee850a \(00:02:58\) Task 4506 \| 06:43:56 \| Compiling packages: mariadb/2e701e7a9e4241b28052d984733de36aae152275 \(00:10:26\) Task 4506 \| 06:55:22 \| Creating missing vms: mariadb/ea075ae6-6326-478b-a1ba-7fbb0b5b0bf5 \(0\) Task 4506 \| 06:55:22 \| Creating missing vms: mysql/e8c52bf2-cd48-45d0-9553-f6367942a634 \(2\) Task 4506 \| 06:55:22 \| Creating missing vms: proxy/023edddd-418e-46e4-8d40-db452c694e16 \(0\) Task 4506 \| 06:55:22 \| Creating missing vms: postgres/8a830154-25b6-432a-ad39-9ff09d015760 \(1\) Task 4506 \| 06:55:22 \| Creating missing vms: paasta-mysql-java-broker/bb5676ca-efba-48fc-bc11-f464d0ae9c78 \(0\) Task 4506 \| 06:57:18 \| Creating missing vms: mysql/ea075ae6-6326-478b-a1ba-7fbb0b5b0bf5 \(0\) \(00:01:56\) Task 4506 \| 06:57:23 \| Creating missing vms: ci\_server/023edddd-418e-46e4-8d40-db452c694e16 \(0\) \(00:02:01\) Task 4506 \| 06:57:23 \| Creating missing vms: mysql/e8c52bf2-cd48-45d0-9553-f6367942a634 \(2\) \(00:02:01\) Task 4506 \| 06:57:23 \| Creating missing vms: delivery-pipeline-service-broker/bb5676ca-efba-48fc-bc11-f464d0ae9c78 \(0\) \(00:02:01\) Task 4506 \| 06:57:23 \| Creating missing vms: mysql/8a830154-25b6-432a-ad39-9ff09d015760 \(1\) \(00:02:01\) Task 4506 \| 06:57:24 \| Updating instance mysql: mysql/ea075ae6-6326-478b-a1ba-7fbb0b5b0bf5 \(0\) \(canary\) \(00:02:32\) Task 4506 \| 06:59:56 \| Updating instance mysql: mysql/8a830154-25b6-432a-ad39-9ff09d015760 \(1\) \(00:03:03\) Task 4506 \| 07:02:59 \| Updating instance mysql: mysql/e8c52bf2-cd48-45d0-9553-f6367942a634 \(2\) \(00:03:04\) Task 4506 \| 07:06:03 \| Updating instance proxy: proxy/023edddd-418e-46e4-8d40-db452c694e16 \(0\) \(canary\) \(00:01:01\) Task 4506 \| 07:07:04 \| Updating instance delivery-pipeline-service-broker: paasta-mysql-java-broker/bb5676ca-efba-48fc-bc11-f464d0ae9c78 \(0\) \(canary\) \(00:01:02\)

  Task 4506 Started Fri Aug 31 06:04:10 UTC 2018 Task 4506 Finished Fri Aug 31 07:08:06 UTC 2018 Task 4506 Duration 01:03:56 Task 4506 done

  Succeeded

* 배포된 배포파이프라인 서비스팩을 확인한다.
* **사용 예시**

  ```text
    $bosh -e micro-bosh -d paasta-delivery-pipeline-service vms
    Using environment '10.30.40.111' as user 'admin' (openid, bosh.admin)

    Task 7819. Done

    Deployment 'paasta-delivery-pipeline-service'

    Instance                                                                   Process State  AZ  IPs            VM CID                                   VM Type  Active  
    binary_storage/4c54c4a5-9a63-4b7f-acd7-afe2937af04d                        running        z5  10.30.107.39   vm-0ea717f1-8f07-4924-bbc7-6bafbe6fbea3  minimal  true  
    ci_server/24ed965d-51a0-4477-8c80-fa4e4c2502a7                             running        z5  10.30.107.72   vm-5e8639c8-3a41-4ed9-9eb1-e1e48ff17b6d  minimal  true  
    ci_server/fe3e03eb-55f8-4980-9f0b-bbb396bd02cf                             running        z5  10.30.107.71   vm-cc8dd7ed-237a-4c48-b23b-ff9113c64f3d  minimal  true  
    delivery-pipeline-api/de15549f-a7e4-4dd8-867d-958c7f6d8e18                 running        z5  10.30.107.65   vm-ab366342-771c-484a-8a4a-be7563f12add  minimal  true  
    delivery-pipeline-binary-storage-api/dcfdeca5-8a09-4090-addf-d64f1e910063  running        z5  10.30.107.61   vm-a2042942-01e9-45b5-bc43-52354d02c8d0  minimal  true  
    delivery-pipeline-common-api/700424b3-4c81-4c72-88ed-99d2f6d2d368          running        z5  10.30.107.66   vm-f65d3452-7da4-428d-8cb4-7406f20708fe  minimal  true  
    delivery-pipeline-inspection-api/46f771f3-dcb6-40de-a727-52bcb4e014e5      running        z5  10.30.107.62   vm-a56941ab-2305-4a9d-96ed-4db17d84aa0b  minimal  true  
    delivery-pipeline-scheduler/fd17f7c8-7bd3-4c68-8c22-542d3013a90b           running        z5  10.30.107.63   vm-4dc86c13-ab35-4424-8205-1b461325243e  minimal  true  
    delivery-pipeline-service-broker/67243664-06d9-4915-9e8f-534812bd099c      running        z5  10.30.107.64   vm-06a9756a-07df-4f85-a422-fd652f61cb66  minimal  true  
    delivery-pipeline-ui/a9229a2a-6d25-4755-954e-fbe97e11c3ea                  running        z5  10.30.107.67   vm-0a095d59-eb00-4a35-b93f-475d5250bca9  minimal  true  
    haproxy/65081aeb-50e0-49bc-a6c1-131f74d52a44                               running        z5  10.30.107.70   vm-111c720f-e640-40f3-8c10-d252fca5b4ec  minimal  true  
                                                      115.68.47.175                                                      
    inspection/a5789621-8615-417a-9a8a-6fccddb38b4b                            running        z5  10.30.107.69   vm-09df943c-704f-4952-b9fe-054d74d271b6  minimal  true  
    mariadb/2d876573-1248-48b0-9a00-92463cc33978                               running        z5  10.30.107.68   vm-76ee37ce-e99b-4a59-b0a5-f860f284b924  minimal  true  
    postgres/3226e31b-e86f-40bb-b0d8-31755b087699                              running        z5  10.30.107.82   vm-060c3a81-4a89-4370-9777-c06e280a83c6  minimal  true  

    14 vms

    Succeeded
  ```

### 2.4 배포 파이프라인 서비스 브로커 등록

배포 파이프라인 서비스팩 배포가 완료되었으면 파스-타 포탈에서 서비스 팩을 사용하기 위해서 먼저 배포 파이프라인 서비스 브로커를 등록해 주어야 한다. 서비스 브로커 등록 시 개방형 클라우드 플랫폼에서 서비스 브로커를 등록할 수 있는 사용자로 로그인이 되어있어야 한다.

#### 서비스 브로커 목록을 확인한다.

> `$ cf service-brokers`
>
> \`\`\` Getting service brokers as admin...

name url cubrid-service-broker [http://10.30.60.22:8080](http://10.30.60.22:8080) glusterfs-service [http://10.30.120.197:8080](http://10.30.120.197:8080) mysql-service-broker [http://10.30.40.195:8080](http://10.30.40.195:8080) paasta-redis-broker [http://10.30.60.71:12350](http://10.30.60.71:12350)

```text
##### 배포 파이프라인 서비스 브로커를 등록한다.
>`$ cf create-service-broker {서비스팩 이름}{서비스팩 사용자ID}{서비스팩 사용자비밀번호} http://{서비스팩 URL}`

  **서비스팩 이름** : 서비스 팩 관리를 위해 PaaS-TA에서 보여지는 명칭이다. 서비스 Marketplace에서는 각각의 API 서비스 명이 보여지니 여기서 명칭은 서비스팩 리스트의 명칭이다.<br>
  **서비스팩 사용자ID** / 비밀번호 : 서비스팩에 접근할 수 있는 사용자 ID입니다. 서비스팩도 하나의 API 서버이기 때문에 아무나 접근을 허용할 수 없어 접근이 가능한 ID/비밀번호를 입력한다.<br>
  **서비스팩 URL** : 서비스팩이 제공하는 API를 사용할 수 있는 URL을 입력한다.

>`$ cf create-service-broker delivery-pipeline admin cloudfoundry http://10.30.107.64:8080`

##### 등록된 배포 파이프라인 서비스 브로커를 확인한다.

>`$ cf service-brokers`
```

Getting service brokers as admin...

name url cubrid-service-broker [http://10.30.60.22:8080](http://10.30.60.22:8080) glusterfs-service [http://10.30.120.197:8080](http://10.30.120.197:8080) delivery-pipeline [http://10.30.107.64:8080](http://10.30.107.64:8080) mysql-service-broker [http://10.30.40.195:8080](http://10.30.40.195:8080) paasta-redis-broker [http://10.30.60.71:12350](http://10.30.60.71:12350)

```text
##### 접근 가능한 서비스 목록을 확인한다.
>`$ cf service-access`
```

서비스 브로커 생성시 디폴트로 접근을 허용하지 않는다. Getting service access as admin... broker: paasta-redis-broker service plan access orgs redis shared-vm all redis dedicated-vm all

broker: cubrid-service-broker service plan access orgs CubridDB utf8 all CubridDB euckr all

broker: glusterfs-service service plan access orgs glusterfs glusterfs-5Mb all glusterfs glusterfs-100Mb all glusterfs glusterfs-1000Mb all

broker: mysql-service-broker service plan access orgs Mysql-DB Mysql-Plan1-10con all Mysql-DB Mysql-Plan2-100con all

broker: rabbitmq-service-broker service plan access orgs p-rabbitmq standard all

broker: delivery-pipeline service plan access orgs delivery-pipeline default none

```text
##### 특정 조직에 해당 서비스 접근 허용을 할당하고 접근 서비스 목록을 다시 확인한다. (전체 조직)
>`$ cf enable-service-access delivery-pipeline`
```

Getting service access as admin... broker: paasta-redis-broker service plan access orgs redis shared-vm all redis dedicated-vm all

broker: cubrid-service-broker service plan access orgs CubridDB utf8 all CubridDB euckr all

broker: glusterfs-service service plan access orgs glusterfs glusterfs-5Mb all glusterfs glusterfs-100Mb all glusterfs glusterfs-1000Mb all

broker: mysql-service-broker service plan access orgs Mysql-DB Mysql-Plan1-10con all Mysql-DB Mysql-Plan2-100con all

broker: rabbitmq-service-broker service plan access orgs p-rabbitmq standard all

broker: delivery-pipeline service plan access orgs delivery-pipeline default all

```text
### <div id='25'/> 2.5 배포 파이프라인 UAA Client Id 등록
UAA 포털 계정 등록 절차에 대한 순서를 확인한다.

**haproxy IPs**:8084 입력 후 루트 도메인을 입력한다. **haproxy IPs**의 주소는 **실제 대시보드 URL**이다. URL 정보 다중 입력 시 ","(comma)를 사용하여 추가한다.
>`$ http://115.68.47.175:8084/dashboard/c6f57069-dc64-460d-9fa2-d6a431580c22` 

##### *haproxy IPs 확인*
>`$ bosh -e micro-bosh -d paasta-delivery-pipeline-service vms`
```

Deployment 'paasta-delivery-pipeline-service'

Instance Process State AZ IPs VM CID VM Type Active binary\_storage/4c54c4a5-9a63-4b7f-acd7-afe2937af04d running z5 10.30.107.39 vm-0ea717f1-8f07-4924-bbc7-6bafbe6fbea3 minimal true ci\_server/24ed965d-51a0-4477-8c80-fa4e4c2502a7 running z5 10.30.107.72 vm-5e8639c8-3a41-4ed9-9eb1-e1e48ff17b6d minimal true ci\_server/fe3e03eb-55f8-4980-9f0b-bbb396bd02cf running z5 10.30.107.71 vm-cc8dd7ed-237a-4c48-b23b-ff9113c64f3d minimal true delivery-pipeline-api/de15549f-a7e4-4dd8-867d-958c7f6d8e18 running z5 10.30.107.65 vm-ab366342-771c-484a-8a4a-be7563f12add minimal true delivery-pipeline-binary-storage-api/dcfdeca5-8a09-4090-addf-d64f1e910063 running z5 10.30.107.61 vm-a2042942-01e9-45b5-bc43-52354d02c8d0 minimal true delivery-pipeline-common-api/700424b3-4c81-4c72-88ed-99d2f6d2d368 running z5 10.30.107.66 vm-f65d3452-7da4-428d-8cb4-7406f20708fe minimal true delivery-pipeline-inspection-api/46f771f3-dcb6-40de-a727-52bcb4e014e5 running z5 10.30.107.62 vm-a56941ab-2305-4a9d-96ed-4db17d84aa0b minimal true delivery-pipeline-scheduler/fd17f7c8-7bd3-4c68-8c22-542d3013a90b running z5 10.30.107.63 vm-4dc86c13-ab35-4424-8205-1b461325243e minimal true delivery-pipeline-service-broker/67243664-06d9-4915-9e8f-534812bd099c running z5 10.30.107.64 vm-06a9756a-07df-4f85-a422-fd652f61cb66 minimal true delivery-pipeline-ui/a9229a2a-6d25-4755-954e-fbe97e11c3ea running z5 10.30.107.67 vm-0a095d59-eb00-4a35-b93f-475d5250bca9 minimal true haproxy/65081aeb-50e0-49bc-a6c1-131f74d52a44 running z5 10.30.107.70 vm-111c720f-e640-40f3-8c10-d252fca5b4ec minimal true 115.68.47.175 inspection/a5789621-8615-417a-9a8a-6fccddb38b4b running z5 10.30.107.69 vm-09df943c-704f-4952-b9fe-054d74d271b6 minimal true mariadb/2d876573-1248-48b0-9a00-92463cc33978 running z5 10.30.107.68 vm-76ee37ce-e99b-4a59-b0a5-f860f284b924 minimal true postgres/3226e31b-e86f-40bb-b0d8-31755b087699 running z5 10.30.107.82 vm-060c3a81-4a89-4370-9777-c06e280a83c6 minimal true

```text
###### 특정 조직에 해당 서비스 접근 허용을 할당하고 접근 서비스 목록을 다시 확인한다. (전체 조직)
>`$ uaac target`
```

inception@inception-new:~$ uaac target Target: [https://uaa.115.68.46.186.xip.io](https://uaa.115.68.46.186.xip.io) Context: admin, from client admin

```text
###### URL을 변경하고 싶을 경우 [uaac target https://uaa.115.68.46.186.xip.io]과 같이 입력하여 변경 가능합니다. (uaac target [URL])

###### 로그인을 한다.

>`$ uaac token client get`
```

계정 : admin 비번: admin-secret

inception@inception-new:~$ uaac token client get Client ID: admin Client secret: _**\*\***_

Successfully fetched token via client credentials grant. Target: [https://uaa.115.68.46.186.xip.io](https://uaa.115.68.46.186.xip.io) Context: admin, from client admin

```text
##### 파이프라인 포탈 계정 생성을 한다.

>`$ uaac client add pipeclient -s clientsecret --redirect_uri "[URL]" /
  --scope "cloud_controller_service_permissions.read , openid , cloud_controller.read , cloud_controller.write , cloud_controller.admin" /
  --authorized_grant_types "authorization_code , client_credentials , refresh_token" /
  --authorities="uaa.resource" /
  --autoapprove="openid , cloud_controller_service_permissions.read"`
```

$ uaac client add pipeclient -s clientsecret --redirect\_uri "[http://115.68.47.175:8084](http://115.68.47.175:8084) [http://115.68.47.175:8084/dashboard](http://115.68.47.175:8084/dashboard)"  --scope "cloud\_controller\_service\_permissions.read , openid , cloud\_controller.read , cloud\_controller.write , cloud\_controller.admin"  --authorized\_grant\_types "authorization\_code , client\_credentials , refresh\_token"  --authorities="uaa.resource"  --autoapprove="openid , cloud\_controller\_service\_permissions.read"

\`\`\`

