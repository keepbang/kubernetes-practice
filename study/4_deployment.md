# Deloyment

- 배포에 최적화된 오브젝트로써 업데이트가 용이하며 유지보수가 쉽다.
- revision이 저장되어 롤백이 쉽다.

## Deployment vs Replicaset 차이점
```
Replicaset의 경우 업데이트를 위해서 yml을 수정하고 다시 apply해야 하며 개수 확인만 하기 때문에 재시작을 안해 기존에 떠있는 파드는 수정된 환경으로 반영이 안된다.
Replicaset에서 업데이트를 하기 위해서는 기존 떠있던 파드를 삭제해야 업데이트된 내용으로 파드가 다시 올라간다.
Deployment는 자동으로 업데이트 된다.
```

-----

- 파드와 레플리카셋에 대한 선언적 업데이트 가능.
  - 즉, 레플리카셋의 한계를 극복
-  단, 파드 템플릿(.spec.templete)이 변경된 경우에만 롤아웃이 실행됨.
  - 예를 들어, 템플릿의 레이블이나 컨테이너 이미지가 업데이트된 경우.
  - 디플로이먼트의 스케일링과 같은 다른 업데이트는 롤아웃을 트리거하면 안됨

-----

## 실습
```
# nginx-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-nginx
  template:
    metadata:
      name: my-nginx-pod
      labels:
        app: my-nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

#### deployment apply
```
# k apply -f nginx-deployment.yaml
```
<img width="396" alt="image" src="https://github.com/keepbang/kubernetes-practice/assets/38740455/0dd0e9e2-7029-4540-8b5d-a0d448dc5ef1">
<img width="496" alt="image" src="https://github.com/keepbang/kubernetes-practice/assets/38740455/f9e5c256-ac43-41ad-8041-65918fbcc041">

#### pod 개수 변경
```
k scale deployment my-nginx-deployment --replicas 5
```
<img width="532" alt="image" src="https://github.com/keepbang/kubernetes-practice/assets/38740455/29c63b6f-24bc-450e-8629-79e417d5c90b">
- pod 개수가 5개로 늘어남.

## 업데이트 전략
- rolling update : 순처적으로 실행하며, maxUnavailable, maxSurge 옵션을 통해 조정이 가능하다.
  (리소스를 택해서 하는것도 가능함 3개를 먼저 띄우거나, 리소스의 30%를 죽이거나, 살리거나 등)
- recreate : 실행중인 pod를 동시에 죽인뒤 동시에 실행한다.

## Rollout

deployment로 업데이트가 될 경우 rollout에 기록이 남게되며, 해당 rollout으로 롤백, 버전관리 등을 수행 할 수 있게 된다.

### 조회

```yaml
kubectl rollout history deployment my-nginx-deployment
```

- 특정 revision 상세 조회
    
    ```yaml
    kubectl rollout history deployment my-nginx-deployment --revision 3
    ```

## Revision
- `deployment` 롤아웃 기록에 기반하여 버전관리 기능을 제공.
- 새로운 배포 버전이 문제가 있을 경우, 자동으로 이전 버전으로 롤백.
  
```
kubectl rollout history deployment my-nginx-deployment
```
```
#REVISION  CHANGE-CAUSE
#1         <none>
#2         <none>
#3         <none>
```
- `CHANGE_CAUSE`는 `kubectl annotate` 명령어로 따로 현재 버전에 대해 주석을 남겨주어야함!
```
kubectl annotate deployments.apps my-nginx-deployment kubernetes.io/change-cause="nginx version updated to 1.25.0"
```
- `.spec.revisionHistoryLimit` 필드로 Revision 개수를 지정할 수 있다. `default = 10`
