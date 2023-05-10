# 실습 전 환경 구축

> 실습환경
> 
> OS : macOS  
> 칩 : M2  
> VM : [UTM Virtual Machines](https://mac.getutm.app/)

- m2 환경으로 VM이아닌 docker를 사용할려고 했으나 최대한 책의 실습환경과 유사한 환경으로 하는게 괜찮을거 같아서 UTM으로 실습 진행
- 최소사양`(cpu : 2, memory : 4g, disk: 10GB)`으로 VM 생성
- 우분투 이미지는 `ARM`으로 다운로드 받아서 가상화모드로 설치
- [Ubuntu for ARM | Download](https://ubuntu.com/download/server/arm)

<br/>

---
# docker 사용시 컨테이너 생성 명령어

```shell
docker run -d -it --cpus=2 --memory=4g --name ubun-01 ubuntu:latest
docker run -d -it --cpus=2 --memory=4g --name ubun-02 ubuntu:latest
docker run -d -it --cpus=2 --memory=4g --name ubun-03 ubuntu:latest
```