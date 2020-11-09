## Table of Contents
1. [문서 개요](#1)
     * [1.1. 목적](#2)
     * [1.2. 범위](#3)
     * [1.3. 참고](#4)
2. [InfluxDB & Grafana Release 배포](#5)
     * [2.1.  upload release](#6)
     * [2.2.  manifest 파일 설정](#7)
     * [2.3.  deploy](#8)
     * [2.4.  확인](#9)

<div id='1'></div>

# 1. 문서 개요

<div id='2'></div>

### 1.1. 목적

본 문서는 IaaS(Infrastructure as a Service) 중 하나인 Openstack 환경에서 모니터링 시스템의 주요 정보인 로그를 저장하기 위한 Elasticsearch, 수집하기 위한 LogStash 및 화면으로 보여주기 위한 Kibana를 설치하기 위한 가이드를 제공하는데 그 목적이 있다

<div id='3'></div>

### 1.2. 범위

본 문서는 VMWare 기반에 설치하기 위한 내용으로 한정되어 있다.

<div id='4'></div>

### 1.3. 참고

> <a style="text-decoration:underline" href="https://github.com/PaaS-TA/Guide-3.0-Penne-/blob/master/Install-Guide/monitoring/PaaS-TA%20%EB%AA%A8%EB%8B%88%ED%84%B0%EB%A7%81%20%EC%8B%9C%EC%8A%A4%ED%85%9C%20%EC%84%A4%EC%B9%98%EA%B0%80%EC%9D%B4%EB%93%9C.md">모니터링 시스템 Architecutre</a>

<div id='5'></div>

# 2.  Logsearch Release 배포

본 장에서는 Logsearch 서비스를 배포하는 방법에 대해서 기술하였다.

<div id='6'></div>

### 2.1.  upload "Logsearch" release

하단 링크로 접속하여 PaaS-TA 모니터링 패키지 파일을 다운로드하여, 압축해제한다.

>PaaS-TA 모니터링 : **<https://paas-ta.kr/data/packages/2.0/PaaSTA-Monitoring.zip>** <br>
>다운로드 받은 PaaSTA-Monitoring.zip 파일을 압축해제한다. <br>
>paasta-logsearch-2.0.tgz 파일을 확인한다. <br>

다음의 명령어를 이용하여 릴리즈 파일을 bosh에 업로드한다.

$ bosh upload release paasta-logsearch-2.0.tgz

<kbd>![2-1-1]</kbd>
<kbd>![2-1-2]</kbd>

<div id='7'></div>

### 2.2.  manifest 파일 설정

1. "Logsearch" 서비스가 배포되는 환경에 맞게 manifest 파일의 설정 정보를 수정한다.

$ vi logsearch-release.yml

```
---
name: paasta-logsearch									#deployment name

compilation:
  cloud_properties:
    cpu: 2
    disk: 8192
    ram: 2048
  network: paasta-logsearch-network						#network name
  reuse_compilation_vms: true						
  workers: 6


director_uuid: <%= `bosh status --uuid` %>				#bosh uuid

disk_pools:
- cloud_properties: {}
  disk_size: 1024 										#disk pool size
  name: elasticsearch_master							#disk pool name

- cloud_properties: {}
  disk_size: 20480										#disk pool size
  name: elasticsearch_data								#disk pool name

- cloud_properties: {}
  disk_size: 1024
  name: queue

- cloud_properties: {}
  disk_size: 1024
  name: cluster_monitor


jobs:
- instances: 1
  name: elasticsearch_master							#job name
  networks:
  - name: paasta-logsearch-network						#network name
    static_ips:
    - 10.30.152.120										#local static ip
  persistent_disk_pool: elasticsearch_master		    #disk pool name
  properties:
    elasticsearch:
      node:
        allow_data: false
        allow_master: true
  resource_pool: elasticsearch_master					#resource name
  templates:
  - name: elasticsearch									#release job name
    release: paasta-logsearch							#release name
  update:
    max_in_flight: 1

- instances: 1
  name: cluster_monitor													
  networks:
  - name: paasta-logsearch-network
    static_ips:
    - 10.30.152.122
  persistent_disk_pool: cluster_monitor
  properties:																			
    curator:
      elasticsearch_host: 10.30.152.120				    #elasticsearch master ip
      elasticsearch_port: 9200							#elasticsearch master port
      purge_logs:
        retention_period: 7
    elasticsearch:
      cluster_name: monitor
      master_hosts:
      - 127.0.0.1
      node:
        allow_data: true
        allow_master: true
    elasticsearch_config:
      elasticsearch:
        host: 127.0.0.1
        port: 9200
      templates:
      - shards-and-replicas: '{ "template" : "*", "order" : 99, "settings" : { "number_of_shards"
          : 1, "number_of_replicas" : 0 } }'
      - index_template: /var/vcap/packages/logsearch-config/default-mappings.json
    kibana:
      elasticsearch:
        host: 127.0.0.1
        port: 9200
      memory_limit: 30
      port: 5601
      wait_for_templates:
      - shards-and-replicas
    logstash_ingestor:
      syslog:
        port: 514
    logstash_parser:
      filters:
      - monitor: /var/vcap/packages/logsearch-config/logstash-filters-monitor.conf
    nats_to_syslog:
      debug: true
      nats:
        machines:
        - 10.30.150.31			                    # paasta-controller nats IP
        password: nats
        port: 4222
        subject: '>'
        user: nats
      syslog:
        host: 127.0.0.1
        port: 514
    redis:
      host: 127.0.0.1
      maxmemory: 10
  resource_pool: cluster_monitor					#resource name
  templates:
  - name: queue
    release: paasta-logsearch
  - name: parser
    release: paasta-logsearch
  - name: ingestor_syslog
    release: paasta-logsearch
  - name: elasticsearch
    release: paasta-logsearch
  - name: elasticsearch_config
    release: paasta-logsearch
  - name: curator
    release: paasta-logsearch
  - name: kibana
    release: paasta-logsearch
  - name: nats_to_syslog
    release: paasta-logsearch

- instances: 1
  name: queue
  networks:
  - name: paasta-logsearch-network
    static_ips:
    - 10.30.152.123
  persistent_disk_pool: queue
  resource_pool: queue
  templates:
  - name: queue
    release: paasta-logsearch

- instances: 1
  name: maintenance
  networks:
  - name: paasta-logsearch-network
  resource_pool: maintenance
  templates:
  - name: elasticsearch_config
    release: paasta-logsearch
  - name: curator
    release: paasta-logsearch
  update:
    serial: true

- instances: 3
  name: elasticsearch_data
  networks:
  - name: paasta-logsearch-network
    static_ips:
    - 10.30.152.136									#local static ip
    - 10.30.152.137									#instance 개수와 static ip 개수 일치
    - 10.30.152.138
  persistent_disk_pool: elasticsearch_data
  properties:
    elasticsearch:
      node:
        allow_data: true
        allow_master: false
  resource_pool: elasticsearch_data
  templates:
  - name: elasticsearch
    release: paasta-logsearch
  update:
    max_in_flight: 1

- instances: 1
  name: kibana
  networks:
  - name: paasta-logsearch-network
    static_ips:
    - 10.30.152.126
  properties:
    route_registrar:
      routes:
      - name: kibana
        port: 5601
        registration_interval: 20s
        uris:
        - kibana.monitoring.open-paas.com
  resource_pool: kibana
  templates:
  - name: kibana
    release: paasta-logsearch
  update:
    max_in_flight: 1

- instances: 1
  name: ingestor
  networks:
  - name: paasta-logsearch-network
    static_ips:
    - 10.30.152.121
  properties:
    logstash_ingestor:
      debug: true
      relp:
        port: 2514
  resource_pool: ingestor
  templates:
  - name: ingestor_syslog
    release: paasta-logsearch
  - name: ingestor_relp
    release: paasta-logsearch
  - name: ingestor_bosh
    release: paasta-logsearch
  update:
    max_in_flight: 1

- instances: 1
  name: parser
  networks:
  - name: paasta-logsearch-network
  resource_pool: parser
  templates:
  - name: parser
    release: paasta-logsearch
  - name: elasticsearch
    release: paasta-logsearch
  update:
    max_in_flight: 4
    serial: false

- instances: 1
  name: ls-router
  networks:
  - name: paasta-logsearch-network
    static_ips:
    - 10.30.152.125
  - default:
    - dns
    - gateway
    name: external
    static_ips:
    - "xxx.xxx.xxx.xxx"                             #public ip
  properties:
    haproxy:										# haproxy 모듈의 Port Forwarding 정보
      cluster_monitor:
        backend_servers:
        - 10.30.152.122
      ingestor:
        backend_servers:
        - 10.30.152.121
        cf-service-backend_servers:
        - 10.30.152.121
        bosh-backend_servers:
        - 10.30.152.121
#      grafana:
#        backend_servers:
#        - 10.30.152.53
#      influxdb:
#        backend_servers:
#        - 10.30.152.51
      elastic:
        backend_servers:
        - 10.30.152.120
      kibana:
        backend_servers:
        - 10.30.152.126
      syslog_server: 10.30.152.122
  resource_pool: haproxy
  templates:
  - name: haproxy
    release: paasta-logsearch
  update:
    max_in_flight: 1

networks:
- name: paasta-logsearch-network						#internal network name
  subnets:
  - cloud_properties:
      name: Internal
    dns:
    - 10.30.20.27										#dns
    - 8.8.8.8
    gateway: 10.30.20.23								#gateway
    range: 10.30.0.0/16									#static ip range
    reserved:
    - 10.30.0.1 - 10.30.152.109							#reserved ip range
    static:
    - 10.30.152.110 - 10.30.152.150				    	#available ip range
  type: manual

- name: external										#external network name
  subnets:
  - cloud_properties:
      name: External
    dns:
    - 10.30.20.27										#dns
    - 8.8.8.8
    gateway: "xxx.xxx.xxx.xxx"							#gateway
    range: "xxx.xxx.xxx.xxx"/24							#public ip range
    static:
    - "xxx.xxx.xxx.xxx"									#available public ip
  type: manual

properties:
  curator:
    elasticsearch:
      host: 10.30.152.120
      port: 9200
  elasticsearch:
    cluster_name: logsearch
    exec: null
    master_hosts:
    - 10.30.152.120
  elasticsearch_config:
    elasticsearch:
      host: 10.30.152.120
    templates:
    - index_template: /var/vcap/packages/logsearch-config/default-mappings.json
  kibana:
    elasticsearch:
      host: 10.30.152.120
      port: 9200
  logstash_ingestor:
    debug: false
  logstash_parser:
    debug: false
  redis:
    host: 10.30.152.123

releases:
- name: paasta-logsearch							#release name
  version: 2.0										#release version

resource_pools:
- cloud_properties:
    cpu: 1
    disk: 4096
    ram: 1024
  env:
    bosh:
      password: $6$4gDD3aV0rdqlrKC$2axHCxGKIObs6tAmMTqYCspcdvQXh3JJcvWOY2WGb4SrdXtnCyNaWlrf3WEqvYR2MYizEGp3kMmbpwBC6jsHt0
  name: elasticsearch_master							#resource name
  network: paasta-logsearch-network						#networkname
  stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent		#stemcell name
    version: latest										#stemcell version

- cloud_properties:
    cpu: 1
    disk: 10240
    ram: 1024
  env:
    bosh:
      password: $6$4gDD3aV0rdqlrKC$2axHCxGKIObs6tAmMTqYCspcdvQXh3JJcvWOY2WGb4SrdXtnCyNaWlrf3WEqvYR2MYizEGp3kMmbpwBC6jsHt0
  name: elasticsearch_data
  network: paasta-logsearch-network
  stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent
    version: latest

- cloud_properties:
    cpu: 1
    disk: 2048
    ram: 1024
  env:
    bosh:
      password: $6$4gDD3aV0rdqlrKC$2axHCxGKIObs6tAmMTqYCspcdvQXh3JJcvWOY2WGb4SrdXtnCyNaWlrf3WEqvYR2MYizEGp3kMmbpwBC6jsHt0
  name: ingestor
  network: paasta-logsearch-network
  stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent
    version: latest

- cloud_properties:
    cpu: 1
    disk: 4096
    ram: 2048
  env:
    bosh:
      password: $6$4gDD3aV0rdqlrKC$2axHCxGKIObs6tAmMTqYCspcdvQXh3JJcvWOY2WGb4SrdXtnCyNaWlrf3WEqvYR2MYizEGp3kMmbpwBC6jsHt0
  name: queue
  network: paasta-logsearch-network
  stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent
    version: latest

- cloud_properties:
    cpu: 2
    disk: 4096
    ram: 2048
  env:
    bosh:
      password: $6$4gDD3aV0rdqlrKC$2axHCxGKIObs6tAmMTqYCspcdvQXh3JJcvWOY2WGb4SrdXtnCyNaWlrf3WEqvYR2MYizEGp3kMmbpwBC6jsHt0
  name: parser
  network: paasta-logsearch-network
  stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent
    version: latest

- cloud_properties:
    cpu: 1
    disk: 2048
    ram: 1024
  env:
    bosh:
      password: $6$4gDD3aV0rdqlrKC$2axHCxGKIObs6tAmMTqYCspcdvQXh3JJcvWOY2WGb4SrdXtnCyNaWlrf3WEqvYR2MYizEGp3kMmbpwBC6jsHt0
  name: kibana
  network: paasta-logsearch-network
  stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent
    version: latest

- cloud_properties:
    cpu: 1
    disk: 2048
    ram: 1024
  env:
    bosh:
      password: $6$4gDD3aV0rdqlrKC$2axHCxGKIObs6tAmMTqYCspcdvQXh3JJcvWOY2WGb4SrdXtnCyNaWlrf3WEqvYR2MYizEGp3kMmbpwBC6jsHt0
  name: maintenance
  network: paasta-logsearch-network
  stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent
    version: latest

- cloud_properties:
    cpu: 2
    disk: 4096
    ram: 2048
  env:
    bosh:
      password: $6$4gDD3aV0rdqlrKC$2axHCxGKIObs6tAmMTqYCspcdvQXh3JJcvWOY2WGb4SrdXtnCyNaWlrf3WEqvYR2MYizEGp3kMmbpwBC6jsHt0
  name: cluster_monitor
  network: paasta-logsearch-network
  stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent
    version: latest

- cloud_properties:
    cpu: 2
    disk: 2048
    ram: 1024
  env:
    bosh:
      password: $6$4gDD3aV0rdqlrKC$2axHCxGKIObs6tAmMTqYCspcdvQXh3JJcvWOY2WGb4SrdXtnCyNaWlrf3WEqvYR2MYizEGp3kMmbpwBC6jsHt0
  name: haproxy
  network: paasta-logsearch-network
  stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent
    version: latest

- cloud_properties:
    cpu: 1
    disk: 2048
    ram: 1024
  env:
    bosh:
      password: $6$4gDD3aV0rdqlrKC$2axHCxGKIObs6tAmMTqYCspcdvQXh3JJcvWOY2WGb4SrdXtnCyNaWlrf3WEqvYR2MYizEGp3kMmbpwBC6jsHt0
  name: errand
  network: paasta-logsearch-network
  stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent
    version: latest

update:
  canaries: 1
  canary_watch_time: 30000-600000
  max_errors: 1
  max_in_flight: 1
  serial: false
  update_watch_time: 5000-600000
```

2. manifest 파일 지정

$ bosh deployment logsearch-release.yml

<kbd>![2-2-1]</kbd>

<div id='8'></div>

### 2.3.  배포

$ bosh -n deploy

<kbd>![2-3-1]</kbd>
<kbd>![2-3-2]</kbd>

<div id='9'></div>

### 2.4.  확인

$ bosh deployments

<kbd>![2-4-1]</kbd>


[2-1-1]:images/logsearch/2-1-1.png
[2-1-2]:images/logsearch/2-1-2.png
[2-2-1]:images/logsearch/2-2-1.png
[2-3-1]:images/logsearch/2-3-1.png
[2-3-2]:images/logsearch/2-3-2.png
[2-4-1]:images/logsearch/2-4-1.png
