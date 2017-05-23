---
title: Building with AWS S3 in Rancher Compose
layout: rancher-default-v1.6
version: v1.6
lang: en
---

## 使用AWS S3构建

------

Docker版本有两种支持。首先是设置`build:`与使用[Docker Remote API](https://docs.docker.com/reference/api/docker_remote_api_v1.18/#build-image-from-a-dockerfile)兼容的git或HTTP URL 。第二种方法是设置`build:`到本地目录，构建上下文将被上传到S3，然后在每个节点上根据需要构建。

### 要求

- 搬运工人
- 牧场主撰写
- AWS账户
- Rancher服务器运行1主机

在我们的示例中，我们将定义我们的应用程序`docker-compose.yml`并将文件放在`composetest`目录中。撰写文件定义了一个名为的服务`web`，该服务打开`5000`要在主机上公开的容器的端口。还有一个指向一个服务的链接`redis`。运行在容器内的`web`应用程序也可以`redis`通过其主机名到达容器`redis`。

```
版本：' 2 '
服务：
   网络：
     建立：。
    港口：
      - “ 5000：5000 ”
    链接：
      - redis

  redis：
     image：redis
```

我们还将添加一个`rancher-compose.yml`文件到同一个`composetest`目录，以便使用`scale`Rancher 的属性。默认情况下，如果没有`rancher-compose.yml`文件或服务未定义，服务的比例将是一个容器。

```
版本：' 2 '
服务：
   web：
     scale：3
```

一旦文件设置为Rancher Compose，下一步是编写应用程序本身和步骤来构建它。

使用`docker-compose`文档中的示例，我们将创建一个名为的文件`app.py`。该应用程序与被叫的主机进行通话，该主机`redis`预计将运行一个redis KV存储。它会调用所调用的商店中的一个键的值，并将`hits`其检索。

```
从烧瓶导入Flask 
从redis import Redis

app = Flask（__ name__）
redis = Redis（host ='redis'，port = 6379）

@ app.route（'/'）
def hello（）：
     redis.incr（'hits'）return'Hello 
    World！我已经看到了％s次。％redis.get（'hits'）

如果__name__ ==“__ main __”：
     app.run（host =“0.0.0.0”，debug = True）
```

应用程序依赖于两个库，因此我们还将创建一个名为`requirements.txt`。

```
flask
redis

```

现在，我们来定义使用a构建应用程序的步骤`Dockerfile`。在其中`Dockerfile`，该指令定义应该如何构建应用程序容器。

```
FROM python:2.7
ADD . /code
WORKDIR /code
RUN pip install -r requirements.txt
CMD python app.py

```

由于您已经有Rancher服务器正在运行，您需要设置您的AWS凭据，并使用Rancher服务器URL和[API密钥](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/rancher-compose/build/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/api/api-keys)运行Rancher Compose 。

```
＃设置您的AWS凭据
$ aws配置
AWS访问密钥ID []：AWS_ACCESS_KEY
AWS秘密访问密钥[]：AWS_SECRET_KEY
默认区域名称[]：NOT_NEEDED_FOR_S3
默认输出格式[无]：
＃在您创建所有文件的composetest目录中运行rancher 
-compose $ rancher-compose --url URL_of_Rancher --access-key username_of_API_key --secret-key password_of_API_key up
```

使用该命令，Web容器应该在您的Rancher服务器的主机上启动。它将首先将当前目录上传到S3，可以通过转到S3 UI并检查新的上传来进行验证。图像上传后，它将下载到主机，并使用提供的文件构建容器。

### S3构建故障排除

如果您的S3版本遇到问题，您可以在Docker中测试您的构建，以确保可以构建您的映像，并且容器可以运行。在与您运行Rancher Compose命令相同的位置，使用以下命令来测试它是否适用于Docker。

```bash
# Test building locally to see if works
$ docker build -t test .
# Test running the newly built image
$ docker run test
```
