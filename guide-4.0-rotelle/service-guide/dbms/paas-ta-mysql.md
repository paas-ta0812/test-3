# Table of Contents

1. [문서 개요](paas-ta-mysql.md#1)
   * 1.1. [목적](paas-ta-mysql.md#11)
   * 1.2. [범위](paas-ta-mysql.md#12)
   * 1.3. [시스템 구성도](paas-ta-mysql.md#13)
   * 1.4. [참고자료](paas-ta-mysql.md#14)
2. [MySQL 서비스팩 설치](paas-ta-mysql.md#2)
   * 2.1. [설치전 준비사항](paas-ta-mysql.md#21)
   * 2.2. [MySQL 서비스 릴리즈 업로드](paas-ta-mysql.md#22)
   * 2.3. [MySQL 서비스 Deployment 파일 수정 및 배포](paas-ta-mysql.md#23)
   * 2.4. [MySQL 서비스 브로커 등록](paas-ta-mysql.md#24)
3. [MySQL 연동 Sample Web App 설명](paas-ta-mysql.md#3)
   * 3.1. [Sample Web App 구조](paas-ta-mysql.md#31)
   * 3.2. [PaaS-TA에서 서비스 신청](paas-ta-mysql.md#32)
   * 3.3. [Sample Web App에 서비스 바인드 신청 및 App 확인](paas-ta-mysql.md#33)
4. [MySQL Client 툴 접속](paas-ta-mysql.md#4)
   * 4.1. [HeidiSQL 설치 및 연결](paas-ta-mysql.md#41)

## 1. 문서 개요

### 1.1. 목적

본 문서\(MySQL 서비스팩 설치 가이드\)는 전자정부표준프레임워크 기반의 PaaS-TA에서 제공되는 서비스팩인 MySQL 서비스팩을 Bosh2.0을 이용하여 설치 하는 방법과 PaaS-TA의 SaaS 형태로 제공하는 Application 에서 MySQL 서비스를 사용하는 방법을 기술하였다. PaaS-TA 3.5 버전부터는 Bosh2.0 기반으로 deploy를 진행하며 기존 Bosh1.0 기반으로 설치를 원할경우에는 PaaS-TA 3.1 이하 버전의 문서를 참고한다.

### 1.2. 범위

설치 범위는 MySQL 서비스팩을 검증하기 위한 기본 설치를 기준으로 작성하였다.

### 1.3. 시스템 구성도

본 문서의 설치된 시스템 구성도이다. MySQL Server, MySQL 서비스 브로커, Proxy로 최소사항을 구성하였다.

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/mysql_vsphere_1.3.01.png)

* 설치할때 cloud config에서 사용하는 VM\_Tpye명과 스펙 

| VM\_Type | 스펙 |
| :--- | :--- |
| minimal | 1vCPU / 1GB RAM / 8GB Disk |

* 각 Instance의 Resource Pool과 스펙

| 구분 | Resource Pool | 스펙 |
| :--- | :--- | :--- |
| paasta-mysql-broker | minimal | 1vCPU / 1GB RAM / 8GB Disk |
| proxy | minimal | 1vCPU / 1GB RAM / 8GB Disk |
| mysql | minimal | 1vCPU / 1GB RAM / 8GB Disk +8GB\(영구적 Disk\) |

### 1.4. 참고자료

[**http://bosh.io/docs**](http://bosh.io/docs)  
[**http://docs.cloudfoundry.org/**](http://docs.cloudfoundry.org/)

## 2. MySQL 서비스팩 설치

### 2.1. 설치전 준비사항

본 설치 가이드는 Linux 환경에서 설치하는 것을 기준으로 하였다. 서비스팩 설치를 위해서는 먼저 BOSH CLI v2 가 설치 되어 있어야 하고 BOSH 에 로그인이 되어 있어야 한다.  
 BOSH CLI v2 가 설치 되어 있지 않을 경우 먼저 BOSH2.0 설치 가이드 문서를 참고 하여 BOSH CLI v2를 설치를 하고 사용법을 숙지 해야 한다.  


* BOSH2.0 사용자 가이드

  > BOSH2 사용자 가이드 : [https://github.com/PaaS-TA/Guide-4.0-ROTELLE/blob/master/PaaS-TA\_BOSH2\_사용자\_가이드v1.0.md](https://github.com/PaaS-TA/Guide-4.0-ROTELLE/blob/master/PaaS-TA_BOSH2_사용자_가이드v1.0.md)

> BOSH CLI V2 사용자 가이드 : [https://github.com/PaaS-TA/Guide-4.0-ROTELLE/blob/master/Use-Guide/Bosh/PaaS-TA\_BOSH\_CLI\_V2\_사용자\_가이드v1.0.md](https://github.com/PaaS-TA/Guide-4.0-ROTELLE/blob/master/Use-Guide/Bosh/PaaS-TA_BOSH_CLI_V2_사용자_가이드v1.0.md)

* PaaS-TA에서 제공하는 압축된 릴리즈 파일들을 다운받는다. \(PaaSTA-Deployment.zip, PaaSTA-Sample-Apps.zip, PaaSTA-Services.zip\)
* 다운로드 위치

  > Download : [https://paas-ta.kr/download/package](https://paas-ta.kr/download/package)

### 2.2. MySQL 서비스 릴리즈 업로드

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

* Mysql 서비스 릴리즈가 업로드 되어 있지 않은 것을 확인
* MySQL 서비스 릴리즈 파일을 업로드한다.
* **사용 예시**

  ```text
    $ bosh -e micro-bosh upload-release paasta-mysql-2.0.tgz
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

* 업로드 된 MySQL 릴리즈를 확인한다.
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
    paasta-mysql                      2.0       85e3f01e+  
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

* Mysql 서비스 릴리즈가 업로드 되어 있는 것을 확인
* Deploy시 사용할 Stemcell을 확인한다.
* **사용 예시**

  ```text
    $ bosh -e micro-bosh stemcells
    Name                                       Version  OS             CPI  CID  
    bosh-openstack-kvm-ubuntu-xenial-go_agent  315.41*  ubuntu-xenial  -    fb08e389-2350-4091-9b29-41743495e62c  
    ~                                          315.36*  ubuntu-xenial  -    7076cf5d-a473-4c46-b6c1-4a7813911f76   

    (*) Currently deployed

    3 stemcells

    Succeeded
  ```

> Stemcell 목록이 존재 하지 않을 경우 BOSH 설치 가이드 문서를 참고 하여 Stemcell을 업로드를 해야 한다.

### 2.3. MySQL 서비스 Deployment 파일 및 deploy-mysql-bosh2.0.sh 수정 및 배포

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
# paasta-mysql-service 설정 파일 내용
name: paasta-mysql-service                              # 서비스 배포이름(필수)

releases:
- name: paasta-mysql                                    # 서비스 릴리즈 이름(필수)
  version: "2.0"                                        # 서비스 릴리즈 버전(필수):latest 시 업로드된 서비스 릴리즈 최신버전

stemcells:
- alias: default
  os: ((stemcell_os))
  version: "((stemcell_version))"

update:
  canaries: 1                            # canary 인스턴스 수(필수)
  canary_watch_time: 30000-600000        # canary 인스턴스가 수행하기 위한 대기 시간(필수)
  max_in_flight: 1                       # non-canary 인스턴스가 병렬로 update 하는 최대 개수(필수)
  update_watch_time: 30000-600000        # non-canary 인스턴스가 수행하기 위한 대기 시간(필수)

instance_groups:
- name: mysql
  azs:
  - z4
  instances: 1
  vm_type: ((vm_type_small))
  stemcell: default
  persistent_disk_type: 8GB
  networks:
  - name: ((default_network_name))
    static_ips:
    - xx.x.xxx.01
  jobs:
  - name: mysql
    release: paasta-mysql  
    properties:
      admin_password: admin                # MySQL 어드민 패스워드
      cluster_ips:                         # 클러스터 구성시 IPs(필수)
      - xx.x.xxx.01
      network_name: ((default_network_name))
      seeded_databases: null
      syslog_aggregator: null
      collation_server: utf8_unicode_ci    # Mysql CharSet
      character_set_server: utf8

- name: proxy
  azs:
  - z4
  instances: 1
  vm_type: ((vm_type_small))
  stemcell: default
  networks:
  - name: ((default_network_name))
    static_ips:
    - xx.x.xxx.02
  jobs:
  - name: proxy
    release: paasta-mysql
    properties:
      cluster_ips:
      - xx.x.xxx.01
      external_host: xx.x.xxx.04.xip.io       # PaaS-TA 설치시 설정한 외부 호스트 정보(필수)
      nats:                                    # PaaS-TA 설치시 설치한 nats 정보 (필수)
        machines:
        - xx.x.xxx.05 
        password: "((nats_password))"
        port: 4222
        user: nats
      network_name: ((default_network_name))
      proxy:                                   # proxy 정보 (필수)
        api_password: admin
        api_username: api
        api_force_https: false
      syslog_aggregator: null

- name: paasta-mysql-java-broker
  azs:
  - z4
  instances: 1
  vm_type: ((vm_type_small))
  stemcell: default
  networks:
  - name: ((default_network_name))
    static_ips:
    - xx.x.xxx.03
  jobs:
  - name: op-mysql-java-broker
    release: paasta-mysql
    properties:                                        # Mysql 정보
      jdbc_ip: xx.x.xxx.02
      jdbc_pwd: admin
      jdbc_port: 3306
      log_dir: paasta-mysql-java-broker
      log_file: paasta-mysql-java-broker
      log_level: INFO

- name: broker-registrar
  lifecycle: errand                                 # bosh deploy시 vm에 생성되어 설치 되지 않고 bosh errand 로 실행할때 설정, 주로 테스트 용도에 쓰임
  azs:
  - z4
  instances: 1
  vm_type: ((vm_type_small))
  stemcell: default
  networks:
  - name: ((default_network_name))
  jobs:
  - name: broker-registrar
    release: paasta-mysql
    properties:
      broker:
        host: xx.x.xxx.03
        name: mysql-service-broker
        password: cloudfoundry
        username: admin
        protocol: http
        port: 8080
      cf:
        admin_password: admin
        admin_username: admin
        api_url: https://api.xx.x.xxx.04.xip.io
        skip_ssl_validation: true

- name: broker-deregistrar
  lifecycle: errand                                 # bosh deploy시 vm에 생성되어 설치 되지 않고 bosh errand 로 실행할때 설정, 주로 테스트 용도에 쓰임
  azs:
  - z4
  instances: 1
  vm_type: ((vm_type_small))
  stemcell: default
  networks:
  - name: ((default_network_name))
  jobs:
  - name: broker-deregistrar
    release: paasta-mysql
    properties:
      broker:
        name: mysql-service-broker
      cf:
        admin_password: admin
        admin_username: admin
        api_url: https://api.xx.x.xxx.04.xip.io
        skip_ssl_validation: true

meta:
  apps_domain: xx.x.xxx.04.xip.io
  environment: null
  external_domain: xx.x.xxx.04.xip.io
  nats:
    machines:
    - xx.x.xxx.05                                    # PaaSTA nats IP
    password: "((nats_password))"                 # PaaSTA nats password
    port: 4222
    user: nats
  syslog_aggregator: null
```

* deploy-mysql-bosh2.0.sh 파일을 서버 환경에 맞게 수정한다.

```bash
#!/bin/bash

bosh -e micro-bosh -d paasta-mysql-service deploy paasta_mysql_bosh2.0.yml \
   -v default_network_name=default \
   -v stemcell_os=ubuntu-xenial \
   -v stemcell_version="315.36" \
   -v nats_password=229yq707p0tfaq32gkhs \
   -v vm_type_small=minimal
```

* MySQL 서비스팩을 배포한다.
* **사용 예시**

  ```text
    $ ./deploy-mysql-bosh2.0.sh
    Using environment '10.30.40.111' as user 'admin' (openid, bosh.admin)

    Using deployment 'paasta-mysql-service'

    + azs:
    + - cloud_properties:
    +     datacenters:
    +     - clusters:
    +       - BD-HA:
    +           resource_pool: CF_BOSH2_Pool
    +       name: BD-HA
    +   name: z1
    + - cloud_properties:
    +     datacenters:
    +     - clusters:
    +       - BD-HA:
    +           resource_pool: CF_BOSH2_Pool
    +       name: BD-HA
    +   name: z2
    + - cloud_properties:
    +     datacenters:
    +     - clusters:
    +       - BD-HA:
    +           resource_pool: CF_BOSH2_Pool
    +       name: BD-HA
    +   name: z3
    + - cloud_properties:
    +     datacenters:
    +     - clusters:
    +       - BD-HA:
    +           resource_pool: CF_BOSH2_Pool
    +       name: BD-HA
    +   name: z4
    + - cloud_properties:
    +     datacenters:
    +     - clusters:
    +       - BD-HA:
    +           resource_pool: CF_BOSH2_Pool
    +       name: BD-HA
    +   name: z5
    + - cloud_properties:
    +     datacenters:
    +     - clusters:
    +       - BD-HA:
    +           resource_pool: CF_BOSH2_Pool
    +       name: BD-HA
    +   name: z6

    + vm_types:
    + - cloud_properties:
    +     cpu: 1
    +     disk: 8192
    +     ram: 1024
    +   name: minimal
    + - cloud_properties:
    +     cpu: 1
    +     disk: 10240
    +     ram: 2048
    +   name: default
    + - cloud_properties:
    +     cpu: 1
    +     disk: 30720
    +     ram: 4096
    +   name: small
    + - cloud_properties:
    +     cpu: 2
    +     disk: 20480
    +     ram: 4096
    +   name: medium
    + - cloud_properties:
    +     cpu: 2
    +     disk: 20480
    +     ram: 8192
    +   name: medium-memory-8GB
    + - cloud_properties:
    +     cpu: 4
    +     disk: 20480
    +     ram: 8192
    +   name: large
    + - cloud_properties:
    +     cpu: 8
    +     disk: 20480
    +     ram: 16384
    +   name: xlarge
    + - cloud_properties:
    +     cpu: 2
    +     disk: 51200
    +     ram: 4096
    +   name: small-50GB
    + - cloud_properties:
    +     cpu: 2
    +     disk: 51200
    +     ram: 4096
    +   name: small-50GB-ephemeral-disk
    + - cloud_properties:
    +     cpu: 4
    +     disk: 102400
    +     ram: 8192
    +   name: small-100GB-ephemeral-disk
    + - cloud_properties:
    +     cpu: 4
    +     disk: 102400
    +     ram: 8192
    +   name: small-highmem-100GB-ephemeral-disk
    + - cloud_properties:
    +     cpu: 8
    +     disk: 20480
    +     ram: 16384
    +   name: small-highmem-16GB
    + - cloud_properties:
    +     cpu: 1
    +     disk: 4096
    +     ram: 2048
    +   name: caas_small
    + - cloud_properties:
    +     cpu: 1
    +     disk: 4096
    +     ram: 1024
    +   name: caas_small_api
    + - cloud_properties:
    +     cpu: 1
    +     disk: 4096
    +     ram: 4096
    +   name: caas_medium
    + - cloud_properties:
    +     cpu: 2
    +     disk: 8192
    +     ram: 4096
    +   name: service_medium
    + - cloud_properties:
    +     cpu: 2
    +     disk: 10240
    +     ram: 2048
    +   name: service_medium_2G

    + vm_extensions:
    + - cloud_properties:
    +     ports:
    +     - host: 3306
    +   name: mysql-proxy-lb
    + - name: cf-router-network-properties
    + - name: cf-tcp-router-network-properties
    + - name: diego-ssh-proxy-network-properties
    + - name: cf-haproxy-network-properties
    + - cloud_properties:
    +     disk: 51200
    +   name: small-50GB
    + - cloud_properties:
    +     disk: 102400
    +   name: small-highmem-100GB

    + compilation:
    +   az: z1
    +   network: default
    +   reuse_compilation_vms: true
    +   vm_type: large
    +   workers: 5

    + networks:
    + - name: default
    +   subnets:
    +   - azs:
    +     - z1
    +     - z2
    +     - z3
    +     - z4
    +     - z5
    +     - z6
    +     cloud_properties:
    +       name: Internal
    +     dns:
    +     - 8.8.8.8
    +     gateway: 10.30.20.23
    +     range: 10.30.0.0/16
    +     reserved:
    +     - 10.30.0.0 - 10.30.111.40
    + - name: public
    +   subnets:
    +   - azs:
    +     - z1
    +     - z2
    +     - z3
    +     - z4
    +     - z5
    +     - z6
    +     cloud_properties:
    +       name: External
    +     dns:
    +     - 8.8.8.8
    +     gateway: 115.68.46.177
    +     range: 115.68.46.176/28
    +     reserved:
    +     - 115.68.46.176 - 115.68.46.188
    +     static:
    +     - 115.68.46.189 - 115.68.46.190
    +   type: manual
    + - name: service_private
    +   subnets:
    +   - azs:
    +     - z1
    +     - z2
    +     - z3
    +     - z4
    +     - z5
    +     - z6
    +     cloud_properties:
    +       name: Internal
    +     dns:
    +     - 8.8.8.8
    +     gateway: 10.30.20.23
    +     range: 10.30.0.0/16
    +     reserved:
    +     - 10.30.0.0 - 10.30.106.255
    +     static:
    +     - 10.30.107.1 - 10.30.107.255
    + - name: service_public
    +   subnets:
    +   - azs:
    +     - z1
    +     - z2
    +     - z3
    +     - z4
    +     - z5
    +     - z6
    +     cloud_properties:
    +       name: External
    +     dns:
    +     - 8.8.8.8
    +     gateway: 115.68.47.161
    +     range: 115.68.47.160/24
    +     reserved:
    +     - 115.68.47.161 - 115.68.47.174
    +     static:
    +     - 115.68.47.175 - 115.68.47.185
    +   type: manual
    + - name: portal_service_public
    +   subnets:
    +   - azs:
    +     - z1
    +     - z2
    +     - z3
    +     - z4
    +     - z5
    +     - z6
    +     cloud_properties:
    +       name: External
    +     dns:
    +     - 8.8.8.8
    +     gateway: 115.68.46.209
    +     range: 115.68.46.208/28
    +     reserved:
    +     - 115.68.46.216 - 115.68.46.222
    +     static:
    +     - 115.68.46.214
    +   type: manual

    + disk_types:
    + - disk_size: 1024
    +   name: default
    + - disk_size: 1024
    +   name: 1GB
    + - disk_size: 2048
    +   name: 2GB
    + - disk_size: 4096
    +   name: 4GB
    + - disk_size: 5120
    +   name: 5GB
    + - disk_size: 8192
    +   name: 8GB
    + - disk_size: 10240
    +   name: 10GB
    + - disk_size: 20480
    +   name: 20GB
    + - disk_size: 30720
    +   name: 30GB
    + - disk_size: 51200
    +   name: 50GB
    + - disk_size: 102400
    +   name: 100GB
    + - disk_size: 1048576
    +   name: 1TB

    + stemcells:
    + - alias: default
    +   os: ubuntu-xenial
    +   version: '315.36'

    + releases:
    + - name: paasta-mysql
    +   version: '2.0'

    + update:
    +   canaries: 1
    +   canary_watch_time: 30000-600000
    +   max_in_flight: 1
    +   update_watch_time: 30000-600000

    + instance_groups:
    + - azs:
    +   - z5
    +   instances: 3
    +   name: mysql
    +   networks:
    +   - name: service_private
    +     static_ips:
    +     - 10.30.107.166
    +     - 10.30.107.165
    +     - 10.30.107.164
    +   persistent_disk_type: 8GB
    +   properties:
    +     admin_password: "<redacted>"
    +     character_set_server: "<redacted>"
    +     cluster_ips:
    +     - "<redacted>"
    +     - "<redacted>"
    +     - "<redacted>"
    +     collation_server: "<redacted>"
    +     network_name: "<redacted>"
    +     seeded_databases: "<redacted>"
    +     syslog_aggregator: "<redacted>"
    +   release: paasta-mysql
    +   stemcell: default
    +   template: mysql
    +   vm_type: minimal
    + - azs:
    +   - z5
    +   instances: 1
    +   name: proxy
    +   networks:
    +   - name: service_private
    +     static_ips:
    +     - 10.30.107.168
    +   properties:
    +     cluster_ips:
    +     - "<redacted>"
    +     - "<redacted>"
    +     - "<redacted>"
    +     external_host: "<redacted>"
    +     nats:
    +       machines:
    +       - "<redacted>"
    +       password: "<redacted>"
    +       port: "<redacted>"
    +       user: "<redacted>"
    +     network_name: "<redacted>"
    +     proxy:
    +       api_force_https: "<redacted>"
    +       api_password: "<redacted>"
    +       api_username: "<redacted>"
    +     syslog_aggregator: "<redacted>"
    +   release: paasta-mysql
    +   stemcell: default
    +   template: proxy
    +   vm_type: minimal
    + - azs:
    +   - z5
    +   instances: 1
    +   name: paasta-mysql-java-broker
    +   networks:
    +   - name: service_private
    +     static_ips:
    +     - 10.30.107.167
    +   properties:
    +     jdbc_ip: "<redacted>"
    +     jdbc_port: "<redacted>"
    +     jdbc_pwd: "<redacted>"
    +     log_dir: "<redacted>"
    +     log_file: "<redacted>"
    +     log_level: "<redacted>"
    +   release: paasta-mysql
    +   stemcell: default
    +   template: op-mysql-java-broker
    +   vm_type: minimal
    + - azs:
    +   - z5
    +   instances: 1
    +   lifecycle: errand
    +   name: broker-registrar
    +   networks:
    +   - name: service_private
    +   properties:
    +     broker:
    +       host: "<redacted>"
    +       name: "<redacted>"
    +       password: "<redacted>"
    +       port: "<redacted>"
    +       protocol: "<redacted>"
    +       username: "<redacted>"
    +     cf:
    +       admin_password: "<redacted>"
    +       admin_username: "<redacted>"
    +       api_url: "<redacted>"
    +       skip_ssl_validation: "<redacted>"
    +   release: paasta-mysql
    +   stemcell: default
    +   template: broker-registrar
    +   vm_type: minimal
    + - azs:
    +   - z5
    +   instances: 1
    +   lifecycle: errand
    +   name: broker-deregistrar
    +   networks:
    +   - name: service_private
    +   properties:
    +     broker:
    +       name: "<redacted>"
    +     cf:
    +       admin_password: "<redacted>"
    +       admin_username: "<redacted>"
    +       api_url: "<redacted>"
    +       skip_ssl_validation: "<redacted>"
    +   release: paasta-mysql
    +   stemcell: default
    +   template: broker-deregistrar
    +   vm_type: minimal

    + meta:
    +   apps_domain: 115.68.46.189.xip.io
    +   environment: 
    +   external_domain: 115.68.46.189.xip.io
    +   nats:
    +     machines:
    +     - 10.30.112.2
    +     password: fxaqRErYZ1TD8296u9HdMg8ol8dJ0G
    +     port: 4222
    +     user: nats
    +   syslog_aggregator: 

    + name: paasta-mysql-service

    Continue? [yN]: y

    Task 4506

    Task 4506 | 06:04:10 | Preparing deployment: Preparing deployment (00:00:01)
    Task 4506 | 06:04:12 | Preparing package compilation: Finding packages to compile (00:00:00)
    Task 4506 | 06:04:12 | Compiling packages: cli/24305e50a638ece2cace4ef4803746c0c9fe4bb0
    Task 4506 | 06:04:12 | Compiling packages: openjdk-1.8.0_45/57e0ee876ea9d90f5470e3784ae1171bccee850a
    Task 4506 | 06:04:12 | Compiling packages: op-mysql-java-broker/3bf47851b2c0d3bea63a0c58452df58c14a15482
    Task 4506 | 06:04:12 | Compiling packages: syslog_aggregator/078da6dcb999c1e6f5398a6eb739182ccb4aba25
    Task 4506 | 06:04:12 | Compiling packages: common/ba480a46c4b2aa9484fb24ed01a8649453573e6f
    Task 4506 | 06:06:53 | Compiling packages: syslog_aggregator/078da6dcb999c1e6f5398a6eb739182ccb4aba25 (00:02:41)
    Task 4506 | 06:06:53 | Compiling packages: golang/f57ddbc8d55d7a0f08775bf76bb6a27dc98c7ea7
    Task 4506 | 06:06:55 | Compiling packages: common/ba480a46c4b2aa9484fb24ed01a8649453573e6f (00:02:43)
    Task 4506 | 06:06:55 | Compiling packages: python/4e255efa754d91b825476b57e111345f200944e1
    Task 4506 | 06:06:55 | Compiling packages: cli/24305e50a638ece2cace4ef4803746c0c9fe4bb0 (00:02:43)
    Task 4506 | 06:06:55 | Compiling packages: check/d6811f25e9d56428a9b942631c27c9b24f5064dc
    Task 4506 | 06:07:05 | Compiling packages: op-mysql-java-broker/3bf47851b2c0d3bea63a0c58452df58c14a15482 (00:02:53)
    Task 4506 | 06:07:05 | Compiling packages: boost/3eb8bdb1abb7eff5b63c4c5bdb41c0a778925c31
    Task 4506 | 06:07:10 | Compiling packages: openjdk-1.8.0_45/57e0ee876ea9d90f5470e3784ae1171bccee850a (00:02:58)
    Task 4506 | 06:07:53 | Compiling packages: golang/f57ddbc8d55d7a0f08775bf76bb6a27dc98c7ea7 (00:01:00)
    Task 4506 | 06:07:53 | Compiling packages: switchboard/fad565dadbb37470771801952001c7071e55a364
    Task 4506 | 06:07:53 | Compiling packages: route-registrar/f3fdfb8c940e7227a96c06e413ae6827aba8eeda
    Task 4506 | 06:07:55 | Compiling packages: check/d6811f25e9d56428a9b942631c27c9b24f5064dc (00:01:00)
    Task 4506 | 06:07:55 | Compiling packages: gra-log-purger/f02fa5774ab54dbb1b1c3702d03cb929b85d60e6
    Task 4506 | 06:08:30 | Compiling packages: route-registrar/f3fdfb8c940e7227a96c06e413ae6827aba8eeda (00:00:37)
    Task 4506 | 06:08:30 | Compiling packages: galera-healthcheck/3da4dedbcd7d9f404a19e7720e226fd472002266
    Task 4506 | 06:08:31 | Compiling packages: gra-log-purger/f02fa5774ab54dbb1b1c3702d03cb929b85d60e6 (00:00:36)
    Task 4506 | 06:08:31 | Compiling packages: mariadb_ctrl/7658290da98e2cad209456f174d3b9fa143c87fc
    Task 4506 | 06:08:32 | Compiling packages: switchboard/fad565dadbb37470771801952001c7071e55a364 (00:00:39)
    Task 4506 | 06:08:58 | Compiling packages: galera-healthcheck/3da4dedbcd7d9f404a19e7720e226fd472002266 (00:00:28)
    Task 4506 | 06:08:59 | Compiling packages: mariadb_ctrl/7658290da98e2cad209456f174d3b9fa143c87fc (00:00:28)
    Task 4506 | 06:09:42 | Compiling packages: boost/3eb8bdb1abb7eff5b63c4c5bdb41c0a778925c31 (00:02:37)
    Task 4506 | 06:11:27 | Compiling packages: python/4e255efa754d91b825476b57e111345f200944e1 (00:04:32)
    Task 4506 | 06:11:27 | Compiling packages: scons/11e7ad3b28b43a96de3df7aa41afddde582fcc38 (00:00:22)
    Task 4506 | 06:11:49 | Compiling packages: galera/d15a1d2d15e5e7417278d4aa1b908566022b9623 (00:13:18)
    Task 4506 | 06:25:07 | Compiling packages: mariadb/43aa3547bc5a01dd51f1501e6b93c215dd7255e9 (00:18:49)
    Task 4506 | 06:43:56 | Compiling packages: xtrabackup/2e701e7a9e4241b28052d984733de36aae152275 (00:10:26)
    Task 4506 | 06:55:22 | Creating missing vms: mysql/ea075ae6-6326-478b-a1ba-7fbb0b5b0bf5 (0)
    Task 4506 | 06:55:22 | Creating missing vms: mysql/e8c52bf2-cd48-45d0-9553-f6367942a634 (2)
    Task 4506 | 06:55:22 | Creating missing vms: proxy/023edddd-418e-46e4-8d40-db452c694e16 (0)
    Task 4506 | 06:55:22 | Creating missing vms: mysql/8a830154-25b6-432a-ad39-9ff09d015760 (1)
    Task 4506 | 06:55:22 | Creating missing vms: paasta-mysql-java-broker/bb5676ca-efba-48fc-bc11-f464d0ae9c78 (0)
    Task 4506 | 06:57:18 | Creating missing vms: mysql/ea075ae6-6326-478b-a1ba-7fbb0b5b0bf5 (0) (00:01:56)
    Task 4506 | 06:57:23 | Creating missing vms: proxy/023edddd-418e-46e4-8d40-db452c694e16 (0) (00:02:01)
    Task 4506 | 06:57:23 | Creating missing vms: mysql/e8c52bf2-cd48-45d0-9553-f6367942a634 (2) (00:02:01)
    Task 4506 | 06:57:23 | Creating missing vms: paasta-mysql-java-broker/bb5676ca-efba-48fc-bc11-f464d0ae9c78 (0) (00:02:01)
    Task 4506 | 06:57:23 | Creating missing vms: mysql/8a830154-25b6-432a-ad39-9ff09d015760 (1) (00:02:01)
    Task 4506 | 06:57:24 | Updating instance mysql: mysql/ea075ae6-6326-478b-a1ba-7fbb0b5b0bf5 (0) (canary) (00:02:32)
    Task 4506 | 06:59:56 | Updating instance mysql: mysql/8a830154-25b6-432a-ad39-9ff09d015760 (1) (00:03:03)
    Task 4506 | 07:02:59 | Updating instance mysql: mysql/e8c52bf2-cd48-45d0-9553-f6367942a634 (2) (00:03:04)
    Task 4506 | 07:06:03 | Updating instance proxy: proxy/023edddd-418e-46e4-8d40-db452c694e16 (0) (canary) (00:01:01)
    Task 4506 | 07:07:04 | Updating instance paasta-mysql-java-broker: paasta-mysql-java-broker/bb5676ca-efba-48fc-bc11-f464d0ae9c78 (0) (canary) (00:01:02)

    Task 4506 Started  Fri Aug 31 06:04:10 UTC 2018
    Task 4506 Finished Fri Aug 31 07:08:06 UTC 2018
    Task 4506 Duration 01:03:56
    Task 4506 done

    Succeeded
  ```

* 배포된 MySQL 서비스팩을 확인한다.
* **사용 예시**

  ```text
    $bosh -e micro-bosh -d paasta-mysql-service vms
    Using environment '10.30.40.111' as user 'admin' (openid, bosh.admin)

    Task 4525. Done

    Deployment 'paasta-mysql-service'

    Instance                                                       Process State  AZ  IPs            VM CID                                   VM Type  Active  
    mysql/8a830154-25b6-432a-ad39-9ff09d015760                     running        z5  10.30.107.165  vm-214663a8-fcbc-4ae4-9aae-92027b9725a9  minimal  true  
    mysql/e8c52bf2-cd48-45d0-9553-f6367942a634                     running        z5  10.30.107.164  vm-81ecdc43-03d2-44f5-9b89-c6cdaa443d8b  minimal  true  
    mysql/ea075ae6-6326-478b-a1ba-7fbb0b5b0bf5                     running        z5  10.30.107.166  vm-bee33ffa-3f65-456c-9250-1e74c7c97f64  minimal  true  
    paasta-mysql-java-broker/bb5676ca-efba-48fc-bc11-f464d0ae9c78  running        z5  10.30.107.167  vm-7c3edc00-3074-4e98-9c89-9e9ba83b47e4  minimal  true  
    proxy/023edddd-418e-46e4-8d40-db452c694e16                     running        z5  10.30.107.168  vm-e447eb75-1119-451f-adc9-71b0a6ef1a6a  minimal  true  

    5 vms

    Succeeded
  ```

### 2.4. MySQL 서비스 브로커 등록

Mysql 서비스팩 배포가 완료 되었으면 Application에서 서비스 팩을 사용하기 위해서 먼저 MySQL 서비스 브로커를 등록해 주어야 한다.  
서비스 브로커 등록시 PaaS-TA에서 서비스브로커를 등록할 수 있는 사용자로 로그인이 되어 있어야 한다.

#### 서비스 브로커 목록을 확인한다.

> `$ cf service-brokers`
>
> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/update_mysql_vsphere_16.png)

#### MySQL 서비스 브로커를 등록한다.

> `$ cf create-service-broker {서비스팩 이름} {서비스팩 사용자ID} {서비스팩 사용자비밀번호} http://{서비스팩 URL(IP)}`

서비스팩 이름 : 서비스 팩 관리를 위해 PaaS-TA에서 보여지는 명칭이다. 서비스 Marketplace에서는 각각의 API 서비스 명이 보여지니 여기서 명칭은 서비스팩 리스트의 명칭이다. 서비스팩 사용자ID / 비밀번호 : 서비스팩에 접근할 수 있는 사용자 ID입니다. 서비스팩도 하나의 API 서버이기 때문에 아무나 접근을 허용할 수 없어 접근이 가능한 ID/비밀번호를 입력한다. 서비스팩 URL : 서비스팩이 제공하는 API를 사용할 수 있는 URL을 입력한다.

> `$ cf create-service-broker mysql-service-broker admin cloudfoundry http://10.30.107.167:8080`
>
> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/update_mysql_vsphere_17.png)

#### 등록된 MySQL 서비스 브로커를 확인한다.

> `$ cf service-brokers`
>
> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/update_mysql_vsphere_18.png)

#### 접근 가능한 서비스 목록을 확인한다.

> `$ cf service-access`
>
> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/update_mysql_vsphere_19.png)
>
> 서비스 브로커 생성시 디폴트로 접근을 허용하지 않는다.

#### 특정 조직에 해당 서비스 접근 허용을 할당하고 접근 서비스 목록을 다시 확인한다. \(전체 조직\)

> `$ cf enable-service-access Mysql-DB`
>
> `$ cf service-access`
>
> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/update_mysql_vsphere_20.png)

## 3. MySQL 연동 Sample Web App 설명

본 Sample Web App은 PaaS-TA에 배포되며 MySQL의 서비스를 Provision과 Bind를 한 상태에서 사용이 가능하다.

### 3.1. Sample Web App 구조

Sample Web App은 PaaS-TA에 App으로 배포가 된다. App을 배포하여 구동시 Bind 된 MySQL 서비스 연결정보로 접속하여 초기 데이터를 생성하게 된다. 배포 완료 후 정상적으로 App 이 구동되면 브라우져나 curl로 해당 App에 접속 하여 MySQL 환경정보\(서비스 연결 정보\)와 초기 적재된 데이터를 보여준다.

Sample Web App 구조는 다음과 같다.

| 이름 | 설명 |
| :--- | :--- |
| src | Sample 소스 디렉토리 |
| manifest | PaaS-TA에 app 배포시 필요한 설정을 저장하는 파일 |
| pom.xml | 메이븐 project 설정 파일 |
| target | 메이븐 빌드시 생성되는 디렉토리\(war 파일, classes 폴더 등\) |

#### PaaSTA-Sample-Apps을 다운로드 받고 Service 폴더안에 있는 MySQL Sample Web App인 hello-spring-mysql를복사한다.

> `$ ls -all`
>
> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/update_mysql_vsphere_21.png)

### 3.2. PaaS-TA에서 서비스 신청

Sample Web App에서 MySQL 서비스를 사용하기 위해서는 서비스 신청\(Provision\)을 해야 한다.

\*참고: 서비스 신청시 PaaS-TA에서 서비스를 신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.

#### 먼저 PaaS-TA Marketplace에서 서비스가 있는지 확인을 한다.

> `$ cf marketplace`
>
> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/update_mysql_vsphere_22.png)

#### Marketplace에서 원하는 서비스가 있으면 서비스 신청\(Provision\)을 한다.

> `$ cf create-service {서비스명} {서비스플랜} {내서비스명}`
>
> 서비스명 : p-mysql로 Marketplace에서 보여지는 서비스 명칭이다. 서비스플랜 : 서비스에 대한 정책으로 plans에 있는 정보 중 하나를 선택한다. MySQL 서비스는 10 connection, 100 connection 를 지원한다. 내 서비스명 : 내 서비스에서 보여지는 명칭이다. 이 명칭을 기준으로 환경설정정보를 가져온다.
>
> \`$ cf create-service 'Mysql-DB' Mysql-Plan2-100con mysql-service-instance
>
> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/update_mysql_vsphere_23.png)

#### 생성된 MySQL 서비스 인스턴스를 확인한다.

> `$ cf services`
>
> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/update_mysql_vsphere_24.png)

### 3.3. Sample Web App에 서비스 바인드 신청 및 App 확인

서비스 신청이 완료되었으면 Sample Web App 에서는 생성된 서비스 인스턴스를 Bind 하여 App에서 MySQL 서비스를 이용한다. \*참고: 서비스 Bind 신청시 PaaS-TA에서 서비스 Bind신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.

#### Sample Web App 디렉토리로 이동하여 manifest 파일을 확인한다.

> `$ cd hello-spring-mysql`
>
> `$ vi manifest.yml`

```text
---
applications:
- name: hello-spring-mysql       #배포할 App 이름
  memory: 512M                # 배포시 메모리 사이즈
  instances: 1                    # 배포 인스턴스 수
path: target/hello-spring-mysql-1.0.0-BUILD-SNAPSHOT.war      #배포하는 App 파일 PATH
```

> 참고: target/hello-spring-mysql-1.0.0-BUILD-SNAPSHOT.war파일이 존재 하지 않을 경우 mvn 빌드를 수행 하면 파일이 생성된다.

#### --no-start 옵션으로 App을 배포한다.

> --no-start: App 배포시 구동은 하지 않는다.
>
> `$ cf push --no-start`
>
> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/update_mysql_vsphere_25.png)

#### 배포된 Sample App을 확인하고 로그를 수행한다.

> `$ cf apps`
>
> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/update_mysql_vsphere_26.png)
>
> `$ cf logs {배포된 App명}`
>
> `$ cf logs hello-spring-mysql`
>
> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/update_mysql_vsphere_27.png)

#### Sample Web App에서 생성한 서비스 인스턴스 바인드 신청을 한다.

> `$ cf bind-service hello-spring-mysql mysql-service-instance`
>
> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/update_mysql_vsphere_28.png)

#### 바인드가 적용되기 위해서 App을 재기동한다.

> `$ cf restart hello-spring-mysql`
>
> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/update_mysql_vsphere_29.png)
>
> \(참고\) 바인드 후 App구동시 Mysql 서비스 접속 에러로 App 구동이 안될 경우 보안 그룹을 추가한다.

#### rule.json 화일을 만들고 아래와 같이 내용을 넣는다.

> `$ vi rule.json`

```javascript
[
  {
    "protocol": "tcp",
    "destination": "10.0.0.63",
    "ports": "3306"
  }
]
```

#### 보안 그룹을 생성한다.

> `$ cf create-security-group p-mysql rule.json`
>
> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/update_mysql_vsphere_30.png)

#### 모든 App에 Mysql 서비스를 사용할수 있도록 생성한 보안 그룹을 적용한다.

> `$ cf bind-running-security-group p-mysql`
>
> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/update_mysql_vsphere_31.png)

#### App을 리부팅 한다.

> `$ cf restart hello-spring-mysql`
>
> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/update_mysql_vsphere_32.png)

#### App이 정상적으로 MySQL 서비스를 사용하는지 확인한다.

> curl 로 확인
>
> `$ curl hello-spring-mysql.52.71.64.39.xip.io`
>
> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/update_mysql_vsphere_33.png)
>
> 브라우져에서 확인 ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/update_mysql_vsphere_34.png)

## 4. MySQL Client 툴 접속

Application에 바인딩 된 MySQL 서비스 연결정보는 Private IP로 구성되어 있기 때문에 MySQL Client 툴에서 직접 연결할수 없다. 따라서 MySQL Client 툴에서 SSH 터널, Proxy 터널 등을 제공하는 툴을 사용해서 연결하여야 한다. 본 가이드는 SSH 터널을 이용하여 연결 하는 방법을 제공하며 MySQL Client 툴로써는 오픈 소스인 HeidiSQL로 가이드한다. HeidiSQL 에서 접속하기 위해서 먼저 SSH 터널링 할수 있는 VM 인스턴스를 생성해야한다. 이 인스턴스는 SSH로 접속이 가능해야 하고 접속 후 Open PaaS 에 설치한 서비스팩에 Private IP 와 해당 포트로 접근이 가능하도록 시큐리티 그룹을 구성해야 한다. 이 부분은 vSphere관리자 및 OpenPaaS 운영자에게 문의하여 구성한다.

### 4.1. HeidiSQL 설치 및 연결

HeidiSQL 프로그램은 무료로 사용할 수 있는 오픈소스 소프트웨어이다.

#### HeidiSQL을 다운로드 하기 위해 아래 URL로 이동하여 설치파일을 다운로드 한다.

> [http://www.heidisql.com/download.php](http://www.heidisql.com/download.php)
>
> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/mysql_vsphere_4.1.01.png)

#### 다운로드한 설치파일을 실행한다.

> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/mysql_vsphere_4.1.02.png)

#### HeidSQL 설치를 위한 안내사항이다. Next 버튼을 클릭한다.

> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/mysql_vsphere_4.1.03.png)

#### 프로그램 라이선스에 관련된 내용이다. 동의\(I accept the agreement\)에 체크 후 Next 버튼을 클릭한다.

> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/mysql_vsphere_4.1.04.png)

#### HeidiSQL을 설치할 경로를 설정 후 Next 버튼을 클릭한다.

> 별도의 경로 설정이 필요 없을 경우 default로 C드라이브 Program Files 폴더에 설치가 된다.
>
> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/mysql_vsphere_4.1.05.png)

#### 설치 완료 후 시작메뉴에 HeidiSQL 바로가기 아이콘의 이름을 설정하는 과정이다.

> Next 버튼을 클릭하여 다음 과정을 진행한다.
>
> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/mysql_vsphere_4.1.06.png)

#### 체크박스가 4개가 있다. 아래의 경우를 고려하여 체크 및 해제를 한다.

> 바탕화면에 바로가기 아이콘을 생성할 경우  
> sql확장자를 HeidiSQL 프로그램으로 실행할 경우  
> heidisql 공식 홈페이지를 통해 자동으로 update check를 할 경우  
> heidisql 공식 홈페이지로 자동으로 버전을 전송할 경우
>
> 체크박스에 체크 설정/해제를 완료했다면 Next 버튼을 클릭한다.
>
> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/mysql_vsphere_4.1.07.png)

#### 설치를 위한 모든 설정이 한번에 출력된다. 확인 후 Install 버튼을 클릭하여 설치를 진행한다.

> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/mysql_vsphere_4.1.08.png)

#### Finish 버튼 클릭으로 설치를 완료한다.

> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/mysql_vsphere_4.1.09.png)

#### HeidiSQL을 실행했을 때 처음 뜨는 화면이다. 이 화면에서 Server에 접속하기 위한 profile을 설정/저장하여 접속할 수 있다. 신규 버튼을 클릭한다.

> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/mysql_vsphere_4.1.10.png)

#### 어떤 Server에 접속하기 위한 Connection 정보인지 별칭을 입력한다.

> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/mysql_vsphere_4.1.11.png)

#### 네트워크 유형의 목록에서 MySQL\(SSH tunel\)을 선택한다.

> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/mysql_vsphere_4.1.12.png)

#### 아래 붉은색 영역에 접속하려는 서버 정보를 모두 입력한다.

> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/mysql_vsphere_4.1.13.png)
>
> 서버 정보는 Application에 바인드되어 있는 서버 정보를 입력한다. cf env  명령어로 이용하여 확인한다.
>
> **예\)** $cf env hello-spring-mysql
>
> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/mysql_vsphere_4.1.14.png)

#### - SSH 터널 탭을 클릭하고 OpenPaaS 운영 관리자에게 제공받은 SSH 터널링 가능한 서버 정보를 입력한다. plink.exe 위치 입력은 Putty에서 제공하는 plink.exe 실행 위치를 넣어주고 만일 해당 파일이 없을 경우 plink.exe 내려받기 링크를 클릭하여 다운받는다. 로컬 포트 정보는 임의로 넣고 열기 버튼을 클릭하면 Mysql 데이터베이스에 접속한다.

> \(참고\) 만일 개인 키로 접속이 가능한 경우에는 openstack용 Open PaaS Mysql 서비스팩 설치 가이드를 참고한다.
>
> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/mysql_vsphere_4.1.15.png)

#### 접속이 완료되면 좌측에 스키마 정보가 나타난다. 하지만 초기설정은 테이블, 뷰, 프로시져, 함수, 트리거, 이벤트 등 모두 섞여 있어서 한눈에 구분하기가 힘들어서 접속한 DB 별칭에 마우스 오른쪽 클릭 후 "트리 방식 옵션" - "객체를 유형별로 묶기"를 클릭하면 아래 화면과 같이 각 유형별로 구분이된다.

> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/mysql_vsphere_4.1.16.png)

#### 우측 화면에 쿼리 탭을 클릭하여 Query문을 작성한 후 실행 버튼\(삼각형\)을 클릭한다.

> 쿼리문에 이상이 없다면 정상적으로 결과를 얻을 수 있을 것이다.
>
> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/mysql/mysql_vsphere_4.1.17.png)

