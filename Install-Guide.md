# 각종 툴 및 패키지 설치 및 설정
> Kubernetes를 사용하기 위해서 사용 해야하는 툴 및 패키지와 설정에 대해서 기록합니다.

## HyperKit & Minikube
```shell
$ brew update
# Install HyperKit
$ brew install hyperkit
# Install Minikube
$ brew install minikube
$ minikube version
```

## HyperKit & Minikube 설정
```shell
# 미니큐브를 사용하여서 쿠버네티스 v1.21.3(=>v1.22.0) 환경을 구축하고 기동
$ minikube start driver=hyperkit --kubernetes-version v1.21.3 
```
![Set-UP Minikube](./images/minikube-setup.png)

```shell
# 미니큐브 클러스터 상태 확인
$ kubectl status

# 컨텍스트 전환
$ kubectl config use-context minikube

# 노드 확인
$ kubectl get nodes

# 미니큐브 클러스터 정지
$ minikube delete
```

## Docker Desktop for Mac/Windows
1. Docker Desktop을 설치 후 
2. 설정에서 Kubernetes 목록에서 `Enable Kubernetes`를 체크 해 주세요!

```shell
# 컨텍스트 변경
$ kubectl config use-context docker-desktop

# 노드 확인
$ kubectl get nodes

# 기동 중인 쿠버네티스 관련 구성 요소 확인
$ docker container ls --format 'table {{.Image}}\t{{.Command}}' | grep -v pause
```

## Kind 설치
```shell
# kind 설치
$ brew install kind

# kind 버전 확인
$ kind version
```

### [kind 클러스터 설정 예제](./sample/chapter03/kind.yaml)
```yaml
apiVersion: kind.x-k8s.io/v1alpha4
kind: Cluster
nodes: 
- role: control-plane
  image: kindest/node:v1.18.15
- role: control-plane
  image: kindest/node:v1.18.15
- role: control-plane
  image: kindest/node:v1.18.15
- role: worker
  image: kindest/node:v1.18.15
- role: worker
  image: kindest/node:v1.18.15
- role: worker
  image: kindest/node:v1.18.15
```
```shell
# kind에서 쿠버네티스 클러스터 기동
$ kind create cluster --config kind.yaml --name kindcluster
```

**오류 발생:** 오류 발생 이유는 맥의 메모리가 부족해서 오류가 발생합니다ㅠㅠ  
[**ERROR: failed to create cluster: failed to init node with kubeadm #1437**](https://github.com/kubernetes-sigs/kind/issues/1437)

![Kind Create](./images/kind-create.jpg)

```shell
# 컨텍스트 전환
$ kubectl config use-context kind-kindcluster

# 노드 확인
$ kubectl get nodes

# 도커 컨테이너 확인 (일부 발췌)
$ docker container ls

# kind 클러스터 삭제
$kind delete cluster --name kind-kin
```