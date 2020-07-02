# How to Istio

 - [Istio](https://istio.io/)
   - [Docs](https://istio.io/latest/docs/)

## Content

1. [설치](docs/01.install.md)
1. [인그레스](docs/02.ingress.md)

---

## Istio

클라우드 플랫폼 위에 MSA를 구축할 때, 운영과 개발이 복잡해진다.  
Istio는 분산 마이크로 서비스를 관리하는 Service Mesh다.

### Service mesh

애플리케이션 앞에 프록시를 배치해 네트워크를 처리하는 설계 구조.

마이크로 서비스마다 붙어있는 프록시를 이용해서 복잡한 네트워크 기능을 쉽게 처리할 수 있다.

#### MSA 단점

1. 아키텍처가 커지면서 서비스간 통신은 복잡해진다.
1. 많은 인스턴스를 로깅, 모니터링, 관리해야 한다.

#### Service mesh 장점

1. Control plane: OSI 모델 7계층 네트워크 인프라를 중앙에서 관리
1. Sidecar: 애플리케이션 비즈니스 로직과 네트워크 로직 분리

#### Service mesh 단점

1. 런타임 인스턴스 수 x2 이상 증가
1. 서비스 간 통신에 네트워크 레이어 추가: 오버헤드

#### 기능

- Traffic management: 서비스 디스커버리, 로드 밸런싱, 장애복구, 라우팅, 서킷브레이킹 등
- Security: 인증, 권한부여, 암호화 등
- Observability: 추적, 모니터링, 로깅

#### 종류

1. PaaS: Mesh-native Code. 서비스 메시 구현 특화 코드 필요
   - Azure Service fabric, lagom, SENECA
1. 라이브러리: Mesh Aware Code. 서비스 메시를 이해한 코드 작성 필요
   - Spring Cloud, Netflix OSS(Ribbon/Hystrix/Eureka/Archaius), finagle.
   - Prana(sidecar 형태)
1. **Sidecar proxy**
   - **Istio/Envoy**, Consul, Linkerd

#### 글

- **[Service Mesh 란?](https://medium.com/dtevangelist/service-mesh-%EB%9E%80-8dfafb56fc07)**: 깔끔한 자세한 설명
- [서비스 메시의 의미와 서비스 메시 프로젝트들](http://www.itworld.co.kr/t/62076/%EA%B0%80%EC%83%81%ED%99%94/124515): 쉬운 설명
- [MSA 제대로 이해하기 -(4)Service Mesh](https://velog.io/@tedigom/MSA-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-4Service-Mesh-f8k317qn1b): 간단한 설명
- [Service Mesh Comparison](https://servicemesh.es/)
- [What Is a Service Mesh?](https://www.nginx.com/blog/what-is-a-service-mesh/)

### 아키텍처

[Architecture](https://istio.io/latest/docs/ops/deployment/architecture/)

![](https://istio.io/latest/docs/ops/deployment/architecture/arch.svg)

- data plane: Envoy 프록시 사용하여 sidecar로 배포. 모든 네트워크 통신을 처리한다.
- control plane: 프록시 설정 관리

#### 구성 요소

- Envoy: C++로 개발된 고성능 프록시
  - 모든 트래픽을 처리한다.
- Istiod: 서비스 디스커버리, 설정, 인증 관리. 
  - Envoy의 설정값으로 바꿔준다.
