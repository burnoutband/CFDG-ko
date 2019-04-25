# Cloud Foundry The Definitive Guide - *Translation to Korean*

## Ch 1 - The Cloud-Native Platform
## Ch 2 - Concepts
## Ch 3 - Components

### User Management and UAA

### The Cloud Controller

#### System State

#### The Cloud Controller Blobstore

### The CCDB

### The Application Life-Cycle Policy

### Continuous Delivery Pipelines
- Concourse.ci

###  Application Execution
- application 실행 과 task 실행을 담당하는 구성요소 = Diego, Garden , runC

###  Diego
- the Container runtime architecture for CF
- provides the scheduling, orchestration, and placement of application and tasks

###  Garden and runC
1. Garden (a container management API: Go로 작성됨)
2. runC (OCI compatible backend container implementation)
    - Docker 같은 container runtime 임
    - OCI: Open Container Initiative: 
        + https://github.com/opencontainers/runtime-spec/blob/master/runtime.md

### Metrics and Logging
### Metron Agent
- Cell로부터 application logs 를 모은다 (gathering)
    * Cell = CF Diego host
- application logs 와 component metrics를 Loggregator subsystem으로 포워딩한다 (forwarding)

### Loggregator (log aggregator)
- Firehose
    1. 파이어호스를 통해서 application logs, container metrics (memory, CPU, and disk-per-app instance), component counter/HTTP events 에 접근할 수 있음 (component logs 는 제공안함)
    2. Component logs는 rsyslog drain 을 통해서 검색할 수 있음

### Messaging
### Additional Components
#### Stacks
- prebuilt root filesystem (rootfs)
- 스택은 droplet과 함께 사용된다 (droplet: the output of buildpack staging)
- 스택은 애플리케이션 실행을 위해 사용되는 container filesystem을 제공함

#### A marketplace of On-Demand Services
- CF는 마켓플레이스 개념을 가지고 있다
- 애플리케이션은 자주 외부 서비스에 의지한다 (databases, caches, messaging engines, third-party APIs)
- CF marketplace는 플랫폼 확장 포인트이다
    1. 개발자들은 실행중인 애플리케이션을 지원하기 위해 서비스들을 마켓플레이스에서 사용할 수 있다
- 플랫폼 운영자는 service brokers, route services user-provided services 를 통해서 추가적인 서비스들을 마켓플레이스에 추가할 수 있다
    1. 마켓플레이스는 CF 사용자에게 셀프서비스, 추가 서비스 인스턴스의 주문형 프로비저닝을 제공한다
    2. 서비스 개발자는 마켓플레이스에 플랫폼에서 실행할 수 있는 어떤 애플리케이션을 서비스 형태로 노출할 수 있다
- Service brokers
    1. 개발자들은 Service Instance를 프로비전할 수 있다. 그리고 Service Broker를 통해서 그 service instance를 애플리케이션에 바인딩할 수 있다
    2. 서비스 브로커는 CF 사용자에게 다음과 같은 것을 제공하기 위해 CAPI (CF API) 를 구현한다 (자기 서비스를 제공하고 싶은 사람이 해야 할 일)
        - List Service Offerings
        - Provision (create) and deprovision (delete) service instance
        - Enable applications to bind to, and ubind from, the service instances
    3. Provision 은 서비스 리소스를 예약하는 것. Bind 는 리소스접근에 필요한 정보를 애플리케이션에 전달하는 것
        1. Reserved resource 는 한마디로 service instance 이다
    4. The key concern is that the broker implements the required API to interact with the Cloud Controller
- User-provided services
    1. Service Broker 방식이외에도, 플랫폼 운영자는 기존 서비스들을 User-provided services 를 통해서 노출할 수 있음. customer database 를 cf application과 binding 할 수 있다는 의미

#### Buildpacks and Docker Images
1. cf push 배포 할 수 있는 두가지 형태
    * A standalone application
        + war/jar file or raw source code (link to a git remote)  --> 이걸 buildpack 과정이 droplet으로 만든다 (컨테이너이미지)
    * A prebuilt Docker image (that could contain additional runtime and middleware dependencies)
2. standalone application 을 cf push 하면, buildpack process 가 다음과 같은 일을 수행한다:
    * The detection of an application framework
    * The application compilation (known in cf terminology as Staging)
    * Running the application
3. 요약
    1. Buildpacks 은 개발자의 application artifact 를 가져와서 컨테이너 이미지화 시킨다. 컨테이너 이미지화 된 그것을 우리는 *__droplet__* 이라고 부른다
    2. 결국 application 을 위한 Delivery 체계는 Containerization 이다

#### Infrastructure and Cloud Provider Interface
* **CF 설치 전에 셋팅 해야하는 내용**
    > + Networks and subnets (typically a /22 private network)
    > + VMs with specified CPU and memory requirements
    > + Storage for VMs
    > + File server or blobstore
    > + DNS, certificates, and wildcard domains
    > + Load balancer to pass traffic into the GoRouter
    > + NAT for traffic flowing back to the load balancer

* CF는 CPI (Cloud Provider Interface)를 가지고 인프라스트럭처-특수구현부분을 추상화 한다.

### The Cloud Foundry Github Repository
* https://github.com/cloudfoundry/cf-deployment  


### Summary
* __Cloud Foundry Component layers__  
<img src="../images/3-1.CF_Compoonent_layers.png" width="500">  
 
* __Routing__ : GoRouter, TCPRouter, and external load balancer
* __Authentication__ and user management :  User Access and Authentication Management
* __Application__ life cycle and system state : Cloud Controller, Diego's core components (e.g., BBS and Brain)
* __App__ storage and execution : blobstore (including app artifacts/droplets and the Application Life-Cycle Binaries), Diego Cell (Garden, and runC)
* __Services__ : Service Brokers, User Provided Service)
* __Messaging__ : NATS (Network Address Translation) Messaging Bus
* __Metrics__ and logging : Loggregator (including Doppler and the Firehose)

## Ch 4 - Preparing Your Cloud Foundry Environment
## Ch5 - Installing and Configuring Cloud Foundry
## Ch 6 - Diego
## Ch 7 - Routing Considerations
## Ch 8 - Containers, Containers, Containers
## Ch 9 - Buildpacks and Docker
## Ch 10 - BOSH Concepts
## Ch 11 - BOSH Releases
## Ch 12 - BOSH Deployments
## Ch 13 - BOSH Components and Commands
## Ch 14 - Debugging Cloud Foundry
## Ch 15 - User Account and Authentication Management
## Ch 16 - Designing for Resilience, Planning for Disaster
## Ch 17 - Cloud Foundry Roadmap

