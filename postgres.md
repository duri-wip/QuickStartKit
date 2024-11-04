# postgres 설치하고 postico2와 연결하기

## 1. PostgreSQL 설치


```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
```


### postgres 상태 확인하기

```bash
sudo systemctl status postgresql
```

## 2. PostgreSQL 기본 설정

PostgreSQL의 기본 사용자 `postgres`는 슈퍼유저로 생성된다. 이 사용자로 데이터 베이스를 관리할 수 있다. 

```bash
sudo -i -u postgres
psql
```

이제 `psql`에 접속하여 필요한 데이터베이스나 사용자 계정을 추가할 수 있다. 

### 데이터베이스 생성하기

```sql
CREATE DATABASE class1;
```

## 3. 외부에서 PostgreSQL 접근 허용하기

외부에서 PostgreSQL에 접근하려면 설정 파일을 수정해야 한다.

#### `postgresql.conf` 파일 수정

`postgresql.conf` 파일을 열어 `listen_addresses` 설정을 변경.

```bash
sudo vim /etc/postgresql/<version>/main/postgresql.conf
```

`listen_addresses`를 모든 IP에서 접근할 수 있도록 `'*'`로 설정합니다.

```plaintext
listen_addresses = '*'
```

#### `pg_hba.conf` 파일 수정

외부 접근 권한을 설정하려면 `pg_hba.conf` 파일도 수정해야 한다.

```bash
sudo vim /etc/postgresql/<version>/main/pg_hba.conf
```

맨 아래에 다음 줄을 추가하여 모든 IP에서 `md5` 암호 인증을 통해 접근할 수 있도록 설정합니다.

```plaintext
host    all             all             0.0.0.0/0            md5
```

### 이후 재시작해 설정 적용

```bash
sudo systemctl restart postgresql
```

## 4. Postico에서 데이터베이스 연결하기

- **Host**: PostgreSQL 서버의 IP 주소
- **User**: PostgreSQL 사용자 이름 (`postgres` 등)
- **Password**: PostgreSQL 사용자 비밀번호
- **Database**: 데이터베이스 이름 (`class1` 등)

### 5. 인증 오류 해결: Peer Authentication

PostgreSQL 설치 후 `Peer authentication failed for user "postgres"` 오류가 발생할 수 있다. 이는 로컬 접근 시 `peer` 인증 방식을 요구하기 때문에 발생한다.

#### `pg_hba.conf` 파일에서 `peer` 인증을 `md5`로 변경

`pg_hba.conf` 파일을 다시 열고 `postgres` 사용자에 대한 `peer` 인증을 `md5`로 변경한다.

```plaintext
local   all             postgres                                md5
```

#### 비밀번호 설정

`postgres` 사용자의 비밀번호를 설정하여 인증 오류를 해결할 수 있다.

```bash
sudo -i -u postgres
psql
ALTER USER postgres WITH PASSWORD 'your_password';
\q
```

### 6. 테이블을 다른 데이터 베이스로 옮기기

#### 테이블 덤프 생성

옮기고 싶은 테이블의 정보를 담은 dump 파일을 생성한다. 

```bash
pg_dump -U postgres -d postgres -t public.customer -t public.purchase -f customer_purchase_dump.sql
```

#### 덤프 파일을 다른 데이터베이스로 복원

```bash
psql -U postgres -d class1 -f customer_purchase_dump.sql
```

