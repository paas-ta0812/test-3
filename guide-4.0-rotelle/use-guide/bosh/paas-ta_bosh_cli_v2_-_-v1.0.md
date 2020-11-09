# PaaS-TA\_BOSH\_CLI\_V2\_사용자\_가이드v1.0

## Table Contents

1. [개요](paas-ta_bosh_cli_v2_-_-v1.0.md#1)
   * [문서 목적](paas-ta_bosh_cli_v2_-_-v1.0.md#2)
   * [문서 범위](paas-ta_bosh_cli_v2_-_-v1.0.md#3)
   * [참고 자료](paas-ta_bosh_cli_v2_-_-v1.0.md#4)
2. [BOSH CLI 기본 사용법](paas-ta_bosh_cli_v2_-_-v1.0.md#5)
3. [BOSH CLI - Environments](paas-ta_bosh_cli_v2_-_-v1.0.md#6)
   * [bosh environments](paas-ta_bosh_cli_v2_-_-v1.0.md#7)
   * [bosh create-env](paas-ta_bosh_cli_v2_-_-v1.0.md#8)
   * [bosh alias-env](paas-ta_bosh_cli_v2_-_-v1.0.md#9)
   * [bosh environment](paas-ta_bosh_cli_v2_-_-v1.0.md#10)
   * [bosh delete-env](paas-ta_bosh_cli_v2_-_-v1.0.md#11)
4. [BOSH CLI - Session](paas-ta_bosh_cli_v2_-_-v1.0.md#12)
   * [bosh log-in](paas-ta_bosh_cli_v2_-_-v1.0.md#13)
   * [bosh log-out](paas-ta_bosh_cli_v2_-_-v1.0.md#14)
5. [BOSH CLI - Stemcell](paas-ta_bosh_cli_v2_-_-v1.0.md#15)
   * [bosh Stemcells](paas-ta_bosh_cli_v2_-_-v1.0.md#16)
   * [bosh upload-stemcell](paas-ta_bosh_cli_v2_-_-v1.0.md#17)
   * [bosh delete-stemcell](paas-ta_bosh_cli_v2_-_-v1.0.md#18)
   * [bosh repack-stemcell](paas-ta_bosh_cli_v2_-_-v1.0.md#19)
6. [BOSH CLI - Release creation](paas-ta_bosh_cli_v2_-_-v1.0.md#20)
   * [bosh init-release](paas-ta_bosh_cli_v2_-_-v1.0.md#20)
   * [bosh generate-job](paas-ta_bosh_cli_v2_-_-v1.0.md#21)
   * [bosh generate-package](paas-ta_bosh_cli_v2_-_-v1.0.md#22)
   * [bosh vendor-package](paas-ta_bosh_cli_v2_-_-v1.0.md#23)
   * [bosh create-release](paas-ta_bosh_cli_v2_-_-v1.0.md#24)
   * [bosh finalize-release](paas-ta_bosh_cli_v2_-_-v1.0.md#25)
   * [bosh reset-release](paas-ta_bosh_cli_v2_-_-v1.0.md#26)
7. [BOSH CLI - Release blobs](paas-ta_bosh_cli_v2_-_-v1.0.md#27)
   * [bosh blob](paas-ta_bosh_cli_v2_-_-v1.0.md#28)
   * [bosh add-blob](paas-ta_bosh_cli_v2_-_-v1.0.md#29)
   * [bosh reomove-blob](paas-ta_bosh_cli_v2_-_-v1.0.md#30)
   * [bosh sync-blob](paas-ta_bosh_cli_v2_-_-v1.0.md#31)
8. [BOSH CLI - Releases](paas-ta_bosh_cli_v2_-_-v1.0.md#32)
   * [bosh releases](paas-ta_bosh_cli_v2_-_-v1.0.md#33)
   * [bosh upload-release](paas-ta_bosh_cli_v2_-_-v1.0.md#34)
   * [bosh delete-release](paas-ta_bosh_cli_v2_-_-v1.0.md#35)
   * [bosh export-release](paas-ta_bosh_cli_v2_-_-v1.0.md#36)
   * [bosh inspect-release](paas-ta_bosh_cli_v2_-_-v1.0.md#37)
9. [BOSH CLI - Configs](paas-ta_bosh_cli_v2_-_-v1.0.md#38)
   * [bosh configs](paas-ta_bosh_cli_v2_-_-v1.0.md#39)
   * [bosh update-config](paas-ta_bosh_cli_v2_-_-v1.0.md#40)
   * [bosh delete-config](paas-ta_bosh_cli_v2_-_-v1.0.md#41)
10. [BOSH CLI - Cloud config](paas-ta_bosh_cli_v2_-_-v1.0.md#42)
    * [bosh cloud-configs](paas-ta_bosh_cli_v2_-_-v1.0.md#43)
    * [bosh update-cloud-config](paas-ta_bosh_cli_v2_-_-v1.0.md#44)
11. [BOSH CLI - Runtime config](paas-ta_bosh_cli_v2_-_-v1.0.md#45)
    * [bosh runtime-configs](paas-ta_bosh_cli_v2_-_-v1.0.md#46)
    * [bosh update-runtime-config](paas-ta_bosh_cli_v2_-_-v1.0.md#47)
12. [BOSH CLI – CPI config](paas-ta_bosh_cli_v2_-_-v1.0.md#48)
    * [bosh cpi-configs](paas-ta_bosh_cli_v2_-_-v1.0.md#49)
    * [bosh update-cpi-config](paas-ta_bosh_cli_v2_-_-v1.0.md#50)
13. [BOSH CLI - Deployments](paas-ta_bosh_cli_v2_-_-v1.0.md#51)
    * [bosh deployments](paas-ta_bosh_cli_v2_-_-v1.0.md#52)
    * [bosh deployment](paas-ta_bosh_cli_v2_-_-v1.0.md#53)
    * [bosh deploy](paas-ta_bosh_cli_v2_-_-v1.0.md#54)
    * [bosh delete-deployment](paas-ta_bosh_cli_v2_-_-v1.0.md#55)
    * [bosh delete-instance](paas-ta_bosh_cli_v2_-_-v1.0.md#56)
    * [bosh manifest](paas-ta_bosh_cli_v2_-_-v1.0.md#57)
    * [bosh recreate](paas-ta_bosh_cli_v2_-_-v1.0.md#58)
    * [bosh restart](paas-ta_bosh_cli_v2_-_-v1.0.md#59)
    * [bosh start](paas-ta_bosh_cli_v2_-_-v1.0.md#60)
    * [bosh stop](paas-ta_bosh_cli_v2_-_-v1.0.md#61)
    * [bosh ignore](paas-ta_bosh_cli_v2_-_-v1.0.md#62)
    * [bosh unignore](paas-ta_bosh_cli_v2_-_-v1.0.md#63)
    * [bosh logs](paas-ta_bosh_cli_v2_-_-v1.0.md#64)
14. [BOSH CLI - VMs](paas-ta_bosh_cli_v2_-_-v1.0.md#65)
    * [bosh vms](paas-ta_bosh_cli_v2_-_-v1.0.md#66)
    * [bosh delete-vms](paas-ta_bosh_cli_v2_-_-v1.0.md#67)
15. [BOSH CLI - Disks](paas-ta_bosh_cli_v2_-_-v1.0.md#68)
    * [bosh disks](paas-ta_bosh_cli_v2_-_-v1.0.md#69)
    * [bosh attach-disk](paas-ta_bosh_cli_v2_-_-v1.0.md#70)
    * [bosh delete-disk](paas-ta_bosh_cli_v2_-_-v1.0.md#71)
16. [BOSH CLI - SSH](paas-ta_bosh_cli_v2_-_-v1.0.md#72)
    * [bosh ssh](paas-ta_bosh_cli_v2_-_-v1.0.md#73)
    * [bosh scp-disk](paas-ta_bosh_cli_v2_-_-v1.0.md#74)
17. [BOSH CLI - Errands](paas-ta_bosh_cli_v2_-_-v1.0.md#75)
    * [bosh errands](paas-ta_bosh_cli_v2_-_-v1.0.md#76)
    * [bosh run-errand](paas-ta_bosh_cli_v2_-_-v1.0.md#77)
18. [BOSH CLI - Tasks](paas-ta_bosh_cli_v2_-_-v1.0.md#78)
    * [bosh tasks](paas-ta_bosh_cli_v2_-_-v1.0.md#79)
    * [bosh task](paas-ta_bosh_cli_v2_-_-v1.0.md#80)
    * [bosh cancle-task](paas-ta_bosh_cli_v2_-_-v1.0.md#81)
19. [BOSH CLI - Snapshot](paas-ta_bosh_cli_v2_-_-v1.0.md#82)
    * [bosh snapshots](paas-ta_bosh_cli_v2_-_-v1.0.md#83)
    * [bosh take-snapshot](paas-ta_bosh_cli_v2_-_-v1.0.md#84)
    * [bosh delete-snapshot](paas-ta_bosh_cli_v2_-_-v1.0.md#85)
    * [bosh delete-snapshots](paas-ta_bosh_cli_v2_-_-v1.0.md#86)
20. [BOSH CLI - Deployment recovery](paas-ta_bosh_cli_v2_-_-v1.0.md#87)
    * [bosh update-resurrection](paas-ta_bosh_cli_v2_-_-v1.0.md#88)
    * [bosh cloud-check](paas-ta_bosh_cli_v2_-_-v1.0.md#89)
    * [bosh locks](paas-ta_bosh_cli_v2_-_-v1.0.md#90)
21. [BOSH CLI - Misc](paas-ta_bosh_cli_v2_-_-v1.0.md#91)
    * [bosh clean-up](paas-ta_bosh_cli_v2_-_-v1.0.md#92)
    * [bosh help](paas-ta_bosh_cli_v2_-_-v1.0.md#93)
    * [bosh interpolate](paas-ta_bosh_cli_v2_-_-v1.0.md#94)

## 문서 개요

### 문서 목적

본 문서는 BOSH에 대한 설치 및 운영 관리를 위한 도구인 BOSH CLI v2에 대해 기본 사용법 및 사용 예시를 통해 BOSH를 이해하는데 목적이 있다.

### 문서 범위

본 문서에서는 BOSH CLI V2 사용법에 대해서 작성하였다.

### 참고 자료

본 문서는 Cloud Foundry의 BOSH Document\([http://bosh.io](http://bosh.io)\)를 참고로 작성하였다.

## BOSH CLI 기본 사용법

CLI는 BOSH 배포와 Release를 관리하기 위해 도움을 주는 커맨드 라인 명령어로 아래와 같이 구분된다.

* bosh-cli: BOSH를 관리하기 위한 CLI
* **기본 Syntax**

  ```text
    $ bosh [<options>] <command> [<args>]
  ```

  bosh 명령어에 대괄호로 묶인 인자인 과 는 명령어에 따라 선택적으로 사용되고,  인자는 필수 인자이다.

* **Options**

  | **번호** | **옵션** | **설명** |
  | :--- | :--- | :--- |
  | 1 | -c, --config | BOSH configuration file 지정 |
  | 2 | --ca-cert | Director 및 UAA 연결에 사용 되는 CA 인증서 지정 |
  | 3 | --client | 사용자 이름 또는 UAA 클라이언트 재정의 |
  | 4 | -n | 입력 사용이 필요한 확인 |
  | 5 | --json | 출력 형식을 JSON으로 변경 |
  | 6 | --tty | 명령이 리디렉션되지 않을 때 일반적으로 표시되는 모든 텍스트를 출력에 포함 |
  | 7 | --no-color | 색상을 비활성화 |
  | 8 | --deployment, -d | Deploy 명령을 위한 배치 지정 |
  | 9 | -h, --help | Help 메시지 보기 |
  | 10 | --column= | 지정된 열만 표시하도록 필터링 |
  | 11 | -e, --enviroment | SHA256 체크섬 사용 |
  | 12 | --sha2 | BOSH 배포파일 지정 |
  | 13 | --parallel= | 병렬 작업의 최대 수 |
  | 14 | --client-secret= | 암호 또는 UAA 클라이언트 암호 재정의 |

## BOSH CLI - Environments

### _**bosh environments**_

* **기본 Syntax**

  ```text
    $ bosh environments (Alias: envs)
  ```

* **설명**

  ```text
    BOSH CLI에 등록 한 디렉터의 별명이 지정된 환경을 나열
  ```

* **파라미터**
* **사용 예시**

  ```text
    $ bosh envs
    URL              Alias
    104.154.171.255  gcp
    192.168.56.6     vbox
    2 environments
    Succeeded
  ```

### _**bosh create-env**_

* **기본 Syntax**

  ```text
    $ bosh create-env [deploymentFile] [--state path] [-v ...] [-o ...] [--vars-store path]
  ```

* **설명**

  BOSH CLI를 통해 Manifest File을 기반으로 단일 VM을 생성. 일반적으로 Director 환경을 만드는 데 사용

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | deploymentFile | 설치 Manfiest 파일 | O |
  | --state path | Deployment state 파일 경로 | X |
  | -v | Manifest Replace 변수 ex\) internal\_ip, deployment\_name | X |
  | -o | option Manifest File ex\) jumpbox-user.yml, uaa.yml... | X |
  | --vars -store path | creds.yml 파일, 인증 키 및 Job Password yml 파일 경로 | X |

* **사용 예시**

  ```text
    $ bosh create-env ~/workspace/bosh-deployment/bosh.yml \
      --state state.json \
    --vars-store ./creds.yml \
      -o ~/workspace/bosh-deployment/virtualbox/cpi.yml \
      -o ~/workspace/bosh-deployment/virtualbox/outbound-network.yml \
      -o ~/workspace/bosh-deployment/bosh-lite.yml \
      -o ~/workspace/bosh-deployment/jumpbox-user.yml \
      -v director_name=vbox \
      -v internal_ip=192.168.56.6 \
      -v internal_gw=192.168.56.1 \
      -v internal_cidr=192.168.56.0/24 \
      -v network_name=vboxnet0 \
      -v outbound_network_name=NatNetwork
  ```

### _**bosh alias-env**_

* **기본 Syntax**

  ```text
    $ bosh alias-env [name] -e [location] [--ca-cert=path]
  ```

* **설명**

  BOSH CLI를 통해 엑세스 할 디렉터의 별명이 지정

* **파라미터**

| **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
| :--- | :--- | :--- |
| name | 환경 이름 지정 | O |
| location | 디렉터 위치 지정 | O |
| --ca-cert=path | CA 인증서를 지정 | X |

### _**bosh environment**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] environment (Alias: env)
  ```

* **설명**

  해당 Director 정보를 출력

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |

* **사용 예시**

  ```text
    $ bosh -e vbox env
    Using environment '192.168.56.6' as '?'

    Name      vbox
    UUID      eeb27cc6-467e-4c1d-a8f9-f1a8de759f52
    Version   260.5.0 (00000000)
    CPI       warden_cpi
    Features  compiled_package_cache: disabled
      dns: disabled
      snapshots: disabled
    User      admin

    Succeeded
  ```

### _**bosh delete-env**_

* **기본 Syntax**

  ```text
    $ bosh delete-env [deploymentFile] [--state path] [-v ...] [-o ...] [--vars-store path]
  ```

* **설명**

  매니페스트를 기반으로 이전에 만든 VM을 삭제, create-env 명령에 제공된 것과 동일한 플래그를 delete-env 명령에 제공해야한다.

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | deploymentFile | 설치 한 Manfiest 파일 | O |
  | --state path | Deployment state 파일 경로 | O |
  | -v | Manifest Replace 변수 ex\) internal\_ip, deployment\_name | X |
  | -o | option Manifest File ex\) jumpbox-user.yml, uaa.yml… | X |
  | --vars -store path | creds.yml 파일, 인증 키 및 Job Password yml 파일 경로 | X |

* **사용 예시**

  ```text
    $ bosh delete-env ~/workspace/bosh-deployment/bosh.yml \
      --state state.json \
      --vars-store ./creds.yml \
      -o ~/workspace/bosh-deployment/virtualbox/cpi.yml \
      -o ~/workspace/bosh-deployment/virtualbox/outbound-network.yml \
      -o ~/workspace/bosh-deployment/bosh-lite.yml \
      -o ~/workspace/bosh-deployment/jumpbox-user.yml \
      -v director_name=vbox \
     -v internal_ip=192.168.56.6 \
      -v internal_gw=192.168.56.1 \
      -v internal_cidr=192.168.56.0/24 \
      -v network_name=vboxnet0 \
      -v outbound_network_name=NatNetwork
  ```

## BOSH CLI - Session

### _**bosh log-in**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] l
  ```

* **설명**

  주어진 사용자를 Director에 로그인합니다.

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | BOSH 지정 한 Director 환경 이름 명칭 | O |

* **사용 예시**

  ```text
    $ bosh -e my-env l
    User (): admin
    Password ():
  ```

### _**bosh log-out**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] log-out 
  ```

* **설명**

  현재 접속 한 디렉터 로그아웃

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | BOSH 지정 한 Director 환경 이름 명칭 | O |

* **사용 예시**

  ```text
    $ bosh log-out 
    Logged out from '192.168.10.241'

    Succeeded
  ```

## BOSH CLI - Stemcells

### _**bosh Stemcells**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] stemcells (Alias: ss)
  ```

* **설명**

  업로드 한 릴리즈 조회

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |

* **사용 예시**

  ```text
    $ bosh -e my-env ss
    Using environment '192.168.56.6' as '?'

    Name                                         Version  OS             CPI  CID
    bosh-warden-boshlite-ubuntu-trusty-go_agent  3363*    ubuntu-trusty  -    6cbb176a-6a43-42...
    ~                                            3312     ubuntu-trusty  -    43r3496a-4rt3-52...
    bosh-warden-boshlite-centos-7-go_agent       3363*    centos-7       -    38yr83gg-349r-94...

    (*) Currently deployed

    3 stemcells

    Succeeded
  ```

### _**bosh upload-stemcell**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] upload-stemcell [location] [--sha1=digest] [--fix]
  ```

* **설명**

  스템셀 업로드

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | location | 스템셀 파일 위치 및 URL 지정 | X |
  | --sha1 | 스템셀 파일 sha1um 값 확인 | X |
  | --fix | 이전에 업로드 한 스템 셀을 동일한 이름과 버전으로 교체 | X |

* **사용 예시**

  ```text
    $ bosh -e my-env us ~/Downloads/bosh-stemcell-3468.17-warden-boshlite-ubuntu-trusty-go_agent.tgz

    $ bosh -e my-env us https://bosh.io/d/stemcells/bosh-stemcell-warden-boshlite-ubuntu-trusty-go_agent?v=3468.17
  ```

### _**bosh delete-stemcell**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] delete-stemcell [name]/[version]
  ```

* **설명**

  업로드 한 스템셀 삭제

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | name | 삭제 할 스템셀 명 | O |
  | version | 삭제 할 스템셀 버전 | O |

* **사용 예시**

  ```text
    $ bosh -e my-env delete-stemcell bosh-warden-boshlite-ubuntu-trusty-go_agent/3468.17
  ```

### _**bosh repack-stemcell**_

* **기본 Syntax**

  ```text
    $ bosh repack-stemcell src.tgz dst.tgz [--name=name] [--version=ver] [--cloud-properties=json-string]
  ```

* **설명**

  기존 스템셀의 이름, 버전 및 클라우드 등록 정보와 같은 업데이트 된 등록 정보로 새로운 스템셀 타르볼을 생성 참조 URL: [https://bosh.io/docs/repack-stemcell.html](https://bosh.io/docs/repack-stemcell.html)

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | name | 업데이트 등록 스템셀 명 | O |
  | version | 업데이트 등록 스템셀 버전 | X |
  | cloud-properties | 업데이트 등록 스템셀 cloud-properties, Json 형식 | X |

* **사용 예시**

  ```text
    $ bosh repack-stemcell --name=acme-ubuntu-encrypted --cloud-properties='{"encrypted": true, "kms_key_arn": "arn:aws:kms:us-east-1:088444384256:key/4ffbe966-d138-4f4d-a077-4c234d05b3b1"}' bosh-stemcell-3363.9-aws-xen-hvm-ubuntu-trusty-go_agent.tgz acme-encrypted-stemcell.tgz
  ```

## BOSH CLI - Release creation

### _**bosh init-release**_

* **기본 Syntax**

  ```text
    $ bosh init-release [--git] [--dir=dir]
  ```

* **설명**

  dir에 릴리즈에 관련한 구성 파일을 생성 dir을 사용 않할 경우는 현재 디렉토리

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | --git | BOSH 릴리즈 Git repository에 적절한 .gitignore 파일을 생성 | X |
  | --dir | 디렉토리에 대한 빈 릴리스 구성 파일 생성 | X |

* **파라미터**

  ```text
    $ bosh init-release --git --dir release-dir
    $ cd release-dir
  ```

### _**bosh generate-job**_

* **기본 Syntax**

  ```text
    $ bosh generate-job [name] [--dir=dir]
  ```

* **설명**

  dir에 릴리즈에 대한 Job에 관련 한 빈 파일 생성

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | name | 릴리즈 Job 명칭 | O |
  | --dir | 디렉토리에 대한 Job 관련 빈 릴리스 구성 파일 생성 | X |

* **사용 예시**

  ```text
    $ bosh generate-job jenkins
  ```

### _**bosh generate-package**_

* **기본 Syntax**

  ```text
    $ bosh generate-pakage [name] [--dir=dir]
  ```

* **설명**

  dir에 릴리즈에 대한 pakage에 관련 한 빈 파일 생성

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | name | 릴리즈 pakage 명칭 | O |
  | --dir | 디렉토리에 pakage Job 관련 빈 릴리스 구성 파일 생성 | X |

* **사용 예시**

  ```text
    $ bosh generate-package jenkins
  ```

### _**bosh vendor-package**_

* **기본 Syntax**

  ```text
    $ bosh vendor-package [name] src-dir [--dir=dir]
  ```

* **설명**

  다른 릴리스의 패키지를 dir의 릴리스로 제공, 릴리즈를 만들 때 CLI가 특정 패키리를 참조 하도록 디렉토리에 spec.lock을 포함 설명 참조 [https://bosh.io/docs/package-vendoring.html](https://bosh.io/docs/package-vendoring.html)

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | name | 릴리즈 pakage명칭 | O |
  | --dir | 디렉토리에 대한 package 관련 빈 릴리스 구성 파일 생성 | X |

* **사용 예시**

  ```text
    $ bosh vendor-package golang-1.8-linux ~/workspace/golang-release
  ```

### _**bosh create-release**_

* **기본 Syntax**

  ```text
    $ bosh create-release [--force] [--version=ver] [--timestamp-version] [--final] [--tarball=path] [--dir=dir] (Alias: cr)
  ```

* **설명**

  dir에 저장된 릴리스의 새 버전을 생성

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | --force | 릴리스 디렉토리에서 커밋되지 않은 변경 사항을 무시하도록 지정 | X |
  | --version | 사용자 정의 릴리스 버전을 제공 | X |
  | --version | 사용자 정의 릴리스 버전을 제공 | X |
  | --timestamp-version | 타임 스탬프 기반의 dev 릴리즈 버전을 생성 | X |
  | --tarball | 릴리스 타르볼의 대상을 지정 | X |
  | --sha2 | SHA256 체크섬 사용 지정 | X |

* **사용 예시**

  ```text
    $ bosh create-release --force
  ```

### _**bosh finalize-release**_

* **기본 Syntax**

  ```text
    $ bosh finalize-release [path] [--force] [--version=ver] [--dir=dir]
  ```

* **설명**

  선택적으로 주어진 버전으로 최종 릴리스로 릴리스 타볼의 내용을 기록

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | path | 릴리즈 tarball 지정 | O |
  | --force | 릴리스 디렉토리에서 커밋되지 않은 변경 사항을 무시하도록 지정 | X |
  | --version | 사용자 정의 릴리스 버전을 제공 | X |
  | --dir | 디렉토리 위치 지정 | X |

* **사용 예시**

  ```text
    $ cd release-dir
    $ bosh finalize-release /tmp/my-release.tgz
    $ bosh finalize-release /tmp/my-release.tgz --version 20
    $ git commit -am 'Final release 20'
    $ git push origin master
  ```

### _**bosh reset-release**_

* **기본 Syntax**

  ```text
    $ bosh reset-release [--dir=dir]
  ```

* **설명**

  릴리스 디렉토리에 보관 된 dev 릴리스, blob 등의 임시 아티팩트를 제거

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | --dir | 디렉토리 위치 지정 | O |

* **사용 예시**

  ```text
    $ bosh reset-release ~/Download/jenkins
  ```

## BOSH CLI - Release blobs

### _**bosh blob**_

* **기본 Syntax**

  ```text
    $ bosh blobs
  ```

* **설명**

  릴리즈 Blobstore에 등록 한 blob 출력

* **사용 예시**

  ```text
    $ cd release-dir
    $ bosh blobs
    Path                               Size     Blobstore ID         Digest
    golang/go1.6.2.linux-amd64.tar.gz  81 MiB   f1833f76-ad8b-4b...  b8318b0...
    stress/stress-1.0.4.tar.gz         187 KiB  (local)              e1533bc...

    2 blobs

    Succeeded
  ```

### _**bosh add-blob**_

* **기본 Syntax**

  ```text
    $ bosh add-blob [src-path] [dst-path]
  ```

* **설명**

  릴리즈 Blobstore에 로컬 blob 추가

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | src-path | 로컬 Blob 디렉토리 | O |
  | dst-path | 릴리즈 내 blob 디렉토리 | X |

* **사용 예시**

  ```text
    $ cd release-dir
    $ bosh add-blob ~/Downloads/stress-1.0.4.tar.gz stress/stress-1.0.4.tar.gz
  ```

### _**bosh reomove-blob**_

* **기본 Syntax**

  ```text
    $ bosh remove-blob [blob-path]
  ```

* **설명**

  릴리즈 Blobstore에 존재 하는 Blob 삭제

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | blob-path | 릴리즈 내 blob 디렉토리 | O |

* **사용 예시**

  ```text
    $ cd release-dir
    $ bosh remove-blob stress/stress-1.0.4.tar.gz
  ```

### _**bosh sync-blob**_

* **기본 Syntax**

  ```text
    $ bosh sync-blobs
  ```

* **설명**

  릴리즈 내 blobstore의 blob 동기화

* **사용 예시**

  ```text
    $ cd release-dir
    $ bosh sync-blobs
  ```

## BOSH CLI - Releases

### _**bosh releases**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] releases (Alias: rs)
  ```

* **설명**

  업로드 한 릴리즈 조회

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |

* **사용 예시**

  ```text
    $ bosh -e my-env rs
    Using environment '192.168.56.6' as client 'admin'

    Name               Version   Commit Hash
    capi               1.21.0*   716aa812
    cf-mysql           34*       e0508b5
    cf-smoke-tests     11*       a6dad6e
    cflinuxfs2-rootfs  1.52.0*   4827ef51+
    consul             155*      22515a98+
    diego              1.8.1*    0cca668e
    dns                3*        57e27da
    etcd               94*       57c81e16
    garden-runc        1.2.0*    2b3dedc5
    loggregator        78*       773a3ba
    nats               15*       d4dfc4c1+
    routing            0.145.0*  dfb44c41+
    statsd-injector    1.0.20*   552926d
    syslog             9         ac2172f
    uaa                25*       86ec7568

    (*) Currently deployed
    (+) Uncommitted changes

    15 releases

    Succeeded
  ```

### _**bosh upload-release**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] upload-release [location] [--version=ver] [--sha1=digest] [--fix] (Alias: ur)
  ```

* **설명**

  릴리즈 업로드

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | location | 릴리즈 파일 위치 및 URL 지정 | O |
  | --sha1 | 릴리즈 파일 sha1um 값 확인 | X |
  | --fix | 이전에 업로드 한 릴리즈를 동일한 이름과 버전으로 교체 | X |

* **사용 예시**

  ```text
    $ bosh -e my-env ur
    $ bosh -e my-env ur https://bosh.io/d/github.com/concourse/concourse?v=2.7.3
    $ bosh -e my-env ur git+https://github.com/concourse/concourse --version 2.7.3
  ```

### _**bosh delete-release**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] delete-release [name]/[version]
  ```

* **설명**

  업로드 한 릴리즈 삭제

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | name | 삭제 할 릴리즈 명 | O |
  | version | 삭제 할 릴리즈 버전 | O |

* **사용 예시**

  ```text
    $ bosh -e my-env delete-release cf-smoke-tests/94
  ```

### _**bosh export-release**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] -d my-dep export-release [name]/[version] [os]/[version] [--dir=dir]
  ```

* **설명**

  특정 스템셀에 대한 릴리즈를 컴파일 하고 내보낸다

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | name | 릴리즈 명 | O |
  | version | 릴리즈 버전 | O |
  | os | 스템셀 os 명 | O |
  | version | 스템셀 os 버전 | O |
  | dir | 내보내기 디렉토리 | X |

* **사용 예시**

  ```text
    $ bosh -e my-env -d my-dep export-release cf-smoke-tests/94 ubuntu-trusty/3369
  ```

### _**bosh inspect-release**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] inspect-release [name]/[version]
  ```

* **설명**

  모든 Job, Job의 메타데이터 패키지 및 릴리즈 버전과 관련 된 패키지를 출력

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | name | 릴리즈 명 | O |
  | version | 릴리즈 버전 | O |

* **사용 예시**

  ```text
    $ bosh -e gcp-test inspect-release consul/155
    Using environment '192.168.56.6' as client 'admin'

    Job                                                                    Blobstore ID       Digest       Links Consumed    Links Provided
    acceptance-tests/943c6083581e623dc66c7d9126d8e5989c4c2b31              0f3cd013-1d3d-...  17e5e4fc...  -                 -
    consul-test-consumer-windows/6748c0675da2292c680da03e89b738a9d5818370  7461c74c-745d-...  9809861c...  -                 -
    consul-test-consumer/7263db87ba85eaf0dd41bd198359c8597e961839          8bde4572-8e8b-...  7b08b059...  -                 -
    consul_agent/b4872109282347700eaa884dcfe93f3a03dc22dd                  e41f705e-2cb7-...  a8db2c76...  - name: consul    - name: consul
                                                                                                             type: consul      type: consul
                                                                                                             optional: true
    consul_agent_windows/a0b91cb0aa1029734d77fcf064dafdb67f14ada6          3a8755d0-7a39-...  17f07ec0...  - name: consul    - name: consul
                                                                                                             type: consul      type: consul
                                                                                                             optional: true
    fake-dns-server/a1ea5f64de0860512470ace7ce2376aa9470f9b1               5bb53f17-eba9-...  0565f9af...  -                 -

    6 jobs

    Package                                                            Compiled for          Blobstore ID            Digest
    acceptance-tests-windows/e36cef763e5cfd4e28738ad314807e6d1e13b960  (source)              03589024-2596-49fc-...  96eaaf4ba...
    acceptance-tests/9d56ac03d7410dcdfd96a8c96bbc79eb4b53c864          (source)              79fb9ba7-cd23-4b93-...  e08ee88f5...
    confab-windows/52b117effcd95138eca94c789530bcd6499cff9d            (source)              53d4b206-b064-462d-...  43f92c8d0...
    confab/b2ff0bbd68b7d600ecb1ffaf41f59af073e894fd                    (source)              b93214eb-a816-4029-...  4b627d264...
    ~                                                                  ubuntu-        trusty/3363.9  f66fe541-8c21-4fe3-...  8e662c2e2...
    consul-windows/2a8e0b7ce1424d1d5efe5c7184791481a0c26424            (source)              9516870b-801e-42ea-...  19db18127...
    consul/6049d3016cd34ac64ccbf7837b06b6db81942102                    (source)              04aa38af-e883-4842-...  c42cacfc7...
    ~                                                                  ubuntu-trusty/3363.9  ab4afda6-881e-46b1-...  27c1390fa...
    golang1.7-windows/1a80382e081cd429cf518f0c783f4e4172cac79e         (source)              d7670210-7038-4749-...  b91caa06a...
    golang1.7/181f7537c2ec17ac2406d9f2eb3322fd80fa2a1c                 (source)              ac8aa36a-8965-46e9-...  ca440d716...
    ~                                                                  ubuntu-trusty/3363.9  9d40794f-0c50-4d0c-...  9d6e29221...

    11 packages

    Succeeded
  ```

## BOSH CLI - Configs

### _**bosh configs**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] configs [--type=my-type] [--name=my-name]
  ```

* **설명**

  Director의 모든 구성을 출력

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | --name | config 명 기본 Default | X |
  | --type | config type 명 | X |

* **사용 예시**

  ```text
    $ bosh -e my-env configs
    Using environment '192.168.50.6' as client 'admin'

    Type     Name
    cloud    default
    ~        custom-vm-types
    cpi      default
    runtime  default

    3 configs

    Succeeded
  ```

### _**bosh update-config**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] update-config [my-type] [config.yml] [--name=my-name]
  ```

* **설명**

  Director에서 구성을 추가하거나 업데이트

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-typ | config type 명 | O |
  | config.yml | config type의 property Manifest 파일 | O |
  | --name | config 명 기본 Default | X |

* **사용 예시**

  ```text
    $ bosh -e my-env config my-type config.yml
  ```

### _**bosh delete-config**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] delete-config [my-type] [--name=my-name]
  ```

* **설명**

  Director의 my-type 구성을 삭제

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-type | config type 명 | O |
  | --name | config 명 기본 Default | X |

* **사용 예시**

  ```text
    $ bosh -e my-env config my-type config.yml
  ```

## BOSH CLI - Cloud config

### _**bosh  cloud-configs**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] cloud-config (Alias: cc)
  ```

* **설명**

  Deployment Property 설정

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |

* **사용 예시**

  ```text
    $ bosh -e my-env cloud-config
  ```

### _**bosh update-cloud-config**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] update-cloud-config [config.yml] [-v ...] [-o ...] (Alias: ucc)
  ```

* **설명**

  Director의 cloud-conifg 구성 요소를 추가 하거나 수정

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | config.yml | property Manifest 파일 | O |
  | -v | Manifest Replace 변수 ex\) internal\_ip, deployment\_name | X |
  | -o | option Manifest File ex\) jumpbox-user.yml, uaa.yml… | X |

* **사용 예시**

  ```text
    $ bosh -e my-env ucc cc.yml
  ```

## BOSH CLI - Runtime config

### _**bosh runtime-configs**_

* **기본 Syntax**

  ```text
    $ bosh -e my-env runtime-config (Alias: rc)
  ```

* **설명**

  Director의 runtime-conifg 구성 요소를 출력

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |

* **사용 예시**

  ```text
    $ bosh -e my-env runtime-config
  ```

### _**bosh update-runtime-config**_

* **기본 Syntax**

  ```text
    $ bosh -e my-env update-runtime-config [config.yml] [-v ...] [-o ...] (Alias: urc)
  ```

* **설명**

  Director의 cloud-conifg 구성 요소를 추가 하거나 수정

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | config.yml | property Manifest 파일 | O |
  | -v | Manifest Replace 변수 ex\) internal\_ip, deployment\_name | X |
  | -o | option Manifest File ex\) jumpbox-user.yml, uaa.yml… | X |

* **사용 예시**

  ```text
    $ bosh -e my-env urc runtime.yml
  ```

## BOSH CLI - CPI config

### _**bosh cpi-configs**_

* **기본 Syntax**

  ```text
    $ bosh -e my-env cpi-config
  ```

* **설명**

  Director의 cpi-conifg 구성 요소를 출력

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |

* **사용 예시**

  ```text
    $ bosh -e my-env cpi-config
  ```

### _**bosh update-cpi-config**_

* **기본 Syntax**

  ```text
    $ bosh -e my-env update-cpi-config config.yml [-v ...] [-o ...]
  ```

* **설명**

  Director의 cpi-conifg 구성 요소를 추가 하거나 수정

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | config.yml | property Manifest 파일 | O |
  | -v | Manifest Replace 변수 ex\) internal\_ip, deployment\_name | X |
  | -o | option Manifest File ex\) jumpbox-user.yml, uaa.yml… | X |

* **사용 예시**

  ```text
    $ bosh -e my-env update-cpi-config runtime.yml
  ```

## BOSH CLI – Deployments

### _**bosh deployments**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] deployments (Alias: ds)
  ```

* **설명**

  디렉터가 설치 한 전체 배포 목록을 출력.

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |

* **파라미터**

  ```text
    $ bosh -e my-env ds
    Using environment '192.168.56.6' as client 'admin'

    Name                                Release(s)                Stemcell(s)                                         Team(s)  Cloud Config
    cf                                  binary-buildpack/1.0.9    bosh-warden-boshlite-ubuntu-trusty-go_agent/3363.9  -        latest
                                capi/1.21.0
                                cf-mysql/34
                                cf-smoke-tests/11
                                cflinuxfs2-rootfs/1.52.0
                                consul/155
                                diego/1.8.1
                                etcd/94
                                garden-runc/1.2.0
                                loggregator/78
                                nats/15
                                routing/0.145.0
                                statsd-injector/1.0.20
                                uaa/25
    service-instance_0d4140a0-42b7-...  mysql/0.6.0               bosh-warden-boshlite-ubuntu-trusty-go_agent/3363.9  -        latest

    2 deployments

    Succeeded
  ```

### _**bosh deployment**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] -d [my-dep] deployment
  ```

* **설명**

  디렉터가 지정한 이름의 배포 목록 조회

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-dep | 배포 명 지정 | O |

* **사용 예시**

  ```text
    $ bosh -e vbox -d cf dep
    Using environment '192.168.56.6' as client 'admin'

    Name  Release(s)              Stemcell(s)                                         Team(s)  Cloud Config
    cf    binary-buildpack/1.0.9  bosh-warden-boshlite-ubuntu-trusty-go_agent/3363.9  -        latest
      capi/1.21.0
      cf-mysql/34
      cf-smoke-tests/11
      uaa/25
      dns/3
      ...

    1 deployments

    Succeeded
  ```

### _**bosh deploy**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] -d [my-dep] deploy [manifest.yml] [-v ...] [-o ...]
  ```

* **설명**

  디렉터가 지정 한 배포명의 Manifest 파일를 통한 VM 설치

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-dep | 배포 명 | O |
  | -v | Manifest Replace 변수 ex\) internal\_ip, deployment\_name | X |
  | -o | option Manifest File ex\) jumpbox-user.yml, uaa.yml… | X |
  | manifest.yml | 배포 Manifest 파일 | O |

* **사용 예시**

  ```text
    $ bosh -e vbox -d cf deploy cf.yml -v system_domain=sys.example.com -o large-footprint.yml
  ```

### _**bosh delete-deployment**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] -d [my-dep] delete-deployment [--force] (Alias: deld)
  ```

* **설명**

  디렉터가 지정 한 배포명의 VM을 삭제 한다.

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-dep | 배포 명 | O |
  | --force | 다양한 오류 \(IaaS, blobstore, database\)를 무시 지정 | X |

* **사용 예시**

  ```text
    $ bosh -e vbox -d cf deld
  ```

### _**bosh manifest**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] -d [my-dep] manifest (Alias: man)
  ```

* **설명**

  디렉터가 지정 한 배포명의 Manifest 파일을 출력 한다.

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-dep | 배포 명 | O |

* **사용 예시**

  ```text
    $ bosh -e vbox -d cf man > /tmp/manifest.yml
  ```

### _**bosh recreate**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] -d [my-dep] recreate [group[/instance-id]] [--skip-drain] [--fix] [--canaries=] [--max-in-flight=] [--dry-run]
  ```

* **설명**

  디렉터가 지정한 배포의 인스턴스에 대한 VM을 재생성 한다.

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-dep | 배포 명 지정 | O |
  | group\[/instance-id\] | 그룹 또는 그룹과 인스턴스 아이디 | X |
  | --fix | 응답 하지 않는 VM을 대체 | X |
  | --skip-drain | drain scripts를 건너 뛴다. | X |
  | --canaries= | 배포 명 지정 | X |
  | --max-in-flight= | Manifest의 max-in-flight 값을 덮어 쓴다. | X |
  | --dry-run | 배포를 변경 하지 않고 작업을 실행 한다. | X |

* **사용 예시**

  ```text
    $ bosh -e vbox -d cf recreate
    $ bosh -e vbox -d cf recreate --fix
    $ bosh -e vbox -d cf recreate diego-cell
    $ bosh -e vbox -d cf recreate diego-cell/209c42e5-3c1a-432a-8445-ab8d7c9f69b0
    $ bosh -e vbox -d cf recreate diego-cell/209c42e5-3c1a-432a-8445-ab8d7c9f69b0 --skip-drain
    $ bosh -e vbox -d cf recreate diego-cell --canaries=0 --max-in-flight=100%
  ```

### _**bosh restart**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] -d [my-dep] restart [group[/instance-id]] [--skip-drain] [--canaries=] [--max-in-flight=]
  ```

* **설명**

  디렉터가 지정한 배포의 인스턴스에 대한 VM을 재시작 한다.

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-dep | 배포 명 지정 | O |
  | group\[/instance-id\] | 그룹 또는 그룹과 인스턴스 아이디 | X |
  | --skip-drain | drain scripts를 건너 뛴다. | X |
  | --canaries= | 배포 명 지정 | X |
  | --max-in-flight= | Manifest의 max-in-flight 값을 덮어 쓴다. | X |

* **사용 예시**

  ```text
    $ bosh -e my-env -d my-dep restart
  ```

### _**bosh start**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] -d [my-dep] start [group[/instance-id]] [--canaries=] [--max-in-flight=]
  ```

* **설명**

  디렉터가 지정한 배포의 인스턴스에 대한 VM을 시작 한다.

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-dep | 배포 명 지정 | O |
  | group\[/instance-id\] | 그룹 또는 그룹과 인스턴스 아이디 | X |
  | --canaries= | 배포 명 지정 | X |
  | --max-in-flight= | Manifest의 max-in-flight 값을 덮어 쓴다. | X |

* **사용 예시**

  ```text
    $ bosh -e my-env -d my-dep start
  ```

### _**bosh stop**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] -d [my-dep] stop [group[/instance-id]] [--skip-drain] [--canaries=] [--max-in-flight=]
  ```

* **설명**

  디렉터가 지정한 배포의 인스턴스에 대한 VM을 시작 한다.

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-dep | 배포 명 지정 | O |
  | group\[/instance-id\] | 그룹 또는 그룹과 인스턴스 아이디 | X |
  | --canaries= | 배포 명 지정 | X |
  | --max-in-flight= | Manifest의 max-in-flight 값을 덮어 쓴다. | X |
  | --skip-drain | drain scripts를 건너 뛴다. | X |
  | hard | 강제로 VM 삭제, 영구 디스크는 유지 | X |

* **사용 예시**

  ```text
    $ bosh -e my-env -d my-dep stop
  ```

### _**bosh ignore**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] -d [my-dep] ignore group/instance-id
  ```

* **설명**

  bosh deploy와 같은 다른 명령의 영향을 받지 않도록 인스턴스를 무시

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-dep | 배포 명 지정 | O |
  | group\[/instance-id\] | 그룹 또는 그룹과 인스턴스 아이디 | X |

* **사용 예시**

  ```text
    $ bosh -e my-env -d my-dep ignore cell
  ```

### _**bosh unignore**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] -d [my-dep] unignore group/instance-id
  ```

* **설명**

  bosh deploy와 같은 다른 명령의 영향을받지 않도록 인스턴스를 무시하지 않는다

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-dep | 배포 명 지정 | O |
  | group\[/instance-id\] | 그룹 또는 그룹과 인스턴스 아이디 | X |

* **사용 예시**

  ```text
    $ bosh -e my-env -d my-dep unignore cell
  ```

### _**bosh logs**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] -d [my-dep] logs [group[/instance-id]] [--follow] [--num] [--gw-*] [--quiet]
  ```

* **설명**

  하나 이상의 인스턴스에서 로그를 다운로드

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-dep | 배포 명 지정 | O |
  | group\[/instance-id\] | 그룹 또는 그룹과 인스턴스 아이디 | X |
  | --dir | 로그의 디렉토리 지정 | X |
  | --job | 특정 Job의 로그 설정 | X |
  | --only | 로그 필터링 | X |
  | --agent | bosh agent 로그만 포함 | X |
  | --follow | Additional flags for following logs via SSH 로그를 실행 | X |
  | --num | Additional flags for following logs via SSH 마지막 행 수를 출력 | X |
  | --gw | Additional flags for following logs via SSH ssh 게이트웨이 구성 | X |
  | --quiet | Additional flags for following logs via SSH 헤더 출력 생략 | X |

* **사용 예시**

  ```text
    $ bosh -e vbox -d cf logs diego-cell/209c42e5-3c1a-432a-8445-ab8d7c9f69b0
    $ bosh -e vbox -d cf logs diego-cell/209c42e5-3c1a-432a-8445-ab8d7c9f69b0 --job=rep --job=silkd
    $ bosh -e vbox -d cf logs -f
    $ bosh -e vbox -d cf logs -f --num=1000
  ```

## BOSH CLI - VMs

### _**bosh vms**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] -d [my-dep] vms [--vitals]
  ```

* **설명**

  Director 가 관리하는 또는 deployment 의 모든 vm 조회

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-dep | 배포 명 지정 | O |
  | --vitals | RAM CPU disk 와 같은 vm의 기본  정보 조회 | X |

* **사용 예시**

  ```text
    $ bosh -e vbox vms
    $ bosh -e vbox -d cf vms
    $ bosh -e vbox -d cf vms --vitals
  ```

### _**bosh delete-vms**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] -d [my-dep] delete-vm [cid]
  ```

* **설명**

  인스턴스의 Lifecycle을 거치지 않고 VM을 삭제

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-dep | 배포 명 지정 | O |
  | cid | 인스턴스 아이디 지정 | X |

* **사용 예시**

  ```text
    $ bosh -e vbox -d cf delete-vm i-fs384238fjwjf8
  ```

## BOSH CLI - Disks

### _**bosh disks**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] -d [my-dep] disks [--orphaned]
  ```

* **설명**

  Director가 관리하는 또는 deployment의 모든 disk 조회

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-dep | 배포 명 지정 | O |
  | --orphaned | 사용하지 않는 DISK를 나열 | X |

* **사용 예시**

  ```text
    $ bosh -e vnps -d cf disks  
  ```

### _**bosh attach-disk**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] -d [my-dep] attach-disk [group/instance-id] disk-cid
  ```

* **설명**

  인스턴스에 disk attach. 만약 attach된 disk 가 있다면 가장 최근 attach 된 disk를 replace한다.

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-dep | 배포 명 지정 | O |
  | --orphaned | 사용하지 않는 DISK를 나열 | X |
  | group/instance-id | 그룹 또는 그룹과 인스턴스 아이디 | X |

* **사용 예시**

  ```text
    $ bosh -e vbox -d cf attach-disk postgres/209c42e5-3c1a-432a-8445-ab8d7c9f69b0 vol-shw8f293f2f2
  ```

### _**bosh delete-disk**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] -d [my-dep] delete-disk [cid]
  ```

* **설명**

  사용하지 않는 disk 삭제

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-dep | 배포 명 지정 | O |
  | --cid | 삭제 할 DISK 아이디 지정 | X |

* **사용 예시**

  ```text
    $ bosh -e vbox -d cf delete-disk vol-shw8f293f2f2
  ```

## BOSH CLI - SSH

### _**bosh ssh**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] -d [my-dep] ssh [destination] [-r] [-c=cmd] [--opts=opts] [--gw-* ...]
  ```

* **설명**

  인스턴스 한개 또는 여러 개에 SSH설정

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-dep | 배포 명 지정 | O |
  | -c | 커맨드 라인 설정 | X |
  | destination | SSH 목적지 지정 그룹 또는 그룹과 인스턴스 아이디 | X |
  | --opts | ssh에 옵션을 전달 ex\) 포트 포워딩 | X |
  | --gw-\* | SSH 게이트웨이를 구성 | X |
  | -r, --recursive | directory의 반복 복사 허용 | X |

* **사용 예시**

  ```text
    # execute command on all instances in a deployment
    $ bosh -e vbox -d cf ssh -c 'uptime'

    # execute command on one instance group
    $ bosh -e vbox -d cf ssh diego-cell -c 'uptime'

    # execute command on a single instance
    $ bosh -e vbox -d cf ssh diego-cell/209c42e5-3c1a-432a-8445-ab8d7c9f69b0 -c 'uptime'

    # execute command with passwordless sudo
    $ bosh -e vbox -d cf ssh diego-cell -c 'sudo lsof -i|grep LISTEN'

    # present output in a table by instance
    $ bosh -e vbox -d cf ssh -c 'uptime' -r

    # port forward UAA port locally
    $ bosh -e vbox -d cf ssh uaa/0 --opts ' -L 8080:localhost:8080'
  ```

### _**bosh scp**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] -d [my-dep] scp src/dst:[file] src/dst:[file] [-r] [--gw-* ...]
  ```

* **설명**

  인스턴스로 또는 인스턴스로 부터 SCP \(to/from\)설정

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-dep | 배포 명 지정 | O |
  | src/dst: | 복사 받을 script.sh file 경로 | O |
  | src/dst: | 복사 될 script.sh file 경로 | O |
  | -r, --recursive | directory의 반복 복사 허용 | X |
  | --gw-\* | SCP gateway설정 | X |

* **사용 예시**

  ```text
    # copy file from this machine to machines a deployment
    $ bosh -e vbox -d cf scp ~/Downloads/script.sh :/tmp/script.sh
    $ bosh -e vbox -d cf scp ~/Downloads/script.sh diego-cell:/tmp/script.sh
    $ bosh -e vbox -d cf scp ~/Downloads/script.sh diego-cell/209c42e5-3c1a-432a-8445-ab8d7c9f69b0:/tmp/script.sh
    $ bosh -e vbox -d cf scp ~/Downloads/script.ps1 windows_diego_cell:c:/temp/script/script.ps1

    # copy file from remote machines in a deployment to this machine
    $ bosh -e vbox -d cf scp :/tmp/script.sh ~/Downloads/script.sh
    $ bosh -e vbox -d cf scp diego-cell:/tmp/script.sh ~/Downloads/script.sh
    $ bosh -e vbox -d cf scp diego-cell/209c42e5-3c1a-432a-8445-ab8d7c9f69b0:/tmp/script.sh ~/Downloads/script.sh
    $ bosh -e vbox -d cf scp windows_diego_cell:c:/temp/script/script.ps1:~/Downloads/script.ps1

    # copy files from each instance into instance specific local directory
    $ bosh -e vbox -d cf scp diego-cell:/tmp/logs/ /tmp/logs/((instance_id))
  ```

## BOSH CLI - Errands

### _**bosh errands**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] -d [my-dep] errands (Alias:es)
  ```

* **설명**

  deployment로 정의 된 모든 errand 목록 조회

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-dep | 배포 명 지정 | O |

* **사용 예시**

  ```text
    $ bosh -e vbox -d cf es
    Using environment '192.168.56.6' as '?'

    Using deployment 'cf'

    Name
    smoke-tests

    1 errands

    Succeeded
  ```

### _**bosh run-errand**_

* **기본 Syntax**

  ```text
    $ bosh -e <my-env> -d <my-dep> run-errand <name> [--keep-alive] [--when-changed] [--download-logs] [--logs-dir=<dir>] [--instance=<instance-group/instance-id>]
  ```

* **설명**

  errand job 을 name 단위로 실행

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-dep | 배포 명 지정 | O |
  | name | 실행 할 errand 이름 | O |
  | --keep-alive | errand가 실행 되는 곳에서 VM 유지 | X |
  | --when-changed | errand가 skip 설정: 이전에 이미 실행하였고 \(성공적으로 마침\) errand job 설정값이 바뀌지 않았을 경우 | X |
  | --download-logs | errand log를 통채로 --logs-dir에 명시된 경로에 저장 | X |
  | --logs-dir= | errand log를 저장 할 파일 경로 | X |
  | instance=&lt;instance-group/instance-id&gt; \(v2.0.31+\) | errand를 실행하기위해 어떤 인스턴스를 사용할지 결정 | X |

* **사용 예시**

  ```text
    $ bosh -e vbox -d cf run-errand smoke-tests
    $ bosh -e vbox -d cf run-errand smoke-tests --keep-alive
    $ bosh -e vbox -d cf run-errand smoke-tests --when-changed

    # execute errand on all instances that have colocated status errand
    $ bosh -e vbox -d zookeeper run-errand status

    # execute errand on one instance
    $ bosh -e vbox -d zookeeper run-errand status --instance zookeeper/3e977542-d53e-4630-bc40-72011f853cb5

    # execute errand on one instance within an instance group
    # (note that select instance may not necessarily be first based on its index)
    $ bosh -e vbox -d zookeeper run-errand status --instance zookeeper/first

    # execute errand on all instance in an instance group
    $ bosh -e vbox -d zookeeper run-errand status --instance zookeeper

    # execute errand on two instances
    $ bosh -e vbox -d zookeeper run-errand status \
          --instance zookeeper/671d5b1d-0310-4735-8f58-182fdad0e8bc \
          --instance zookeeper/3e977542-d53e-4630-bc40-72011f853cb5
  ```

## BOSH CLI - Tasks

### _**bosh tasks**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] tasks [--recent[=num]] [--all] (Alias: ts)
  ```

* **설명**

  활성 및 이전에 실행 한 작업에 대한 task를 출력

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | --recent | 최근 순 4개 task 조회 | X |
  | num | 최근 순 조회하고 싶은 task 숫자 | X |
  | --all | active tasks 모두 조회 | X |
  | -d, -deployment | deployment 단위로 필터링 해서 조회 | X |

* **사용 예시**

  ```text
    # currently active tasks
    $ bosh -e vbox ts

    # currently active tasks for my-dep deployment
    $ bosh -e vbox -d my-dep ts
    Using environment '192.168.56.6' as '?'

    #   State  Started At                    Last Activity At              User   Deployment   Description                   Result

    27  done   Thu Feb 16 19:16:15 UTC 2017  Thu Feb 16 19:20:33 UTC 2017  admin  cockroachdb  create deployment             /deployments/cockroachdb
    26  done   Thu Feb 16 18:54:32 UTC 2017  Thu Feb 16 18:55:27 UTC 2017  admin  cockroachdb  delete deployment cockroachd  /deployments/cockroachdb
    ...

    110 tasks

    Succeeded

    # show last 30 tasks
    $ bosh -e vbox ts -r --all

    # show last 1000 tasks
    $ bosh -e vbox ts -r=1000
  ```

### _**bosh task**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] task [id] [--debug] [--result] [--event] [--cpi] (Alias: t)
  ```

* **설명**

  task 아이디를 기준으로 상세 조회.

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | id | task 아이디 | O |
  | --debug | Debug 로그 출력 | X |
  | --result | Result 로그 출력 | X |
  | --event | Event 로그 출력 | X |
  | --cpi | CPI 로그 출력 | X |

* **사용 예시**

  ```text
    $ bosh -e vbox t 281
    $ bosh -e vbox t 281 --debug
  ```

### _**bosh cancle-task**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] cancel-task [id] (Alias: ct)
  ```

* **설명**

  task 취소. 다음 checkpoint 에서 task를 취소 한다. task가 취소될 때까지 대기하지 않는다

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | id | task 아이디 | O |

## BOSH CLI - Snapshot

### _**bosh snapshots**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] -d [my-dep] snapshots
  ```

* **설명**

  deployment의 스냅샷 목록을 출력 한다.

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-dep | 배포 명 지정 | O |

* **사용 예시**

  ```text
    $ bosh -e my-env -d my-dep snapshots
  ```

### _**bosh take-snapshot**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] -d [my-dep] take-snapshot [group/instance-id]
  ```

* **설명**

  특정 인스턴스 또는 deployment에 대한 스냅샷을 생성 한다.

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-dep | 배포 명 지정 | O |
  | group/instance-id | 그룹 또는 그룹과 인스턴스 아이디 | X |

* **사용 예시**

  ```text
    $ bosh -e my-env -d my-dep take-snapshot cell
  ```

### _**bosh delete-snapshot**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] -d [my-dep] delete-snapshot [cid]
  ```

* **설명**

  특정 스냅샷을 삭제 한다.

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-dep | 배포 명 지정 | O |
  | cid | 스냅샷 아이디 지정 | O |

* **사용 예시**

  ```text
    $ bosh -e my-env -d my-dep delete-snapshot 1acsda-ccas
  ```

### _**bosh delete-snapshots**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] -d [my-dep] delete-snapshots
  ```

* **설명**

  스냅샷을 전체 삭제 한다.

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-dep | 배포 명 지정 | O |

* **사용 예시**

  ```text
    $ bosh -e my-env -d my-dep delete-snapshots
  ```

## BOSH CLI - Deployment recovery

### _**bosh update-resurrection**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] update-resurrection [on/off]
  ```

* **설명**

  디렉터가 지정한 환경에 대해 recovery를 활성/비활성화

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | on/off | 활성/비활성화 | O |

* **사용 예시**

  ```text
    $ bosh -e my-env update-resurrection on
  ```

### _**bosh cloud-check**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] -d [my-dep] cloud-check [--report] [--auto] (Alias: cck)
  ```

* **설명**

  리소스에 대해 일관적인 검사를 하고 대화 형 복구를 허용 한다.

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | my-dep | 배포 명 지정 | O |
  | --report | Report 생성 | X |
  | --auto | 자동으로 Problem해결 | X |

* **사용 예시**

  ```text
    $ bosh -e vbox -d cf cloud-check --report --auto
  ```

### _**bosh locks**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] locks
  ```

* **설명**

  최근 lock 목록 조회

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |

* **사용 예시**

  ```text
    $ bosh -e my-env locks
  ```

## BOSH CLI - Misc

### _**bosh clean-up**_

* **기본 Syntax**

  ```text
    $ bosh -e [my-env] clean-up [--all]
  ```

* **설명**

  releases, stemcells, orphaned disks 그리고 사용되지 않는 다른 리소스를 clean up 한다

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | my-env | 지정 한 Director 환경 이름 명칭 | O |
  | --all | orphaned disks에 강제 clean up적용 | X |

* **사용 예시**

  ```text
    $ bosh -e my-env clean-up --all
  ```

### _**bosh help**_

* **기본 Syntax**

  ```text
    $ bosh help
  ```

* **설명**

  사용 가능한 모든 명령어와 global option 목록 조회 각각의 command에 대해서는 -h 사용

* **파라미터**

  ```text
    없음
  ```

* **사용 예시**

  ```text
    $ bosh help

    $ bosh upload-release -h
  ```

### _**bosh interpolate**_

* **기본 Syntax**

  ```text
    $ bosh interpolate manifest.yml [-v ...] [-o ...] [--vars-store path] [--path op-path] (Alias: int)
  ```

* **설명**

  결과 값 이 sudout로 넘겨지는 Manifest.yml에 추가적으로 merge할 yml 파일이나 설정 값을 입력

* **파라미터**

  | **파라미터 명** | **설명** | **필수\*\***\(O/X\)\*\* |
  | :--- | :--- | :--- |
  | -v | 수정/입력 하는 variable list | X |
  | -o | 수정/입력 하는 operation file list | X |
  | --vars-store path | 디렉터 접근 아이디 Key 및 각 JOB 패스워드 등이 존재하는 설정 파일 생성 위치 | X |
  | --path op-path | Manifest의 해당 값 출력 | X |

* **사용 예시**

  ```text
    $ bosh int bosh-deployment/bosh.yml \
          --vars-store ./creds.yml \
          -o bosh-deployment/virtualbox/cpi.yml \
          -o bosh-deployment/virtualbox/outbound-network.yml \
          -o bosh-deployment/bosh-lite.yml \
          -o bosh-deployment/jumpbox-user.yml \
          -v director_name=vbox \
          -v internal_ip=192.168.56.6 \
          -v internal_gw=192.168.56.1 \
          -v internal_cidr=192.168.56.0/24 \
          -v network_name=vboxnet0 \
          -v outbound_network_name=NatNetwork

    $ bosh int creds.yml --path /admin_password
    skh32i7rdfji4387hg

    $ bosh int creds.yml --path /director_ssl/ca
    -----BEGIN CERTIFICATE-----
  ```

