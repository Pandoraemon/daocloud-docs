---
title: '如何在 DaoCloud 上部署 Flask Web 应用'
taxonomy:
    category:
        - docs
process:
    twig: true
---



> 目标：把 Flask Web 应用部署到 DaoCloud 上
> 
> 本项目代码维护在 **[DaoCloud/hello-docker-flask](https://github.com/DaoCloud/hello-docker-flask)** 项目中。
>
> 您可以在 GitHub 找到本项目并获取本文中所提到的所有代码文件。

#### 前言

对于个人开发者来说， 将个人博客或者其他 Web 应用部署到云端是一个最常见的需求。下面以 Flask Web 应用为例介绍如何将 Web 应用部署到 DaoCloud 。

> Flask 是一种非常容易上手的 python Web开发框架，不需要我们知道太多的 MVC 的概念，只需要具备基本的 python 开发技能，就可以开发出一个 Web 应用来。。

#### 准备 Flask Web 程序

* 在 Python 虚拟环境中使用 pip 安装 Flask 包

```bash
(venv) $ pip install flask
```

* 生成需求文件

```bash
(venv) $ pip freeze >requirements.txt
```

* hello.py 程序

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def index():
    return '<h1>Hello DaoCloud</h1>'

if __name__ == "__main__":
    app.run(debug=True, host='0.0.0.0')
```


#### 编写 Dockerfile

- 选择 Python 2.7 版本为我们依赖的系统镜像。

```dockerfile
FROM daocloud.io/python:2.7
```

> 因所有官方镜像均位于境外服务器，为了确保所有示例能正常运行，使用与官方镜像保持同步的 DaoCloud 境内镜像。   

- 设置镜像的维护者，相当于镜像的作者或发行方。

```dockerfile
MAINTAINER Pandoraemon
```

- 向镜像中添加文件并安装依赖。

```dockerfile
RUN mkdir -p /app
COPY . /app
WORKDIR /app

RUN pip install -r ./requirements.txt
```

- 启动应用进程

```dockerfile
EXPOSE 80
ENTRYPOINT ["python"]
CMD ["hello.py"]
```


#### 代码构建

- 创建项目

依次点击代码构建，创建新项目， 进入创建新项目页面。

输入项目名称，设置代码源，其他选项暂时使用默认选项。

点击开始创建。

![](create-project.png)

- 构建代码

在构建页面， 在左上角选择分支，点击手动构建。

构建结束后构建记录中会显示执行成功。

![](create-project2.png)

#### 部署镜像


在构建记录页面点击查看镜像，然后点击部署最新版本。

![](create-success.png)

输入镜像名称，配置运行环境。

点击基础配置，然后立即部署。

![](createApp.png)


- 修改容器端口

在应用详情页面，点击配置， 将容器端口修改为 5000，点击保存更改。

![](log.png)

- 查看日志

点击日志，出现如下消息，应用已经运行。

点击访问地址，即可在浏览器看到 Hello DaoCloud 。

![](createApp.png)




