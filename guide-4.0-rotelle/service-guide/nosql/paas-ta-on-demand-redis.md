# Table of Contents

1. [문서 개요](paas-ta-on-demand-redis.md#1)
   * [1.1. 목적](paas-ta-on-demand-redis.md#11)
   * [1.2. 범위](paas-ta-on-demand-redis.md#12)
   * [1.3. 시스템 구성도](paas-ta-on-demand-redis.md#13)
   * [1.4. 참고자료](paas-ta-on-demand-redis.md#14)
2. [On-Demand-Redis 서비스팩 설치](paas-ta-on-demand-redis.md#2)
   * [2.1. 설치 전 준비 사항](paas-ta-on-demand-redis.md#21)
   * [2.2. On-Demand-Redis 서비스 릴리즈 업로드](paas-ta-on-demand-redis.md#22)
   * [2.3. On-Demand-Redis 서비스 Deployment 파일 수정 및 배포](paas-ta-on-demand-redis.md#23)
3. [CF CLI를 이용한 On-Demand-Redis 서비스 브로커 등록](paas-ta-on-demand-redis.md#3)
   * [3.1. PaaS-TA에서 서비스 신청](paas-ta-on-demand-redis.md#31)
   * [3.2. Sample App에 서비스 바인드 신청 및 App 확인](paas-ta-on-demand-redis.md#32)
4. [Portal을 이용한 redis service test](paas-ta-on-demand-redis.md#4)
   * [4.1. 서비스 신청](paas-ta-on-demand-redis.md#41)
   * [4.2. Sample App에 서비스 바인드 신청 및 App 확인](paas-ta-on-demand-redis.md#42)
5. [Redis Client 툴 접속](paas-ta-on-demand-redis.md#5)
   * [5.1. Redis Desktop Manager 설치 및 연결](paas-ta-on-demand-redis.md#51)

## 1. 문서 개요

### 1.1. 목적

본 문서\(Redis 서비스팩 설치 가이드\)는 전자정부표준프레임워크 기반의 PaaS-TA에서 제공되는 서비스팩인 Redis 서비스팩을 Bosh를 이용하여 설치하는 방법과 PaaS-TA의 SaaS 형태로 제공하는 Application에서 Redis 서비스를 사용하는 방법을 기술하였다.

### 1.2. 범위

설치 범위는 Redis서비스팩을 검증하기 위한 기본 설치를 기준으로 작성하였다.

### 1.3. 시스템 구성도

본 문서의 설치된 시스템 구성도입니다. Redis, MariaDB, On-Demand 서비스 브로커로 최소사항을 구성하였다. ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_image_01.png)

| 구분 | 스펙 |
| :--- | :--- |
| on-demand-service-broker | 1vCPU / 1GB RAM / 4GB Disk |
| Mariadb | 1vCPU / 256MB RAM / 4GB Disk+2GB\(영구적 Disk\) |
| Redis | 1vCPU / 256MB RAM / 4GB Disk+1GB\(영구적 Disk\) |

### 1.4. 참고자료

[**http://bosh.io/docs**](http://bosh.io/docs)   
 [**http://docs.cloudfoundry.org/**](http://docs.cloudfoundry.org/)

## 2. On-Demand Redis 서비스팩 설치

### 2.1. **설치 전 준비 사항**

본 설치 가이드는 Linux 환경에서 설치하는 것을 기준으로 하였다. 서비스팩 설치를 위해서는 먼저 BOSH CLI v2 가 설치 되어 있어야 하고 BOSH 에 로그인이 되어 있어야 한다.  
 BOSH CLI v2 가 설치 되어 있지 않을 경우 먼저 BOSH2.0 설치 가이드 문서를 참고 하여 BOSH CLI v2를 설치를 하고 사용법을 숙지 해야 한다.  


* BOSH 2.0 사용자 가이드

  > BOSH 2 사용자 가이드 : [https://github.com/PaaS-TA/Guide-4.0-ROTELLE/blob/master/PaaS-TA\_BOSH2\_사용자\_가이드v1.0.md](https://github.com/PaaS-TA/Guide-4.0-ROTELLE/blob/master/PaaS-TA_BOSH2_사용자_가이드v1.0.md)
  >
  > BOSH CLI V2 사용자 가이드 : [https://github.com/PaaS-TA/Guide-4.0-ROTELLE/blob/master/Use-Guide/Bosh/PaaS-TA\_BOSH\_CLI\_V2\_사용자\_가이드v1.0.md](https://github.com/PaaS-TA/Guide-4.0-ROTELLE/blob/master/Use-Guide/Bosh/PaaS-TA_BOSH_CLI_V2_사용자_가이드v1.0.md)

### 2.2. On-Demand Redis 서비스 릴리즈 업로드

* 업로드 되어 있는 릴리즈 목록을 확인한다.
* **사용 예시**

  ```text
    $ bosh -e micro-bosh releases
        Using environment '10.30.40.111' as user 'admin' (openid, bosh.admin)

    Name                                       Version   Commit Hash  
    PAAS-TA-APP-LIFECYCLE-SERVICE-RELEASE      0.1       b3bd6e6+  
    binary-buildpack                           1.0.28    55c2ed3  
    ~                                          1.0.27*   58b60f9  
    bosh-dns                                   1.10.0*   7c6515f  
    ~                                          1.5.0*    f5a8d25  
    bosh-dns-aliases                           0.0.3*    eca9c5a  
    bpm                                        1.0.3     d2f7197  
    ~                                          1.0.0*    4871b58  
    ~                                          0.13.0*   91e202a  
    ~                                          0.6.0*    b6f4675  
    capi                                       1.74.0    8ee6a87  
    ~                                          1.71.0*   2921352  
    cf-cli                                     1.11.0    c10235d  
    ~                                          1.9.0*    1df1cf8  
    cf-networking                              2.20.0    87e77b2b  
    ~                                          2.17.0*   66858e0a  
    cf-smoke-tests                             40.0.44   2c0914f  
    ~                                          40.0.13*  16f504d  
    cf-syslog-drain                            8.1       6e8acdd  
    ~                                          8.0*      67f9b41  
    cfcr-etcd                                  1.3*      6a62d8f  
    cflinuxfs2                                 1.257.0   57aa652  
    ~                                          1.244.0*  35acdfc  
    cflinuxfs3                                 0.48.0    cff2599  
    credhub                                    2.1.2     124eecf+  
    ~                                          2.1.1*    616b142+  
    diego                                      2.25.0    6442f8a5+  
    ~                                          2.20.1*   non-git  
    docker                                     32.0.0*   542c382  
    dotnet-core-buildpack                      2.2.3     2b25882  
    ~                                          2.1.5*    27a1329  
    garden-runc                                1.17.2    fbf261d+  
    ~                                          1.16.8*   31b588b+  
    go-buildpack                               1.8.30    f78d9f0  
    ~                                          1.8.28*   a25966c  
    haproxy                                    9.3.0*    66eda42  
    influxdb                                   1.5.1*    non-git  
    java-buildpack                             4.16.1*   c350503  
    kubo                                       0.19.0*   c9294bb  
    log-cache                                  2.0.2     6367c8f  
    ~                                          2.0.1*    d78304e  
    loggregator                                104.4     10160fda  
    ~                                          104.0*    ea54700e  
    loggregator-agent                          3.2       4c622e2  
    ~                                          2.3*      1c69c16  
    logsearch                                  209.0.2*  b2a9865  
    logsearch-for-cloudfoundry                 210.0.0*  83b2eff+  
    market-app-release                         8.0*      3b78f31+  
    ~                                          7.0       3b78f31+  
    ~                                          5.0       b9e32f5  
    ~                                          4.0       7747efe+  
    ~                                          3.0       9ca72ed+  
    ~                                          2.0       563e2ff  
    ~                                          1.1       7747efe  
    ~                                          1.0       ea7fa8d  
    nats                                       26*       f407411  
    nodejs-buildpack                           1.6.38    f7c4b52  
    ~                                          1.6.32*   fe11b60  
    paas-ta-web-ide-broker-release             2.0*      non-git  
    paasta-container-service-projects-release  1.0*      02a858d+  
    paasta-delivery-pipeline-release           1.0*      non-git  
    paasta-monitoring                          4.0*      non-git  
    paasta-monitoring-agent                    4.0       non-git  
    paasta-mysql                               2.0*      85e3f01e+  
    paasta-portal-api-release                  1.0*      3c2fa86+  
    paasta-sourcecontrol-release               1.0*      d41bda4d+  
    php-buildpack                              4.3.67    acf3565  
    ~                                          4.3.61*   030de6f  
    postgres                                   33        0c379264+  
    ~                                          31*       eb34d265+  
    ~                                          30*       0cd50c00+  
    private-image-repository-release           1.0*      777b440+  
    python-buildpack                           1.6.25    667dbea  
    ~                                          1.6.21*   7159ca6  
    redis                                      14.0.1*   96f111b  
    routing                                    0.184.0   f7acbb5  
    ~                                          0.182.0*  8fc5ec0  
    ruby-buildpack                             1.7.29    25d0a91  
    ~                                          1.7.24*   2e621b3  
    silk                                       2.20.0    145ad84  
    ~                                          2.17.0*   8d5b1ea  
    staticfile-buildpack                       1.4.37    3839bd7  
    ~                                          1.4.32*   9c2d8b4  
    statsd-injector                            1.5.0*    533c630  
    syslog                                     11.4.0    feedfa7  
    taiga-release                              0.5*      3b78f31+  
    ~                                          0.4       3b78f31+  
    ~                                          0.3       3b78f31+  
    ~                                          0.2       3b78f31+  
    ~                                          0.1       3b78f31+  
    uaa                                        68.0      2549b3b  
    ~                                          64.0*     9dfeac2  

    (*) Currently deployed
    (+) Uncommitted changes

    92 releases

    Succeeded
  ```

* On-Demand Redis 서비스 릴리즈가 업로드 되어 있지 않은 것을 확인
* Redis 서비스 릴리즈 파일을 업로드한다.
* **사용 예시**

  ```text
    $ bosh -e micro-bosh upload-release on-demand-release.tgz
        Using environment '10.30.40.111' as user 'admin' (openid, bosh.admin)

    ######################################################## 100.00% 110.24 MiB/s 0s
    Task 936150

    Task 936150 | 07:14:34 | Extracting release: Extracting release (00:00:01)
    Task 936150 | 07:14:35 | Verifying manifest: Verifying manifest (00:00:00)
    Task 936150 | 07:14:35 | Resolving package dependencies: Resolving package dependencies (00:00:00)
    Task 936150 | 07:14:35 | Creating new packages: java/86d8b8f8115418addf836753c1735abe547d4105 (00:00:05)
    Task 936150 | 07:14:40 | Creating new packages: mariadb/59a218308c6c7dcf8795b531b53aa4a1c666ce00 (00:00:35)
    Task 936150 | 07:15:15 | Creating new packages: paas-ta-on-demand-broker/060339465a66f4a7e030c3c7cd13e85f46667ccc (00:00:02)
    Task 936150 | 07:15:18 | Creating new packages: redis-4/a3c434216eff51abbb62eadf23487cee73ba4b3e (00:00:00)
    Task 936150 | 07:15:18 | Creating new jobs: mariadb/ad604af5c5e5491e616b2dfcf9de2da34750daf5 (00:00:00)
    Task 936150 | 07:15:18 | Creating new jobs: paas-ta-on-demand-broker/fead34137d8f00e8ce8e58919d21f6fe1a513fcc (00:00:00)
    Task 936150 | 07:15:18 | Creating new jobs: redis/e4a06f84c345a755d6dcbc32e0031eca11cd3aec (00:00:00)
    Task 936150 | 07:15:18 | Creating new jobs: sanity-tests/866d68289c2fd6066c97be56011825bba2cc58d5 (00:00:00)
    Task 936150 | 07:15:18 | Release has been created: on-demand-release/1.0 (00:00:00)

    Task 936150 Started  Tue Jul  2 07:14:34 UTC 2019
    Task 936150 Finished Tue Jul  2 07:15:18 UTC 2019
    Task 936150 Duration 00:00:44
    Task 936150 done

    Succeeded
  ```

* 업로드 된 Redis 릴리즈를 확인한다.
* **사용 예시**

  ```text
    $ bosh -e micro-bosh releases
        Using environment '10.30.40.111' as user 'admin' (openid, bosh.admin)

    Name                                       Version   Commit Hash  
    PAAS-TA-APP-LIFECYCLE-SERVICE-RELEASE      0.1       b3bd6e6+  
    binary-buildpack                           1.0.28    55c2ed3  
    ~                                          1.0.27*   58b60f9  
    bosh-dns                                   1.10.0*   7c6515f  
    ~                                          1.5.0*    f5a8d25  
    bosh-dns-aliases                           0.0.3*    eca9c5a  
    bpm                                        1.0.3     d2f7197  
    ~                                          1.0.0*    4871b58  
    ~                                          0.13.0*   91e202a  
    ~                                          0.6.0*    b6f4675  
    capi                                       1.74.0    8ee6a87  
    ~                                          1.71.0*   2921352  
    cf-cli                                     1.11.0    c10235d  
    ~                                          1.9.0*    1df1cf8  
    cf-networking                              2.20.0    87e77b2b  
    ~                                          2.17.0*   66858e0a  
    cf-smoke-tests                             40.0.44   2c0914f  
    ~                                          40.0.13*  16f504d  
    cf-syslog-drain                            8.1       6e8acdd  
    ~                                          8.0*      67f9b41  
    cfcr-etcd                                  1.3*      6a62d8f  
    cflinuxfs2                                 1.257.0   57aa652  
    ~                                          1.244.0*  35acdfc  
    cflinuxfs3                                 0.48.0    cff2599  
    credhub                                    2.1.2     124eecf+  
    ~                                          2.1.1*    616b142+  
    diego                                      2.25.0    6442f8a5+  
    ~                                          2.20.1*   non-git  
    docker                                     32.0.0*   542c382  
    dotnet-core-buildpack                      2.2.3     2b25882  
    ~                                          2.1.5*    27a1329  
    garden-runc                                1.17.2    fbf261d+  
    ~                                          1.16.8*   31b588b+  
    go-buildpack                               1.8.30    f78d9f0  
    ~                                          1.8.28*   a25966c  
    haproxy                                    9.3.0*    66eda42  
    influxdb                                   1.5.1*    non-git  
    java-buildpack                             4.16.1*   c350503  
    kubo                                       0.19.0*   c9294bb  
    log-cache                                  2.0.2     6367c8f  
    ~                                          2.0.1*    d78304e  
    loggregator                                104.4     10160fda  
    ~                                          104.0*    ea54700e  
    loggregator-agent                          3.2       4c622e2  
    ~                                          2.3*      1c69c16  
    logsearch                                  209.0.2*  b2a9865  
    logsearch-for-cloudfoundry                 210.0.0*  83b2eff+  
    market-app-release                         8.0*      3b78f31+  
    ~                                          7.0       3b78f31+  
    ~                                          5.0       b9e32f5  
    ~                                          4.0       7747efe+  
    ~                                          3.0       9ca72ed+  
    ~                                          2.0       563e2ff  
    ~                                          1.1       7747efe  
    ~                                          1.0       ea7fa8d  
    nats                                       26*       f407411  
    nodejs-buildpack                           1.6.38    f7c4b52  
    ~                                          1.6.32*   fe11b60  
    on-demand-redis-release                    1.0       empty+  
    paas-ta-web-ide-broker-release             2.0*      non-git  
    paasta-container-service-projects-release  1.0*      02a858d+  
    paasta-delivery-pipeline-release           1.0*      non-git  
    paasta-monitoring                          4.0*      non-git  
    paasta-monitoring-agent                    4.0       non-git  
    paasta-mysql                               2.0*      85e3f01e+  
    paasta-portal-api-release                  1.0*      3c2fa86+  
    paasta-sourcecontrol-release               1.0*      d41bda4d+  
    php-buildpack                              4.3.67    acf3565  
    ~                                          4.3.61*   030de6f  
    postgres                                   33        0c379264+  
    ~                                          31*       eb34d265+  
    ~                                          30*       0cd50c00+  
    private-image-repository-release           1.0*      777b440+  
    python-buildpack                           1.6.25    667dbea  
    ~                                          1.6.21*   7159ca6  
    redis                                      14.0.1*   96f111b  
    routing                                    0.184.0   f7acbb5  
    ~                                          0.182.0*  8fc5ec0  
    ruby-buildpack                             1.7.29    25d0a91  
    ~                                          1.7.24*   2e621b3  
    silk                                       2.20.0    145ad84  
    ~                                          2.17.0*   8d5b1ea  
    staticfile-buildpack                       1.4.37    3839bd7  
    ~                                          1.4.32*   9c2d8b4  
    statsd-injector                            1.5.0*    533c630  
    syslog                                     11.4.0    feedfa7  
    taiga-release                              0.5*      3b78f31+  
    ~                                          0.4       3b78f31+  
    ~                                          0.3       3b78f31+  
    ~                                          0.2       3b78f31+  
    ~                                          0.1       3b78f31+  
    uaa                                        68.0      2549b3b  
    ~                                          64.0*     9dfeac2  

    (*) Currently deployed
    (+) Uncommitted changes

    93 releases

    Succeeded
  ```

* On-Demand-Redis 서비스 릴리즈가 업로드 되어 있는 것을 확인
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

### 2.3. On-Demand Redis 서비스 Deployment 파일 수정 및 배포

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
# paasta_on_demand_service_broker.yml 설정 파일 내용

---
name: "((deployment_name))"        #서비스 배포이름(필수) bosh deployments 로 확인 가능한 이름
# director_uuid: <%= `bosh status --uuid` %>                  # Director UUID을 입력(필수) bosh status 명령으로 확인 가능

stemcells:
- alias: "((stemcell_alias))"
  os: "((stemcell_os))"
  version: "((stemcell_version))"

addons:
- name: bpm
  jobs:
  - name: bpm
    release: bpm

variables:
- name: redis-password
  type: password

releases:
- name: bpm
  sha1: f2bd126b17b3591160f501d88d79ccf0aba1ae54
  url: git+https://github.com/cloudfoundry-incubator/bpm-release  # 외부통신이 불가능할 경우 bpm 1.0.3 버전을 미리 릴리즈 해야 한다.  
  version: 1.0.3
- name: "((releases_name))"                  # 서비스 릴리즈 이름(필수) bosh releases로 확인 가능
  version: "1.0"                                             # 서비스 릴리즈 버전(필수):latest 시 업로드된 서비스 릴리즈 최신버전

update:
  canaries: 1                                               # canary 인스턴스 수(필수)
  canary_watch_time: 5000-120000                            # canary 인스턴스가 수행하기 위한 대기 시간(필수)
  update_watch_time: 5000-120000                            # non-canary 인스턴스가 수행하기 위한 대기 시간(필수)
  max_in_flight: 1                                          # non-canary 인스턴스가 병렬로 update 하는 최대 개수(필수)
  serial: false

instance_groups:
########## INFRA ##########
- name: mariadb
  azs:
  - z3
  instances: 1
  vm_type: service_tiny 
  stemcell: "((stemcell_alias))"
  persistent_disk_type: "((mariadb_disk_type))"
  networks:
  - name: "((internal_networks_name))"
  templates:
  - name: mariadb
    release: "((releases_name))"
  syslog_aggregator: null

######## BROKER ########

- name: paas-ta-on-demand-broker
  azs:
  - z3
  instances: 1
  vm_type: service_medium
  stemcell: "((stemcell_alias))"
  networks:
  - name: "((internal_networks_name))"
  templates:
  - name: paas-ta-on-demand-broker
    release: "((releases_name))"
  syslog_aggregator: null
- name: redis
  azs: 
  - z3
  instances: 1 
  vm_type: service_tiny
  stemcell: "((stemcell_alias))"
  persistent_disk: 1024
  networks:
  - name: "((internal_networks_name))"
  jobs:
  - name: redis
    release: "((releases_name))"
- name: sanity-tests
  azs:
  - z3
  instances: 1
  lifecycle: errand
  vm_type: service_tiny
  stemcell: "((stemcell_alias))"
  networks: 
  - name: "((internal_networks_name))"
  jobs:
  - name: sanity-tests
    release: "((releases_name))"

######### COMMON PROPERTIES ##########
properties:
  broker:
    server:
      port: "((broker_port))"
    datasource:
      password: "((mariadb_user_password))"
    service_instance:
      guid: "((service_instance_guid))"
      name: "((service_instance_name))"
      bullet:
        name: "((service_instance_bullet_name))"
        desc: "((service_instance_bullet_desc))"
      plan: 
        id: "((service_instance_plan_guid))"
        name: "((service_instance_plan_name))"
        desc: "((service_instance_plan_desc))"
      org_limitation: "((service_instance_org_limitation))"
      space_limitation: "((service_instance_space_limitation))"
    bosh:
      client_id: "((bosh_client_admin_id))"
      client_secret: "((bosh_client_admin_secret))"
      url: ((bosh_url)):((bosh_director_port))
      oauth_url: ((bosh_url)):((bosh_oauth_port))
      deployment_name: "((deployment_name))"
      instance_name: "((on_demand_service_instance_name))"
    cloudfoundry:
      url: "((cloudfoundry_url))"
      sslSkipValidation: "((cloudfoundry_sslSkipValidation))"
      admin:
        id: "((cloudfoundry_admin_id))"
        password: "((cloudfoundry_admin_password))"
  mariadb:                                                # MARIA DB SERVER 설정 정보
    port: "((mariadb_port))"                                            # MARIA DB PORT 번호
    admin_user:
      password: "((mariadb_user_password))"                             # MARIA DB ROOT 계정 비밀번호
    host_names:
    - mariadb0
######### SERVICE PROPERTIES #################
  redis:
    password: "((redis_password))"
```

* necessary\_on\_demand\_vars.yml\(인프라 환경\), unnecessary\_on\_demand\_vars.yml\(브로커환경\) 수정한다.
* deploy-vsphere.sh를 확인후 배포한다.

  ```bash
  #!/bin/bash
  bosh -d on-demand-service-broker deploy paasta_on_demand_service_broker.yml \
   -l necessary_on_demand_vars.yml\
   -l unnecessary_on_demand_vars.yml
  ```

```text
necessary_on_demand_vars.yml

#!/bin/bash

---
### On-Demand Bosh Deployment Name Setting ###
deployment_name: on-demand-service-broker                       #On-Demand Deployment Name을 지정한다.

### Main Stemcells Setting ###
stemcell_os: ubuntu-xenial                                      # Deployment Main Stemcell OS
stemcell_version: 315.68                                       # Main Stemcell Version
stemcell_alias: default                                         # Main Stemcell Alias

### On-Demand Release Deployment Setting ### 
releases_name : on-demand-redis-release                         # On-Demand Release Name
internal_networks_name : service_private                        # Some Network From Your 'bosh cloud-config(cc)'
mariadb_disk_type : 2GB                                         # MariaDB Disk Type 'bosh cloud-config(cc)' 2G 이하로 할당할 경우 에러 발생
broker_port : 8080                                              # On-Demand Broker Server Port 서비스 브로커 등록할때 접근할 포트를 지정한다.
bosh_client_admin_id: admin                                     # Bosh Client Admin ID 를 입력한다.
bosh_client_admin_secret:                                       # Bosh Client Admin Secret을 입력한다.
bosh_url: https://xxx.xxx.xxx.xxx                               # Bosh URL을 입력한다. 'bosh env' 명령어를 치면 알 수 있다.
bosh_director_port: 25555                                       # Bosh API Port
bosh_oauth_port: 8443                                           # Bosh Oauth Port

cloudfoundry_url: xxx.xxx.xxx.xxx.xip.io                        # CloudFoundry URL
cloudfoundry_sslSkipValidation: true                            # CloudFoundry Login SSL Validation
cloudfoundry_admin_id: admin                                    # CloudFoundry Admin ID
cloudfoundry_admin_password:                                    # CloudFoundry Admin Password

### On-Demand Service Property(Changes are possible) ###
mariadb_port: 3306                                              # MariaDB Server Port
mariadb_user_password:                                          # MariaDB Root Password(임의로 지정한다.)

### On-Demand Dedicated Service Instance Properties ###

on_demand_service_instance_name: redis                          # On-Demand Service Instance Name
service_password: admin_test                                    # Dedicated Service Password
service_port: 6379                                              # Dedicated Service Port
```

현재 On-Demand Service는 Plan 1개만 입력받도록 되어있다.

```text
service_instance_guid: 54e2de61-de84-4b9c-afc3-88d08aadfcb6                # Service Instance Guid 이볅한다.
service_instance_name: redis                                               # Service Instance Name 입력한다.
service_instance_bullet_name: Redis Dedicated Server Use                   # Service Instance bullet Name을 입력한다.
service_instance_bullet_desc: Redis Service Using a Dedicated Server       # Service Instance bullet에 대한 설명을 입력한다.
service_instance_plan_guid: 2a26b717-b8b5-489c-8ef1-02bcdc445720           # Service Instance Plan Guid를 입력한다.
service_instance_plan_name: dedicated-vm                                   # Service Instance Plan Name을 입력한다.
service_instance_plan_desc: Redis service to provide a key-value store     # Service Instance Plan에 대한 설명을 입력한다.
service_instance_org_limitation: -1                                        # Org에 설치할수 있는 Service Instance 개수를 제한한다. (-1일경우 제한없음)
service_instance_space_limitation: -1                                      # Space에 설치할수 있는 Service Instance 개수를 제한한다. (-1일경우 제한없음)
```

* On-Demand-Redis 서비스팩을 배포한다.
* **사용 예시**

  ```text
    $ sh deploy-vsphere.sh
    Using environment '10.30.40.111' as client 'admin'

    Using deployment 'on-demand-service-broker'

    Release 'bpm/1.0.3' already exists.

    + azs:
    + - cloud_properties:
    +     datacenters:
    +     - clusters:
    +       - BD-HA:
    +           resource_pool: PAASTA_40_BOSH2_Pools
    +       name: BD-HA
    +   name: z1
    + - cloud_properties:
    +     datacenters:
    +     - clusters:
    +       - BD-HA:
    +           resource_pool: PAASTA_40_BOSH2_Pools
    +       name: BD-HA
    +   name: z2
    + - cloud_properties:
    +     datacenters:
    +     - clusters:
    +       - BD-HA:
    +           resource_pool: PAASTA_40_BOSH2_Pools
    +       name: BD-HA
    +   name: z3
    + - cloud_properties:
    +     datacenters:
    +     - clusters:
    +       - BD-HA:
    +           resource_pool: PAASTA_40_BOSH2_Pools
    +       name: BD-HA
    +   name: z4
    + - cloud_properties:
    +     datacenters:
    +     - clusters:
    +       - BD-HA:
    +           resource_pool: PAASTA_40_BOSH2_Pools
    +       name: BD-HA
    +   name: z5
    + - cloud_properties:
    +     datacenters:
    +     - clusters:
    +       - BD-HA:
    +           resource_pool: PAASTA_40_BOSH2_Pools
    +       name: BD-HA
    +   name: z6
    + - cloud_properties:
    +     datacenters:
    +     - clusters:
    +       - BD-HA:
    +           resource_pool: PAASTA_40_BOSH2_Pools
    +       name: BD-HA
    +   name: z7

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
    +     cpu: 1
    +     disk: 4096
    +     ram: 256
    +   name: service_tiny
    + - cloud_properties:
    +     cpu: 1
    +     disk: 4096
    +     ram: 512
    +   name: service_small
    + - cloud_properties:
    +     cpu: 1
    +     disk: 4096
    +     ram: 1024
    +   name: service_medium
    + - cloud_properties:
    +     cpu: 1
    +     disk: 4096
    +     ram: 2048
    +   name: service_medium_1CPU_2G
    + - cloud_properties:
    +     cpu: 2
    +     disk: 8192
    +     ram: 4096
    +   name: service_medium_4G
    + - cloud_properties:
    +     cpu: 2
    +     disk: 10240
    +     ram: 2048
    +   name: service_medium_2G
    + - cloud_properties:
    +     cpu: 1
    +     disk: 4096
    +     ram: 256
    +   name: portal_tiny
    + - cloud_properties:
    +     cpu: 1
    +     disk: 4096
    +     ram: 512
    +   name: portal_small
    + - cloud_properties:
    +     cpu: 1
    +     disk: 4096
    +     ram: 1024
    +   name: portal_medium
    + - cloud_properties:
    +     cpu: 1
    +     disk: 4096
    +     ram: 2048
    +   name: portal_large

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
    +     - z7
    +     cloud_properties:
    +       name: Internal
    +     dns:
    +     - 8.8.8.8
    +     gateway: 10.30.20.23
    +     range: 10.30.0.0/16
    +     reserved:
    +     - 10.30.0.0 - 10.30.100.255
    + - name: public
    +   subnets:
    +   - azs:
    +     - z1
    +     - z2
    +     - z3
    +     - z4
    +     - z5
    +     - z6
    +     - z7
    +     cloud_properties:
    +       name: External
    +     dns:
    +     - 8.8.8.8
    +     gateway: 115.68.46.177
    +     range: 115.68.46.176/28
    +     reserved:
    +     - 115.68.46.176 - 115.68.46.187
    +     static:
    +     - 115.68.46.188 - 115.68.46.190
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
    +     - z7
    +     cloud_properties:
    +       name: Internal
    +     dns:
    +     - 8.8.8.8
    +     gateway: 10.30.20.23
    +     range: 10.30.0.0/16
    +     reserved:
    +     - 10.30.0.0 - 10.30.49.255
    +     static:
    +     - 10.30.50.0 - 10.30.254.255
    + - name: service_public
    +   subnets:
    +   - azs:
    +     - z1
    +     - z2
    +     - z3
    +     - z4
    +     - z5
    +     - z6
    +     - z7
    +     cloud_properties:
    +       name: External
    +     dns:
    +     - 8.8.8.8
    +     gateway: 115.68.47.161
    +     range: 115.68.47.160/24
    +     reserved:
    +     - 115.68.47.161 - 115.68.47.174
    +     static:
    +     - 115.68.47.175 - 115.68.47.190
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
    +     - z7
    +     cloud_properties:
    +       name: External
    +     dns:
    +     - 8.8.8.8
    +     gateway: 115.68.46.209
    +     range: 115.68.46.208/28
    +     reserved:
    +     - 115.68.46.217 - 115.68.46.222
    +     static:
    +     - 115.68.46.213 - 115.68.46.215
    +   type: manual
    + - name: vip
    +   type: vip

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
    + - name: bpm
    +   sha1: f2bd126b17b3591160f501d88d79ccf0aba1ae54
    +   url: git+https://github.com/cloudfoundry-incubator/bpm-release
    +   version: 1.0.3
    + - name: on-demand-release
    +   version: '1.0'
    + - name: bosh-dns
    +   sha1: d1aadbda5d60c44dec4a429cda872cf64f6d8d0b
    +   url: https://bosh.io/d/github.com/cloudfoundry/bosh-dns-release?v=1.10.0
    +   version: 1.10.0

    + update:
    +   canaries: 1
    +   canary_watch_time: 5000-120000
    +   max_in_flight: 1
    +   serial: false
    +   update_watch_time: 5000-120000

    + addons:
    + - include:
    +     deployments:
    +     - paasta
    +     stemcell:
    +     - os: ubuntu-trusty
    +     - os: ubuntu-xenial
    +   jobs:
    +   - name: bosh-dns
    +     properties:
    +       api:
    +         client:
    +           tls: "<redacted>"
    +         server:
    +           tls: "<redacted>"
    +       cache:
    +         enabled: "<redacted>"
    +       health:
    +         client:
    +           tls: "<redacted>"
    +         enabled: "<redacted>"
    +         server:
    +           tls: "<redacted>"
    +     release: bosh-dns
    +   name: bosh-dns
    + - include:
    +     stemcell:
    +     - os: windows2012R2
    +     - os: windows2016
    +     - os: windows1803
    +     - os: windows2019
    +   jobs:
    +   - name: bosh-dns-windows
    +     properties:
    +       api:
    +         client:
    +           tls: "<redacted>"
    +         server:
    +           tls: "<redacted>"
    +       cache:
    +         enabled: "<redacted>"
    +       health:
    +         client:
    +           tls: "<redacted>"
    +         enabled: "<redacted>"
    +         server:
    +           tls: "<redacted>"
    +     release: bosh-dns
    +   name: bosh-dns-windows

    + variables:
    + - name: redis-password
    +   type: password
    + - name: "/dns_healthcheck_tls_ca"
    +   options:
    +     common_name: dns-healthcheck-tls-ca
    +     is_ca: true
    +   type: certificate
    + - name: "/dns_healthcheck_server_tls"
    +   options:
    +     ca: "/dns_healthcheck_tls_ca"
    +     common_name: health.bosh-dns
    +     extended_key_usage:
    +     - server_auth
    +   type: certificate
    + - name: "/dns_healthcheck_client_tls"
    +   options:
    +     ca: "/dns_healthcheck_tls_ca"
    +     common_name: health.bosh-dns
    +     extended_key_usage:
    +     - client_auth
    +   type: certificate
    + - name: "/dns_api_tls_ca"
    +   options:
    +     common_name: dns-api-tls-ca
    +     is_ca: true
    +   type: certificate
    + - name: "/dns_api_server_tls"
    +   options:
    +     ca: "/dns_api_tls_ca"
    +     common_name: api.bosh-dns
    +     extended_key_usage:
    +     - server_auth
    +   type: certificate
    + - name: "/dns_api_client_tls"
    +   options:
    +     ca: "/dns_api_tls_ca"
    +     common_name: api.bosh-dns
    +     extended_key_usage:
    +     - client_auth
    +   type: certificate

    + instance_groups:
    + - azs:
    +   - z3
    +   instances: 1
    +   name: mariadb
    +   networks:
    +   - name: service_private
    +   persistent_disk_type: 2GB
    +   stemcell: default
    +   syslog_aggregator: 
    +   templates:
    +   - name: mariadb
    +     release: on-demand-release
    +   vm_type: service_tiny
    + - azs:
    +   - z3
    +   instances: 1
    +   name: paas-ta-on-demand-broker
    +   networks:
    +   - name: service_private
    +   stemcell: default
    +   syslog_aggregator: 
    +   templates:
    +   - name: paas-ta-on-demand-broker
    +     release: on-demand-release
    +   vm_type: service_medium
    + - azs:
    +   - z3
    +   instances: 0
    +   jobs:
    +   - name: redis
    +     release: on-demand-release
    +   name: redis
    +   networks:
    +   - name: service_private
    +   persistent_disk: 1024
    +   stemcell: default
    +   vm_type: service_tiny
    + - azs:
    +   - z3
    +   instances: 1
    +   jobs:
    +   - name: sanity-tests
    +     release: on-demand-release
    +   lifecycle: errand
    +   name: sanity-tests
    +   networks:
    +   - name: service_private
    +   stemcell: default
    +   vm_type: service_tiny

    + name: on-demand-service-broker

    + properties:
    +   broker:
    +     bosh:
    +       client_id: "<redacted>"
    +       client_secret: "<redacted>"
    +       deployment_name: "<redacted>"
    +       instance_name: "<redacted>"
    +       oauth_url: "<redacted>"
    +       url: "<redacted>"
    +     cloudfoundry:
    +       admin:
    +         id: "<redacted>"
    +         password: "<redacted>"
    +       sslSkipValidation: "<redacted>"
    +       url: "<redacted>"
    +     datasource:
    +       password: "<redacted>"
    +     server:
    +       port: "<redacted>"
    +     service_instance:
    +       bullet:
    +         desc: "<redacted>"
    +         name: "<redacted>"
    +       guid: "<redacted>"
    +       name: "<redacted>"
    +       org_limitation: "<redacted>"
    +       plan:
    +         desc: "<redacted>"
    +         id: "<redacted>"
    +         name: "<redacted>"
    +       space_limitation: "<redacted>"
    +   mariadb:
    +     admin_user:
    +       password: "<redacted>"
    +     host_names:
    +     - "<redacted>"
    +     port: "<redacted>"
    +   redis:
    +     password: "<redacted>"

    Continue? [yN]: y

    Task 936164

    Task 936164 | 07:29:51 | Preparing deployment: Preparing deployment (00:00:07)
    Task 936164 | 07:29:59 | Preparing package compilation: Finding packages to compile (00:00:00)
    Task 936164 | 07:29:59 | Compiling packages: redis-4/a3c434216eff51abbb62eadf23487cee73ba4b3e
    Task 936164 | 07:29:59 | Compiling packages: java/86d8b8f8115418addf836753c1735abe547d4105
    Task 936164 | 07:29:59 | Compiling packages: mariadb/59a218308c6c7dcf8795b531b53aa4a1c666ce00
    Task 936164 | 07:29:59 | Compiling packages: paas-ta-on-demand-broker/060339465a66f4a7e030c3c7cd13e85f46667ccc
    Task 936164 | 07:32:42 | Compiling packages: java/86d8b8f8115418addf836753c1735abe547d4105 (00:02:43)
    Task 936164 | 07:32:48 | Compiling packages: paas-ta-on-demand-broker/060339465a66f4a7e030c3c7cd13e85f46667ccc (00:02:49)
    Task 936164 | 07:33:32 | Compiling packages: mariadb/59a218308c6c7dcf8795b531b53aa4a1c666ce00 (00:03:33)
    Task 936164 | 07:33:58 | Compiling packages: redis-4/a3c434216eff51abbb62eadf23487cee73ba4b3e (00:03:59)
    Task 936164 | 07:34:47 | Creating missing vms: paas-ta-on-demand-broker/13c11522-10dd-485c-bb86-3ac5337223d0 (0)
    Task 936164 | 07:34:47 | Creating missing vms: mariadb/e35f3ece-9c34-41f4-a88e-d8365e9b8c70 (0)
    Task 936164 | 07:36:12 | Creating missing vms: paas-ta-on-demand-broker/13c11522-10dd-485c-bb86-3ac5337223d0 (0) (00:01:25)
    Task 936164 | 07:36:13 | Creating missing vms: mariadb/e35f3ece-9c34-41f4-a88e-d8365e9b8c70 (0) (00:01:26)
    Task 936164 | 07:36:15 | Updating instance mariadb: mariadb/e35f3ece-9c34-41f4-a88e-d8365e9b8c70 (0) (canary)
    Task 936164 | 07:36:15 | Updating instance paas-ta-on-demand-broker: paas-ta-on-demand-broker/13c11522-10dd-485c-bb86-3ac5337223d0 (0) (canary) (00:00:46)
    Task 936164 | 07:39:05 | Updating instance mariadb: mariadb/e35f3ece-9c34-41f4-a88e-d8365e9b8c70 (0) (canary) (00:02:50)

    Task 936164 Started  Tue Jul  2 07:29:51 UTC 2019
    Task 936164 Finished Tue Jul  2 07:39:05 UTC 2019
    Task 936164 Duration 00:09:14
    Task 936164 done

    Succeeded
  ```

* 배포된 On-Demand-Redis 서비스팩을 확인한다.
* **사용 예시**

  ```text
    $bosh -e micro-bosh vms -d on-demand-service-broker
    Using environment '10.30.40.111' as user 'admin' (openid, bosh.admin)

    Task 936167. Done

    Deployment 'on-demand-service-broker'

    Instance                                                       Process State  AZ  IPs           VM CID                                   VM Type         Active  
    mariadb/e35f3ece-9c34-41f4-a88e-d8365e9b8c70                   running        z3  10.30.255.25  vm-5168ec8d-f42f-40fa-9c3a-8635bf138b0a  service_tiny    true  
    paas-ta-on-demand-broker/13c11522-10dd-485c-bb86-3ac5337223d0  running        z3  10.30.255.26  vm-eab6e832-8b7c-49bc-ac04-80258896880d  service_medium  true  

    2 vms

    Succeeded
  ```

### 3. CF CLI를 이용한 On-Demand-Redis 서비스 브로커 등록

Redis 서비스팩 배포가 완료 되었으면 Application에서 서비스 팩을 사용하기 위해서 먼저 On-Demand-Redis 서비스 브로커를 등록해 주어야 한다. 서비스 브로커 등록시에는 PaaS-TA에서 서비스 브로커를 등록할 수 있는 사용자로 로그인하여야 한다

#### 서비스 브로커 목록을 확인한다.

> `$ cf service-brokers`
>
> \`\`\` Getting service brokers as admin...

name url paasta-pinpoint-broker [http://10.30.70.82:8080](http://10.30.70.82:8080)

```text
##### On-Demand-Redis 서비스 브로커를 등록한다.
>`$ cf create-service-broker {서비스팩 이름} {서비스팩 사용자ID} {서비스팩 사용자비밀번호} http://{서비스팩 URL(IP)}`

  **서비스팩 이름** : 서비스 팩 관리를 위해 PaaS-TA에서 보여지는 명칭이다. 서비스 Marketplace에서는 각각의 API 서비스 명이 보여지니 여기서 명칭은 서비스팩 리스트의 명칭이다.<br>
  **서비스팩 사용자ID** / 비밀번호 : 서비스팩에 접근할 수 있는 사용자 ID입니다. 서비스팩도 하나의 API 서버이기 때문에 아무나 접근을 허용할 수 없어 접근이 가능한 ID/비밀번호를 입력한다.<br>
  **서비스팩 URL** : 서비스팩이 제공하는 API를 사용할 수 있는 URL을 입력한다.

>`$ cf create-service-broker on-demand-redis-service admin cloudfoundry http://10.30.255.26:8080
```

```text
Creating service broker on-demand-redis-service as admin...
OK
```

```text
##### 등록된 On-Demand-Redis 서비스 브로커를 확인한다.

>`$ cf service-brokers`
```

Getting service brokers as admin...

name url paasta-pinpoint-broker [http://10.30.70.82:8080](http://10.30.70.82:8080) on-demand-redis-service [http://10.30.255.26:8080](http://10.30.255.26:8080)

```text
##### 접근 가능한 서비스 목록을 확인한다.

>`$ cf service-access`
```

Getting service access as admin... broker: on-demand-redis-service service plan access orgs redis dedicated-vm none

```text
서비스 브로커 등록시 최초에는 접근을 허용하지 않는다. 따라서 access는 none으로 설정된다.


##### 특정 조직에 해당 서비스 접근 허용을 할당하고 접근 서비스 목록을 다시 확인한다. (전체 조직)

>`$ cf enable-service-access redis` <br>
>`$ cf service-access`
```

Getting service access as admin... broker: paasta-redis-broker service plan access orgs redis dedicated-vm all

```text
### <div id='31'> 3.1. PaaS-TA에서 서비스 신청
Sample App에서 Redis 서비스를 사용하기 위해서는 서비스 신청(Provision)을 해야 한다.
*참고: 서비스 신청시 PaaS-TA에서 서비스를 신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.

##### 먼저 PaaS-TA Marketplace에서 서비스가 있는지 확인을 한다.

>`$  cf marketplace`
```

OK service plans description redis dedicated-vm A paasta source control service for application development.provision parameters : parameters {owner : owner}

```text
<br>

##### Marketplace에서 원하는 서비스가 있으면 서비스 신청(Provision)을 한다.

>`$ cf create-service {서비스명} {서비스 플랜} {내 서비스명}`
- **서비스명** : redis로 Marketplace에서 보여지는 서비스 명칭이다.
- **서비스플랜** : 서비스에 대한 정책으로 plans에 있는 정보 중 하나를 선택한다. On-Demand-Redis 서비스는 dedicated-vm만 지원한다.
- **내 서비스명** : 내 서비스에서 보여지는 명칭이다. 이 명칭을 기준으로 환경 설정 정보를 가져온다.


>`$  cf create-service redis dedicated-vm redis`
```

OK

Create in progress. Use 'cf services' or 'cf service redis' to check operation status.

Attention: The plan `dedicated-vm` of service `redis` is not free. The instance `redis` will incur a cost. Contact your administrator if you think this is in error.

```text
<br>

##### 생성된 Redis 서비스 인스턴스의 status를 확인한다.
 * create in progress인 상태일경우 서비스 준비중이므로 서비스 이용 및 바인드, 삭제가 제한이되므로 create succeeded가 될때까지 기다려야 한다.
>`$ cf services`
```

Showing info of service redis in org system / space dev as admin...

name: redis service: redis tags:  
plan: dedicated-vm description: A paasta source control service for application development.provision parameters : parameters {owner : owner} documentation: [https://paas-ta.kr](https://paas-ta.kr) dashboard: 10.30.255.26

Showing status of last operation from service redis...

status: create in progress message:  
started: 2019-07-05T05:58:13Z updated: 2019-07-05T05:58:16Z

There are no bound apps for this service.

```text
##### 생성된 Redis 서비스 인스턴스의 status가 create succeeded가 된것을 확인한다.
```

Showing info of service redis in org system / space dev as admin...

name: redis service: redis tags:  
plan: dedicated-vm description: A paasta source control service for application development.provision parameters : parameters {owner : owner} documentation: [https://paas-ta.kr](https://paas-ta.kr) dashboard: 10.30.255.26

Showing status of last operation from service redis...

status: create succeeded message: test started: 2019-07-05T05:58:13Z updated: 2019-07-05T06:01:20Z

There are no bound apps for this service.

```text
<br>

### Secuirty-group에 redis_[서비스 할당된 space guid] 가 생성된것을 확인한다.
>`$ cf space [space] --guid`
```

$ cf space dev --guid 20bc9b52-c3d5-4cd2-94d9-7f444f9ab464

```text
>`$ cf security-groups`
```

Getting security groups as admin... OK

```text
 name                                         organization   space   lifecycle
```

## 0   abacus                                       abacus-org     dev     running

## 1   dns                                                       running

```text
 dns                                          <all>          <all>   staging
```

## 2   public\_networks                                           running

```text
 public_networks                              <all>          <all>   staging
```

## 3   redis\_20bc9b52-c3d5-4cd2-94d9-7f444f9ab464   system         dev     running

```text
### <div id='32'> 3.2. Sample App에 서비스 바인드 신청 및 App 확인
서비스 신청이 완료되었으면 Sample App 에서는 생성된 서비스 인스턴스를 Bind 하여 App에서 Redis 서비스를 이용한다.
*참고: 서비스 Bind 신청시 PaaS-TA에서 서비스 Bind신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.


##### Sample App 다운 및 manifest.yml을 생성한다.

>`$ wget -O redis.war http://45.248.73.44/index.php/s/GW7midoBGeQiCBC/download`

>`$ vi manifest.yml 을 아래와 같이 입력후 생성한다.`
```

applications:

* name: redis-sample

  memory: 700M

  instances: 1

  path: redis.war

  buildpacks: \[java\_buildpack\]

  \`\`\`

#### --no-start 옵션으로 App을 배포한다.

* -no-start: App 배포시 구동은 하지 않는다.

> `$ cf push -f manifest.yml --no-start`

```text
Creating app redis-sample...
Mapping routes...
Comparing local files to remote cache...
Packaging files to upload...
Uploading files...
 230.61 KiB / 230.61 KiB [========================================================================================================================================================================================================================================] 100.00% 1s

Waiting for API to complete processing files...

name:              redis-sample
requested state:   stopped
routes:            redis-sample.115.68.46.188.xip.io
last uploaded:     
stack:             
buildpacks:        

type:           web
instances:      0/1
memory usage:   700M
     state   since                  cpu    memory   disk     details
#0   down    2019-07-05T06:25:58Z   0.0%   0 of 0   0 of 0
```

#### Sample App에서 생성한 서비스 인스턴스 바인드 신청을 한다.

> `$ cf bind-service redis-sample redis`

```text
Binding service redis to app redis-sample in org system / space dev as admin...
OK
TIP: Use 'cf restage redis-sample' to ensure your env variable changes take effect
```

#### 바인드를 적용하기 위해서 App을 재기동한다.

> `$ cf restart redis-sample`
>
> \`\`\` Waiting for app to start...

name: redis-sample requested state: started routes: redis-sample.115.68.46.188.xip.io last uploaded: Fri 05 Jul 15:30:29 KST 2019 stack: cflinuxfs2 buildpacks: java\_buildpack

type: web instances: 1/1 memory usage: 700M start command: JAVA\_OPTS="-agentpath:$PWD/.java-buildpack/open\_jdk\_jre/bin/jvmkill-1.16.0\_RELEASE=printHeapHistogram=1 -Djava.io.tmpdir=$TMPDIR -Djava.ext.dirs=$PWD/.java-buildpack/container\_security\_provider:$PWD/.java-buildpack/open\_jdk\_jre/lib/ext -Djava.security.properties=$PWD/.java-buildpack/java\_security/java.security $JAVA\_OPTS" && CALCULATED\_MEMORY=$\($PWD/.java-buildpack/open\_jdk\_jre/bin/java-buildpack-memory-calculator-3.13.0\_RELEASE -totMemory=$MEMORY\_LIMIT -loadedClasses=13239 -poolType=metaspace -stackThreads=250 -vmOptions="$JAVA\_OPTS"\) && echo JVM Memory Configuration: $CALCULATED\_MEMORY && JAVA\_OPTS="$JAVA\_OPTS $CALCULATED\_MEMORY" && MALLOC\_ARENA\_MAX=2 SERVER\_PORT=$PORT eval exec $PWD/.java-buildpack/open\_jdk\_jre/bin/java $JAVA\_OPTS -cp $PWD/. org.springframework.boot.loader.WarLauncher state since cpu memory disk details

## 0   running   2019-07-05T06:44:00Z   0.0%   20.1M of 700M   126.3M of 1G

```text
<br>

##### App이 정상적으로 Redis 서비스를 사용하는지 확인한다.

##### curl 로 확인

>`$  export APP=redis-sample.[CF Domain]`

>`$ curl $APP/info `
```

redis.clients.jedis.Jedis@6f2c0944

```text
>`$ curl $APP/add/number/9 `
```

number : 9 ADD OK !

```text
>`$ curl $APP/get/number `
```

Get Redis Value ::9

```text
>`$ curl $APP/del/number `
```

delete Redis Key ::number

```text
>`$ curl $APP/get/number `
```

Get Redis Value ::null

\`\`\`   


## 4. Portal을 이용한 redis service test

사용자 및 관리자 포탈이 설치가 되어있으면 포탈을 통해서 레디스 서비스 신청 및 바인드, 테스트가 가능하다.

#### 관리자 포탈에 접속해 서비스 관리의 서비스 브로커 페이지에서 브로커 리스트를 확인한다..

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_test1.PNG)

#### On-Demand-Redis 서비스 브로커를 등록한다.

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_test2.PNG) ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_test3.PNG)

#### 등록된 On-Demand-Redis 서비스 브로커를 확인한다.

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_test4.PNG)

#### 서비스관리의 서비스 제어 페이지에서 접근 가능한 서비스 목록을 확인한다.

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_test5.PNG)

서비스 브로커 등록시 최초에는 접근을 허용하지 않는다. 따라서 access는 none으로 설정된다.

#### 특정 조직에 해당 서비스 접근 허용을 할당하고 접근 서비스 목록을 다시 확인한다. \(전체 조직\)

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_test6.PNG)

### 4.1. 서비스 신청

사용자 포탈에서 서비스 신청하기 위해서는 관리자 포탈의 카탈로그페이지에서 서비스 등록을 먼저 해주어야 사용이 가능하다.

#### 관리자 포탈의 운영관리의 카탈로그 페이지로 이동해 서비스 등록을 한다.

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_test7.PNG) 앱 바인드 파라미터는 app\_guid 자동입력을 추가, 온디멘드 Y 로 설정후 서비스 등록을 진행한다.

#### 사용자 포탈 로그인 후 카탈로그 페이지에서 서비스를 생성한다.

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_test8.PNG)

#### 생성된 Redis 서비스 인스턴스의 status를 확인한다.

Service status : in progress ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_test9.PNG) Service status : created succeed ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_test10.PNG)

### 관리자포탈 보안의 시큐리티그룹 페이지로 이동해 redis\_\[서비스 할당된 space guid\] 가 생성된것을 확인한다.

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_test11.PNG)

### 4.2. Sample App에 서비스 바인드 신청 및 App 확인

#### 관리자 포탈 운영관리의 카탈로그 페이지에서 redis-Sample App을 등록한다.

다운로드 페이지: [http://45.248.73.44/index.php/s/GW7midoBGeQiCBC/download](http://45.248.73.44/index.php/s/GW7midoBGeQiCBC/download) ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_test12.PNG)

#### 사용자 포탈 카탈로그 페이지에서 redis-Sample App을 배포한다.

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_test13.PNG)

#### Sample App에서 생성한 서비스 인스턴스 바인드 신청을 한다.

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_test14.PNG)

#### Sample App을 재기동한다.

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_test15.PNG)

#### App이 정상적으로 Redis 서비스를 사용하는지 확인한다.

#### APP URL을 클릭해 테스트를 진행한다.

> APP URL/info ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_test16.PNG) APP URL/add/number/8 ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_test17.PNG) APP URL/get/number ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_test18.PNG) APP URL/del/number ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_test19.PNG) APP URL/get/number ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_test20.PNG)

## 5. Redis Client 툴 접속

Application에 바인딩 된 Redis 서비스 연결정보는 Private IP로 구성되어 있기 때문에 Redis Client 툴에서 직접 연결할 수 없다. 따라서 Redis Client 툴에서 SSH 터널, Proxy 터널 등을 제공하는 툴을 사용해서 연결하여야 한다. 본 가이드는 SSH 터널을 이용하여 연결 하는 방법을 제공하며 Redis Client 툴로써는 오픈 소스인 Redis Desktop Manager로 가이드한다. Redis Desktop Manager 에서 접속하기 위해서 먼저 SSH 터널링할수 있는 VM 인스턴스를 생성해야한다. 이 인스턴스는 SSH로 접속이 가능해야 하고 접속 후 PaaS-TA에 설치한 서비스팩에 Private IP 와 해당 포트로 접근이 가능하도록 시큐리티 그룹을 구성해야 한다. 이 부분은 OpenStack 관리자 및 PaaS-TA 운영자에게 문의하여 구성한다. vsphere 에서 구성한 인스턴스는 공개키\(.pem\) 로 접속을 해야 하므로 공개키는 운영 담당자에게 문의하여 제공받는다. 참고\) 개인키\(.ppk\)로는 접속이 되지 않는다.

### 5.1. Redis Desktop Manager 설치 및 연결

Redis Desktop Manager 프로그램은 무료로 사용할 수 있는 오픈소스 소프트웨어이다.

#### Redis Desktop Manager를 다운로드 하기 위해 아래 URL로 이동하여 설치파일을 다운로드 한다.

[**http://redisdesktop.com/download**](http://redisdesktop.com/download) ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_image_14.png)

#### 다운로드한 설치파일을 실행한다.

> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_image_15.png)

#### Redis Desktop Manager 설치를 위한 안내사항이다. Next 버튼을 클릭한다.

> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_image_16.png)

#### 프로그램 라이선스에 관련된 내용이다. I Agree 버튼을 클릭한다.

> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_image_17.png)

#### Redis Desktop Manager를 설치할 경로를 설정 후 Install 버튼을 클릭한다.

별도의 경로 설정이 필요 없을 경우 default로 C드라이브 Program Files 폴더에 설치가 된다.

> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_image_18.png)

#### 설치 완료 후 Next 버튼을 클릭하여 다음 과정을 진행한다.

> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_image_19.png)

#### Finish 버튼 클릭으로 설치를 완료한다.

> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_image_20.png)

#### Redis Desktop Manager를 실행했을 때 처음 뜨는 화면이다. 이 화면에서 Server에 접속하기 위한 profile을 설정/저장하여 접속할 수 있다. Connect to Redis Server 버튼을 클릭한다.

> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_image_21.png)

#### Connection 탭에서 아래 붉은색 영역에 접속하려는 서버 정보를 모두 입력한다.

> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_image_22.png)

#### 서버 정보는 Application에 바인드 되어 있는 서버 정보를 입력한다. cfenv 명령어로 이용하여 확인한다.

예\) $ cfenvredis-example-app

> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_image_23.png)

#### SSH Tunnel탭을 클릭하고 PaaS-TA 운영 관리자에게 제공받은 SSH 터널링 가능한 서버 정보를 입력하고 공개키\(.pem\) 파일을 불러온다. Test Connection 버튼을 클릭하면 Redis 서버에 접속이 되는지 테스트 하고 OK 버튼을 눌러 Redis 서버에 접속한다.

\(참고\) 만일 공개키 없이 ID/Password로 접속이 가능한 경우에는 공개키 대신 사용자 이름과 암호를 입력한다.

> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_image_24.png)

#### 접속이 완료되고 좌측 서버 정보를 더블 클릭하면 좌측에 스키마 정보가 나타난다.

> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_image_25.png)

#### 신규 키 등록후 확인

> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/Service-Guide/images/redis/redis_image_26.png)

