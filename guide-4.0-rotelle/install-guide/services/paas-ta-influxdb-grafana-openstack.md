# Table of Contents

1. [문서 개요](paas-ta-influxdb-grafana-openstack.md#1)
   * [1.1. 목적](paas-ta-influxdb-grafana-openstack.md#2)
   * [1.2. 범위](paas-ta-influxdb-grafana-openstack.md#3)
   * [1.3. 참고](paas-ta-influxdb-grafana-openstack.md#4)
2. [InfluxDB & Grafana Release 배포](paas-ta-influxdb-grafana-openstack.md#5)
   * [2.1.  upload release](paas-ta-influxdb-grafana-openstack.md#6)
   * [2.2.  manifest 파일 설정](paas-ta-influxdb-grafana-openstack.md#7)
   * [2.3.  deploy](paas-ta-influxdb-grafana-openstack.md#8)
   * [2.4.  확인](paas-ta-influxdb-grafana-openstack.md#9)
3. [InfluxDB WebAdmin](paas-ta-influxdb-grafana-openstack.md#10)
   * [3.1.  InfluxDB WebAdmin](paas-ta-influxdb-grafana-openstack.md#11)
4. [Grafana 설정](paas-ta-influxdb-grafana-openstack.md#12)
   * [3.1.  DataSource 설정](paas-ta-influxdb-grafana-openstack.md#13)

 \# 1. 문서 개요

### 1.1. 목적

본 문서는 IaaS\(Infrastructure as a Service\) 중 하나인 Openstack 환경에서 모니터링 시스템의 주요 정보인 시스템 metrics 를 저장히기 위한 InfluxDB와 View 화면을 제공하는 Grafana를 설치하기 위한 가이드를 제공하는데 그 목적이 있다.

 \#\#\# 1.2. 범위 본 문서는 Openstack 기반에 설치하기 위한 내용으로 한정되어 있다.

### 1.3. 참고

> [모니터링 시스템 Architecutre](https://github.com/PaaS-TA/Guide-3.0-Penne-/blob/master/Install-Guide/monitoring/PaaS-TA%20%EB%AA%A8%EB%8B%88%ED%84%B0%EB%A7%81%20%EC%8B%9C%EC%8A%A4%ED%85%9C%20%EC%84%A4%EC%B9%98%EA%B0%80%EC%9D%B4%EB%93%9C.md)

 \# 2. InfluxDB & Grafana Release 배포 본 장에서는 InfluxDB와 Grafana 서비스를 배포하는 방법에 대해서 기술하였다.

### 2.1.  upload "InfluxDB & Grafana" release

하단 링크로 접속하여 PaaS-TA 모니터링 패키지 파일을 다운로드하여, 압축해제한다.

> PaaS-TA 모니터링 : [https://paas-ta.kr/data/packages/3.0/PaaSTA-Monitoring.zip](https://paas-ta.kr/data/packages/3.0/PaaSTA-Monitoring.zip)
>
> 다운로드 받은 PaaSTA-Monitoring.zip 파일을 압축해제한다.   
>  paasta-influxdb-grafana-2.0.tgz 파일을 확인한다.   
>  다음의 명령어를 이용하여 릴리즈 파일을 bosh에 업로드한다.

$ bosh upload release paasta-influxdb-grafana-2.0.tgz

![](../../../.gitbook/assets/2-1-1%20%2832%29.png) ![](../../../.gitbook/assets/2-1-2%20%2819%29.png)

 \#\#\# 2.2. manifest 파일 설정 &gt; [PaaS-TA 모니터링 DB 및 Metrics 가이드](https://github.com/PaaS-TA/Guide-2.0-Linguine-/blob/master/Use-Guide/PaaS-TA%20%EB%AA%A8%EB%8B%88%ED%84%B0%EB%A7%81%20DB%20%EB%B0%8F%20Metrics%20%EA%B0%80%EC%9D%B4%EB%93%9C.md) 1. "InfluxDB & Grafana" 서비스가 배포되는 환경에 맞게 manifest 파일의 설정 정보를 수정한다. $ vi influxdb-grafana-release.yml \`\`\` --- name: paasta-influxdb-grafana \#deployment name compilation: \#paasta-influxdb-grafana Deployment 소스를 배포시 사용할 임시 Compile VM 설정 cloud\_properties: name: random instance\_type: monitoring \#인스턴스 크기\(openstack flavor\) availability\_zone: nova \#가용존\(available zone\) : Openstack에서 제공\(IaaS에서 정의\) network: paasta-influxdb-grafana-net \#network 명 reuse\_compilation\_vms: true workers: 2 director\_uuid:  \#bosh uuid releases: - name: paasta-influxdb-grafana \#release 명\(paasta-influxdb-grafana deployment가 사용할 Release\) version: latest \#release version disk\_pools: - cloud\_properties: type: gp2 \#Openstack IaaS에서 제공하는 Volumn Type \(IaaS에서 정의\) disk\_size: 10240 \#influxdb disk pool size name: influx\_data jobs: - name: influxdb \#influxdb service name templates: - name: influxdb release: paasta-influxdb-grafana instances: 1 resource\_pool: influx \#resource name persistent\_disk\_pool: influx\_data \#disk pool name networks: - default: - gateway - dns name: paasta-influxdb-grafana-net \#Influxdb VM이 사용할 내부 network 명 static\_ips: - 10.10.18.51 \#PaaS-TA 내부 IP - name: elastic \#Influxdb VM이 사용할 외부 내트워크명 static\_ips: - "xxx.xxx.xxx.xxx" \#public IP properties: influxdb: database: cf\_metric\_db \#InfluxDB default database user: root \#admin account password: root \#admin password replication: 1 ips: 10.10.18.51 \#local IP - name: grafana \#grafana service name templates: - name: grafana release: paasta-influxdb-grafana instances: 1 resource\_pool: grafana \#resoure name networks: - default: - gateway - dns name: paasta-influxdb-grafana-net \#grafana VM이 사용할 내부 network 명 static\_ips: - 10.10.18.53 \#local IP - name: elastic \#grafana VM이 사용할 외부 network명 static\_ips: - "xxx.xxx.xxx.xxx" \#public ip properties: \#grafana에서 접속할 InfluxDB 설정 grafana: listen\_port: 3000 \#grafana listen port admin\_username: admin \#grafana admin account admin\_password: admin \#grafana admin password users: allow\_sign\_up: true auto\_assign\_organization: true datasource: \#grafana에서 접속할 InfluxDB 설정 url: http://10.10.18.51:8086/ \#grafana에서 접속할 InfluxDB URL name: Grafanadb database\_type: Influxdb \#grafana에서 Influx에 접속할 DB TYPE user: root \#grafana에서 Influx에 접속할 계정 password: root \#grafana에서 Influx에 접속할 Password database\_name: cf\_metric\_db \#grafana에서 Influx에 접속할 default Database networks: - name: paasta-influxdb-grafana-net \#내부 network 정의 subnets: - cloud\_properties: name: random net\_id: b7c8c08f-2d3b-4a86-bd10-641cb6faa317 \#openstack subnet id security\_groups: \[bosh\] \#security group\(IaaS에서 정의\) dns: - 10.10.18.2 \#dns gateway: 10.10.18.1 \#gateway range: 10.10.18.0/24 \#static ip range\(cider\) reserved: - 10.10.18.2 - 10.10.18.49 \#reserved ip range static: - 10.10.18.50 - 10.10.18.55 \#available ip range type: manual - name: elastic \#외부 network 정의 type: vip cloud\_properties: net\_id: bb29696d-c0af-4a98-9108-edd23b27c493 security\_groups: \[bosh\] \#security group\(IaaS에서 정의\) properties: resource\_pools: - cloud\_properties: name: random instance\_type: m1.xlarge \#openstack instace type\(IaaS에서 정의\) availability\_zone: nova \#available zone name: influx \#resource name network: paasta-influxdb-grafana-net \#network name stemcell: name: bosh-openstack-kvm-ubuntu-trusty-go\_agent \#stemcell name version: latest \#stemcell version - cloud\_properties: name: random instance\_type: m1.xlarge \#openstack instace type\(IaaS에서 정의\) availability\_zone: nova \#available zone name: grafana \#resource name network: paasta-influxdb-grafana-net \#network name stemcell: name: bosh-openstack-kvm-ubuntu-trusty-go\_agent \#stemcell name version: latest \#stemcell version update: canaries: 1 canary\_watch\_time: 1000-180000 max\_in\_flight: 1 serial: true update\_watch\_time: 1000-180000 \`\`\` 2. manifest 파일 지정 $ bosh deployment influxdb-grafana-release.yml !\[2-2-1\]

### 2.3.  배포

$ bosh -n deploy

![](../../../.gitbook/assets/2-3-1%20%2834%29.png) ![](../../../.gitbook/assets/2-3-2%20%2819%29.png)

 \#\#\# 2.4. 확인 $ bosh deployments !\[2-4-1\]

## 3.  InfluxDB WebAdmin

 \#\#\# 3.1. InfluxDB WebAdmin 접속 1. InfluxDB WebAdmin에 접속 한다. 접속 URL은 InfluxDB Instance의 public ip로 접근 ex\)xxx.xxx.xxx.xxx:8083 2. 화면 상단 Database를 클릭하면 bosh\_metric\_db, cf\_metric\_db, container\_metric\_db가 존재하는지 확인한다. !\[2-6-1\] 3. Database를 cf\_metric\_db로 선택하고 SHOW MEASUREMENTS 명령어 실행하여 measurement를 확인한다. !\[2-6-2\] 4. cf\_metrics metric Data를 조회한다. !\[2-6-3\]

## 3.  Grafana 설정

### 3.1.  DataSource 설정

1. Grafana에 접속하여 로그인 한다. 접속 URL은 Grafana Instance의 public ip로 접근 계정: admin/admin \( influxdb-grafana-release.yml 에서 계정 설정 가능\) ![](../../../.gitbook/assets/2-5-1%20%283%29.png)
2. 화면 좌측에 PaaS-TA Monitor를 클릭하여 DataSource를 클릭한다.

   ![](../../../.gitbook/assets/2-5-2%20%283%29.png)

3. Add data Source를 클릭하고 InfluxDB DataSource설정정보를 등록한다. ![](../../../.gitbook/assets/2-5-3%20%282%29.png)
4. 등록된 DataSource를 확인한다.

![](../../../.gitbook/assets/2-5-4%20%283%29.png)

