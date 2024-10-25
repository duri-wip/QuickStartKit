# 스파크 로컬에 설치하기

## 1. 다운로드하기

[다운로드할 파일 확인: 스파크 다운로드페이지](https://archive.apache.org/dist/spark/)

파일 확인하고 소스 복사해서 wget해도 된다.

```bash
wget https://archive.apache.org/dist/spark/spark-3.5.1/spark-3.5.1-bin-hadoop3.tgz
```
**참고**
스파크는 하둡을 사용한다. 다운로드 버전 중 without hadoop이라는 prefix가 붙어 있는 버전은 하둡을 사용하지 않는 스파크라는 뜻이 아니고, 사용자가 지정한 버전의 하둡을 따로 구축해서 사용할 수 있다는 뜻이다.

```bash
tar xvf spark-3.5.1-bin-hadoop3.tgz
```

## 2. 환경 변수 설정 (~/.bashrc 또는 ~/.zshrc):

```bash
export SPARK_HOME=<위에서 설치한 파일의 경로>
export PATH=$SPARK_HOME/bin:$SPARK_HOME/sbin:$PATH
```

적용
```bash
source ~/.bashrc
```

## 3. 설치 확인

```bash
cd $SPARK_HOME

```
---
# 스파크 클러스터 설정하기

## 1. conf/spark-env.sh 파일의 설정(모든 노드에 대해)

```bash
cd $SPARK_HOME/conf
cp spark-env.sh.template spark-env.sh
```

## 2. 마스터 노드 IP 설정
```
export SPARK_MASTER_HOST=<마스터 노드의 ip>
```

## 3. 워커 노드 ip 설정

```bash
cp conf/workers.template conf/workers
```

그리고 원래 존재하는 localhost를 주석처리하고 워커 노드의 ip를 추가한다.

## 4. 클러스터 시작

### 한번에 시작하기
```bash
$SPARK_HOME/sbin/start-all.sh
```
### 따로 시작하기
```bash
$SPARK_HOME/sbin/start-master.sh
$SPARK_HOME/sbin/start-slaves.sh
```
---
# 스파크 라이브러리 설정하기

## PySpark 설정
pyenv 등 파이썬 가상환경등을 켜고 라이브러리를 다운로드

```bash
pip install pyspark findspark
```
