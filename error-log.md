# ERROR LOG
> 환경을 구축하면서 오류가 있었다면 기록을 한 내용입니다.

## GKE 클러스터 생성 중 오류
```shell
ERROR: (gcloud.container.clusters.create) ResponseError: code=400, message=Failed precondition when calling the ServiceConsumerManager: tenantmanager::185014: Consumer 324714949785 should enable service:container.googleapis.com before generating a service account.
com.google.api.tenant.error.TenantManagerException: Consumer 324714949785 should enable service:container.googleapis.com before generating a service account.
```
위와 같은 오류 사항이 발생하여서 개발자의 아주 친한 친구인 구글을 사용하여서 검색을 해 보았다.  

[ERROR: (gcloud.container.clusters.create) ResponseError: code=400, message=Failed](https://stackoverflow.com/questions/64537546/error-gcloud-container-clusters-create-responseerror-code-400-message-faile)를 참고하여서 해결 하였다.
### 해결 방안
```shell
$ gcloud services enable container.googleapis.com
# 위 명령어를 실행하려고 하였지만 오류가 있어서
$ gcloud components update
# componetes update를 하라고 하여서 진행함.
```

## 지원하지 않은 버전을 사용할려고 하였을 때
```shell
위의 문제를 해결하였더니 다른 문제가 발생하였다.
gcloud container clusters create k8s \
--cluster-version 1.18.16-gke.2100 \
--zone asia-northeast3-a \
--num-nodes 3
```
위 명령어를 사용하여서 클러스터를 생성하려고 진행하였지만 클러스터 버전이 낮은 바람에 실행이 되지 않았다.
```shell
ERROR: (gcloud.container.clusters.create) ResponseError: code=400, message=Master version "1.18.16-gke.2100" is unsupported.
```
그래서 지원하는 클러스터 버전을 찾아 보았다.  
[Google Cloud Kubernetes Engine](https://cloud.google.com/kubernetes-engine/docs/release-notes)에서 확인할 수 있었다.

```shell
$ gcloud container clusters create k8s \
--cluster-version 1.18.20-gke.900 \
--zone asia-northeast3-a \
--num-nodes 3
```
버전을 제대로 확인하여서 해 주었더니 정상적으로 실행 되게 됩니다!