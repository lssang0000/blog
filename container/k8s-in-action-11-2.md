# Chpater11. Understanding Kubernetes internals (2)

<br/>

## 학습 내용

- 11.2 How controllers cooperate  (Pod 생성 원리)
- 11.3 Understanding what a running pod is (실행 중인 Pod)
- 11.4 Inter-pod networking (Pod 간 통신)
- 11.5 How services are implemented (Svc를 이용한 통신)
- 11.6 Running highly available clusters (고 가용성)

<br/>

## 학습 목표

- API 서버, Scheduler, 다양한 controller들이 Controller Manager와 Kubelet에서 함께 작동하여 포드를 생성하는 방법 (11.2)
- 인프라 컨테이너가 포드의 모든 컨테이너를 하나로 묶는 방법 (11.3)
- Network bridge를 통해 동일한 노드에서 실행되는 다른 포드와 포드가 통신하는 방법 (11.4)
- 다른 노드에서 실행되는 포드가 서로 통신 할 수 있는 방법 (11.4)
- kube-proxy가 노드에서 iptables 규칙을 구성하여 동일한 서비스에서 포드 간에 로드 밸런싱을 수행하는 방법 (11.5)
- 클러스터의 가용성을 높이기 위해 Control Plane의 각 컴포넌트 인스턴스를 여러 개 실행하는 방법 (11.6)

<br/>

## Reference
[1] [Kubernets in Action](https://books.google.co.kr/books?id=8bE5MQAACAAJ&dq=kubernetes+in+action&hl=ko&sa=X&ved=2ahUKEwjU4pfAw6_tAhUmGaYKHe16AyMQ6AEwAHoECAMQAg)


## [**Back to Blog Home**](../README.md)