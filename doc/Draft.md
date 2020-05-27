# Draft

Draft是微软Deis团队开源（见Azure/draft）的容器应用开发辅助工具，它可以帮助开发人员简化容器应用程序的开发流程。

Draft主要由三个命令组成

+ draft init：初始化docker registry账号，并在Kubernetes集群中部署draftd（负责镜像构建、将镜像推送到docker registry以及部署应用等）
+ draft create：draft根据packs检测应用的开发语言，并自动生成Dockerfile和Kubernetes Helm Charts
+ draft up：根据Dockfile构建镜像，并使用Helm将应用部署到Kubernetes集群（支持本地或远端集群）。同时，还会在本地启动一个draft client，监控代码变化，并将更新过的代码推送给draftd。

## Draft安装

由于Draft需要构建镜像并部署应用到Kubernetes集群，因而在安装Draft之前需要

+ 部署一个Kubernetes集群，部署方法可以参考kubernetes部署方法（注意minikube的支持还有问题，暂时不要使用）
+ 安装并初始化helm，具体步骤可以参考helm使用方法
+ 注册docker registry账号，比如Docker Hub或Quay.io
+ 配置Ingress Controller并在DNS中设置通配符域*的A记录（如*.http://draft.example.com）到Ingress IP地址。

## 使用

### Java

### Python

```bash
$ git clone [Azure/draft](https://github.com/Azure/draft.git)
$ cd draft/examples/python
$ ls
app.py           requirements.txt

$ cat requirements.txt
flask
$ cat app.py
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return "Hello, World!\n"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080)
```

Draft create生成Dockerfile和chart

```bash
$ draft create
--> Python app detected
--> Ready to sail
$ ls
Dockerfile       app.py           chart            draft.toml       requirements.txt
$ cat Dockerfile
FROM python:onbuild
EXPOSE 8080
ENTRYPOINT ["python"]
CMD ["app.py"]
$ cat draft.toml
[environments]
  [environments.development]
    name = "virulent-sheep"
    namespace = "default"
    watch = true
    watch_delay = 2
```

Draft Up构建镜像并部署应用

```bash
$ draft up
--> Building Dockerfile
Step 1 : FROM python:onbuild
onbuild: Pulling from library/python
10a267c67f42: Pulling fs layer
....
Digest: sha256:5178d22192c2b8b4e1140a3bae9021ee0e808d754b4310014745c11f03fcc61b
Status: Downloaded newer image for python:onbuild
# Executing 3 build triggers...
Step 1 : COPY requirements.txt /usr/src/app/
Step 1 : RUN pip install --no-cache-dir -r requirements.txt
....
Successfully built f742caba47ed
--> Pushing docker.io/feisky/virulent-sheep:de7e97d0d889b4cdb81ae4b972097d759c59e06e
....
de7e97d0d889b4cdb81ae4b972097d759c59e06e: digest: sha256:7ee10c1a56ced4f854e7934c9d4a1722d331d7e9bf8130c1a01d6adf7aed6238 size: 2840
--> Deploying to Kubernetes
    Release "virulent-sheep" does not exist. Installing it now.
--> Status: DEPLOYED
--> Notes:

  [http://virulent-sheep.app.feisky.xyzto](http://virulent-sheep.app.feisky.xyzto) access your application

Watching local files for changes...
```
