# Docker container에서 MNIST CODE TEST

```bash
pip install -r requirements.txt
python main.py
# CUDA_VISIBLE_DEVICES=2 python main.py  # to specify GPU id to ex. 2
```
1. code clone
(git bash)
git clone [repository주소]

2. pycharm에서 project 열기

3. 서버에서 해당 코드를 deploy(복사)할 디렉토리 생성
mkdir [dir_name]

4. pycharm에서 해당 위치로 deployment 세팅 후 서버로 잘 복사되는지 확인
tools-deployment-configuration에서 ssh, path 세팅 후 upload (ctrl+alt_shift+x)

5. 필요 설치 환경 확인
a. github repository에 requirements.txt나 Dockerfile이 존재하는지 확인
b. 없으면 임의의 torch image를 이용
6. requirements.txt, Dockerfile 작성
Dockerfile 작성

예시)
FROM pytorch/pytorch:1.11.0-cuda11.3-cudnn8-runtime
RUN apt-get update
WORKDIR /mnist 
COPY requirements.txt requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

7. requirement.txt, Dockerfile 서버로 deploy
8. Docker image build
docker build --rm -t [image_name] .

9. 테스트용 Docker container 생성
docker run --name [container_name] --rm --gpus '"device=0,1"' --shm-size 8G -it \
 -v [server에서 불러올 path]:[container에서 구동할 path (WORKDIR에 지정한 경로)] e.g) /home/gy0ny/mnist:/mnist \
 [image_name] /bin/bash/

