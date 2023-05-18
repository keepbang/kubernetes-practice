# Kubernetes RBAC-롤바인딩(rolebinding)

- RBAC : Role-Based Access Control
- 역할 기반으로 쿠버네티스 시스템 권한 관리

### 롤바인딩
- 특정 네임스페이스에 대한 권한을 관리 한다.
- ex) 특정 사용자에게 해당 사용자 전용 네임스페이스를 따로 만들어주고 그 네임스페이스만 컨트롤 할 수 있게 계정을 부여함.

-----

### 1. 사용자 생성
- `kubectl create serviceaccount [계정명]`

```
# kubectl create serviceaccount keepbang
```

### 2. 롤 생성
- `Role` 파일 생성 후 `apply`를 통해 적용

[role.yml]
```shell
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: service-reader
rules:
- apiGroups: [""] # 대상이 될 오브젝트 API 그룹, kubectl api-resources
  resources: ["services"] # 대상이 될 오브젝트
  verbs: ["get", "list"] # 허용할 동작
```

```
# kubectl apply -f ./role.yml
```

-----

### 3. 롤 바인딩 생성
- `RoleBinding` 파일 생성 후 `apply`를 통해 적용

[service-reader-rolebinding.yml]
```shell
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  namespace: default
  name: service-reader-rolebinding
subjects: # 누구에게
- kind: ServiceAccount # 권한 부여 오브젝트
  name: keepbang # 계정 명
  namespace: default
roleRef:  # 어떤 롤을
  kind: Role # Role에
  name: service-reader # service-reader로 명명된 Role 을 부여 하겠다.
  apiGroup: rbac.authorization.k8s.io
```

```
# kubectl apply -f ./service-reader-rolebinding.yml
```

-----

### 4. 서비스 확인

```
# kubectl get services --as system:serviceaccount:default:keepbang
```

![img.png](img.png)


-----

#### 과제
- Cluster role 생성해보기