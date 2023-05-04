# 실습 전 환경 구축

- 책에서는 VM으로 서버를 띄웠지만 여기서는 docker로 서버를 구동함
- 최소사양(cpu : 2, memory : 4g)으로 컨테이너 생성

> 상황에 따라 vm 으로 변경될 가능성 존재함

```shell
docker run -d -it --cpus=2 --memory=4g --name ubun-01 ubuntu:latest
docker run -d -it --cpus=2 --memory=4g --name ubun-02 ubuntu:latest
docker run -d -it --cpus=2 --memory=4g --name ubun-03 ubuntu:latest
```

