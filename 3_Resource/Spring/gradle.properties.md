#papercut 

key-value 형태의 변수 값들을 담고있는 파일.

gradle.properties 파일의 변수값들은 빌드 실행 시점에 `build.gradle` 파일의 변수에 주입된다.

주로 버전관리에 사용되며 버전 업데이트가 필요할때 수정 포인트를 줄일 수 있다.

```bash
### Application version ###  
applicationVersion=0.0.1-SNAPSHOT  
  
### Project configs ###  
projectGroup=io.dodn.springboot  
  
### Project depdency versions ###  
kotlinVersion=1.9.20  
javaVersion=17
```

