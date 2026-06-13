# MSA
- MicroService Architecture

- 작고 독립적으로 배포 가능한 각각의 기능을 수행하는 서비스로 구성된 프레임워크 

## 등장 배경
- Monolithic Architecture의 한계 보안을 위해 등장

- 물리적인 서버를 사용하는 방식(Monolithic Architecture) on-promise(IT 인프라를 클라우드가 아닌 사내 전산실이나 자체 데이터센터에 직접 구축하고 운영하는 방식)가 아닌 클라우드 환경으로 서버를 구성하는 방식(MSA)으로 바뀜

### Monolithic Architecture
- 소프트웨어의 모든 구성요소가 한 프로젝트에 통합되어 있는 형태

- 예시로 개발을 위해 모듈별로 개발하고 완료된것을 웹 하나로 배포

- 주로 소규모 프로젝트에 활용

#### 한계점
- 부분 장애가 서비스 전체로 확대될 수 있음

- 부분적인 Scale-out(여러 서버로 나누어 처리하는 방식)이 어렵다