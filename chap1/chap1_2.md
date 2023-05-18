# Kubespray 설치

### kubectl alias
```
k=kubectl
ka='kubectl apply --recursive -f'
kcn='kubectl create deployment nginx --image=nginx'
kd=kubectl
kg=kubectl
kgd='kubectl get deploy -o wide'
kgdep=kubectl
kge='kubectl get event -w --field-selector type=Warning'
kgn='kubectl get nodes -o wide'
kgp='kubectl get pods -o wide'
kgpa='kubectl get pods -o wide -A'
kgpaw='kubectl get pods -o wide -A -w'
kgpw='kubectl get pods -o wide -w'
kgs='kubectl get service -o wide'
kgv='kubectl get pvc -o wide'
krb='kubectl run busybox --image=busybox --restart -- sleep 1d'
krn='kubectl run nginx --image=nginx --restart=Never'
ksys=kubectl
```

- kubespray
  - 다양한 설치 옵션 및 운영체제를 지원하는 앤서블 기반의 쿠버네티스 설치 자동화 도구.
  - 앤서블 : 애플리케이션 배포와 설정을 자동화하는 오픈소스 프로젝트
  

- 설치 옵션
  1. 컨테이너 런타임 : containerd
     - 1.20 버전부터 컨테이너 런타임으로 도커를 사용하면 경고 메시지 발생(도커 지원 종료 예정)
  2. 감사 로그 활성화
  3. MetalLB 설치를 위한 옵션 추가