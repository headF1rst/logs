#papercut 

### 설치되어있는 모든 자바 버전 확인
```bash
/usr/libexec/java_home -V
```
### 특정 버전으로 변경 (예: 11)
```bash
export JAVA_HOME=$(/usr/libexec/java_home -v 11)
```

17로 변경
```shell
export PATH="/Users/wrhk0mhgdx/Library/Java/JavaVirtualMachines/corretto-17.0.9/Contents/Home/bin:$PATH"

```
### 변경되었는지 확인
```bash
java -version
```
