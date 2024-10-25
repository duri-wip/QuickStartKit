## 우분투에 자바 설치하기

### 1. Java 설치 가능 버전 확인

```bash
sudo apt update
sudo apt search openjdk
```

### 2. Java 설치

```bash
sudo apt install openjdk-11-jdk
```

### 3. Java 설치 확인
```bash
java -version
```

### 4. 설치 경로 확인

#### 1. which 명령어 사용

```bash
which java
```
만약 /usr/bin/java와 같은 경로가 출력된다면 해당 경로는 심볼릭 링크일 가능성이 크다. 자바의 실제 설치 경로를 찾으려면 2번 방법을 사용한다. 

#### 2. readlink 명령어 사용 (심볼릭 링크 확인)

```bash
readlink -f $(which java)
```

### 5. 환경변수 설정

```bash
export JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64"
export PATH="$JAVA_HOME/bin:$PATH"
```
설정 적용
```bash
source ~/.bashrc
```
#### 4. 환경 변수 설정 확인

```bash
echo $JAVA_HOME
echo $PATH
```
