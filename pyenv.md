### **1. Ubuntu 기반 시스템에서 필요한 패키지 설치**

```bash
sudo apt update
sudo apt install -y build-essential libssl-dev zlib1g-dev libncurses5-dev \
libgdbm-dev libsqlite3-dev libreadline-dev libffi-dev libbz2-dev \
liblzma-dev
```

### **2. `pyenv` 설치**

- **curl**을 사용하는 방법:

  ```bash
  curl https://pyenv.run | bash
  ```

- **wget**을 사용하는 방법:

  ```bash
  wget -qO- https://pyenv.run | bash
  ```

### **3. 설치 후 환경 변수 설정**

- **Bash**를 사용하는 경우 (`~/.bashrc`):

  ```bash
  echo 'export PATH="$HOME/.pyenv/bin:$PATH"' >> ~/.bashrc
  echo 'eval "$(pyenv init --path)"' >> ~/.bashrc
  echo 'eval "$(pyenv init -)"' >> ~/.bashrc
  echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bashrc
  ```

설정 파일을 업데이트한 후, 변경 사항을 적용합니다

```bash
source ~/.bashrc
```

### **4. Python 버전 설치**

```bash
pyenv install 3.9.1  # 예시: Python 3.9.1 설치
pyenv global 3.9.1   # 기본 Python 버전을 3.9.1로 설정
```

---

### Docker File

'''
FROM python:3.8

WORKDIR /app

RUN apt-get update && apt-get install -y \
 curl \
 build-essential \
 libssl-dev \
 zlib1g-dev \
 libncurses5-dev \
 libgdbm-dev \
 libsqlite3-dev \
 libreadline-dev \
 libffi-dev \
 libbz2-dev \
 liblzma-dev \
 git

RUN curl https://pyenv.run | bash

ENV PATH="/root/.pyenv/bin:${PATH}"
RUN eval "$(pyenv init --path)" && eval "$(pyenv init -)" && eval "$(pyenv virtualenv-init -)"

RUN pyenv install 3.8.5
RUN pyenv global 3.8.5

COPY . /app

RUN pip install -r requirements.txt

CMD ["python", "train_diabetes.py", "0.01", "0.75"]
'''
