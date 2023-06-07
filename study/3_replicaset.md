# Pod

pod의 문제
- down되었을 경우
- replication을 유동적으로 할 수 없음
  - 이름이 겹치지 않게 여러개를 나눠서 띄워야한다.
  - 로드밸런싱도 따로 해줘야함

# ReplicationController(deprecated)
# ReplicaSet 사용
- 유동적인 selector
- 유동적으로 pod를 늘릴수 있다.

[replicaset_nginx.yaml]
```shell
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: replicaset-nginx
spec:
  replicas: 3
  selector: # Definition Replicaset
    matchLabels:
      app: my-nginx-pod
  template: # Definition Pods
    metadata:
      name: my-nginx-pod
      labels:
        app: my-nginx-pod
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

```shell
$ k apply -f replicaset_nginx.yaml
$ kgp
```

# Replicaset(레플리카셋)
- replica 속성에 넣은 숫자만큼 파드를 띄움
- `replicaset`의 목적은 설정한 파드 집합의 실행을 항상 안정적으로 유지하는 것.
- 가용성 보증.

### 단일 파드를 띄웠을 경우
- 파드를 단독으로 생성할때 레플리카셋의 selector와 일치하는 레이블을 가지고 있다면 해당 파드에 대한 획득이 가능하다.
- 단독 파드가 생성한 레플리카셋에 파드로 들어가게 되는 의도치 않는 상황이 발생 할 수 있다.
- 이처럼 레플리카 셋은 레이블을 참고하여 동작한다.

### label
- 일종의 grouping
- label이 중복되면 안되기 때문에 컨벤션 필요

### ReplicaSet의 대안 : Deployment
- `ReplicaSet`의 상위 개념으로 `Deployment`를 많이 사용한다.
- 롤링 업데이트 : `ReplicaSet`은 단순히 파드 복제본을 유지하는 역할을 하지만 `Deployment`는 새로운 버전의 파드를 배포하고 기존 버전의 파드를 제거하면서 서비스의 가용성을 유지함.
- 이력 추적 : `Deployment`는 애플리케이션 배포의 이력을 관리하여 이전 배포 버전의 정보를 추적하고 롤백을 수행 할 수 있음.
- 라벨 셀렉터 : `Deployment`는 `ReplicaSet`을 생성하고 관리하는데 사용되는 라벨 셀렉터를 자동으로 관리한다.
- 이러한 이점으로 `ReplicaSet`을 직접적으로 사용하기 보다는 `Deployment`를 사용하는 것을 권장한다.
- [레플리카셋을 사용하는 시기](https://kubernetes.io/ko/docs/concepts/workloads/controllers/replicaset/#%EB%A0%88%ED%94%8C%EB%A6%AC%EC%B9%B4%EC%85%8B%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EC%8B%9C%EA%B8%B0)

