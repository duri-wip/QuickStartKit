### docker 환경 구성

https://docs.docker.com/engine/install/ubuntu/

```
Sudo -i # sudo 권한 얻어내기
Apt install docker
Systemctl enable --now docker #다음 부팅에도 실행해줘
```

```python
# sudo 없이 docker 사용하고 , 데몬 등록하기
sudo usermod -aG docker kevin
sudo systemctl daemon-reload
sudo systemctl enable docker
sudo systemctl restart docker
sudo systemctl status containerd.service
sudo reboot
docker version
```
