#papercut 

한 서버에서 여러개의 컨테이너를 하나의 서비스로 묶어서 관리해주는 기능을 제공.

컨테이너의 실행순서와 의존관계, 불륨등의 설정을 `docker compose` 파일에서 정의할 수 있다.

docker compose가 없다면 컨테이너 하나하나 실행하는 cli를 직접 수행해야하는 번거로움이 있다.
### docker compose 설치
https://docs.docker.com/compose/install/
### docker-compose.yml

별도의 설정을 하지 않으면 `docker-compose`는 현재 디렉토리의 `docker-compose.yml` 파일을 읽어 로컬의 도커 엔진에게 컨테이너 생성을 요청한다.

`docker copose up` 명령어로 도커 컴포즈 프로젝트를 실행할 수 있다.

```yml
version: '2'
services: 
	zookeeper:
		image: zookeeper: 3.8.1
		container_name: zookeeper
		ports:
			- "2181:2181"
		restart: unless-stopped
	kafka:
		image: wurstmeister/kafka:2.13-2.8.1
		container_name: kafka
		hostname: kafka
		ports:
			- "9092:9092"
		environment:
			KAFKA_ADVERTISED_HOST_NAME: localhost
			KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
		volumes:
			- /var/run/docker.sock:/var/run/docker.sock
		restart: unless-stopped
```
