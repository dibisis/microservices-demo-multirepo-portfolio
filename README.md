# microservices-demo-multirepo-portfolio

* [MSA 데모](https://github.com/dibisis/microservices-demo)를 기반으로 AWS EKS환경으로 배포 자동화 
* Tech : K8S(EKS), MSA, gRPC, CI/CD(CircleCI), Cloudflare, Locust

# 기존 MSA demo에서 변경점
* monorepo => multirepo
* CI/CD 추가(CircleCI)
* ELB의 CNAME을 지정한 도메인으로 연결(Cloudflare API, CircleCI)
* GKE => AWS EKS
* Jaeger support based OpenCensus


## Service Architecture

**Online Boutique** 는 마이크로서비스간 통신에 gRPC를 사용하는 MSA demo project

[![Architecture of
microservices](./docs/img/architecture-diagram.png)](./docs/img/architecture-diagram.png)

gRPC의 Protocol Buffer Description은 각 repo의 ./pb 

| Service                                              | Language      | Description                                                                                                                       |
| ---------------------------------------------------- | ------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| [frontend](https://github.com/dibisis/microservices-demo-frontend/)                           | Go            |  |
| [cartservice](https://github.com/dibisis/microservices-demo-cartservice)                     | C#            |     |
| [productcatalogservice](https://github.com/dibisis/microservices-demo-productcatalogservice) | Go            | |
| [currencyservice](https://github.com/dibisis/microservices-demo-currencyservice)             | Node.js       | |
| [paymentservice](https://github.com/dibisis/microservices-demo-paymentservice)               | Node.js       | card info(mock) |
| [shippingservice](https://github.com/dibisis/microservices-demo-shippingservice)             | Go            | mock |
| [emailservice](https://github.com/dibisis/microservices-demo-emailservice)                   | Python        | mock |
| [checkoutservice](https://github.com/dibisis/microservices-demo-checkoutservice)             | Go            | |
| [recommendationservice](https://github.com/dibisis/microservices-demo-recommendationservice) | Python        | |
| [adservice](https://github.com/dibisis/microservices-demo-adservice)                         | Java          | |
| [loadgenerator](https://github.com/dibisis/microservices-demo-loadgenerator)                 | Python/Locust |  |
## Screenshots

| Home Page                                                                                                         | Checkout Screen                                                                                                    |
| ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| [![Screenshot of store homepage](./docs/img/online-boutique-frontend-1.png)](./docs/img/online-boutique-frontend-1.png) | [![Screenshot of checkout screen](./docs/img/online-boutique-frontend-2.png)](./docs/img/online-boutique-frontend-2.png) |
## Tech Stack

- **[Kubernetes](https://kubernetes.io)/AWS EKS**
- **[gRPC](https://grpc.io)** 
- **[Istio](https://istio.io):** service mesh
- **[OpenCensus](https://opencensus.io/):** Tracing
- **[Skaffold](https://skaffold.dev):** Kubernetes 개발자도구
- **[Locust](https://locust.io/):** load generator

## 사전 준비
### AWS EKS provisioning with budget config
```
# spot instance 를 이용한 EKS 인프라 설정
eksctl create cluster -f eksctl-spot.yaml
```

```
# redis 설치
kubectl apply -f redis.yaml
```

## CircleCI 환경변수
### context
* dockerhub
  * $DOCKERHUB_PASS
  * $DOCKERHUB_USERNAME
* awsauthdibisis # 사용자의 aws auth 정보
  * $AWS_ACCESS_KEY_ID
  * $AWS_DEFAULT_REGION
  * $AWS_SECRET_ACCESS_KEY
  
## Jaeger 설정
```
# simplest.yaml
apiVersion: jaegertracing.io/v1
kind: Jaeger
metadata:
  name: simplest
```

~~~
$ kubectl apply -f simplest.yaml
~~~
