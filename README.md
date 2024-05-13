- $RUNNER_TOKEN(注册令牌)：TD1kkBT3NCLxKaFySnKx
- $URL(服务器地址)：gitlab (容器内地址)
- $GITLAB_CONFIG(gitlab 配置文件地址)：/Users/user/Documents/gitlab-cicd/gitlab/config
- $GITLAB_LOG(gitlab 日志地址):/Users/user/Documents/gitlab-cicd/gitlab/logs
- $GITLAB_DATA(gitlab 数据文件地址):/Users/user/Documents/gitlab-cicd/gitlab/data
- $GITLAB_RUNNER_CONFIG(gitlab-runner 配置文件地址)：/Users/user/Documents/gitlab-cicd/gitlab-runner/config

## 安装 gitlab

```bash
docker run -d \
           -h $URL \
           -p 443:443 \
           -p 80:80 \
           -p 222:22 \
           --name gitlab \
           --restart always \
           -v $GITLAB_CONFIG:/etc/gitlab \
           -v GITLAB_LOG:/var/log/gitlab \
           -v $GITLAB_DATA:/var/opt/gitlab \
           gitlab/gitlab-ce:latest

```

```bash
docker run -d \
           -h gitlab \
           -p 443:443 \
           -p 80:80 \
           -p 222:22 \
           --name gitlab \
           --restart always \
           -v C:\Users\Adrian.wang\gitlab-docker\gitlab\config\config:/etc/gitlab \
           -v C:\Users\Adrian.wang\gitlab-docker\gitlab\config\log:/var/log/gitlab \
           -v C:\Users\Adrian.wang\gitlab-docker\gitlab\config\data:/var/opt/gitlab \
           gitlab/gitlab-ce:latest


```

## 安装 gitlab-runner

```bash
docker run -d \
           --name gitlab-runner --restart always \
           -v $GITLAB_RUNNER_CONFIG:/etc/gitlab-runner \
           -v /var/run/docker.sock:/var/run/docker.sock \
           gitlab/gitlab-runner:latest
```

## 创建一个 docker 网络

```bash
docker network create gitlab-network
```

## 加入同一个 docker 网络

```bash
docker network connect gitlab-network gitlab
docker network connect gitlab-network gitlab-runner
```

## 注册 gitlab-runner

```bash
docker run --rm -v $GITLAB_RUNNER_CONFIG:/etc/gitlab-runner gitlab/gitlab-runner register \
  --non-interactive \
  --url "https://$URL/" \
  --token "$RUNNER_TOKEN" \
  --executor "docker" \
  --docker-image alpine:latest \
  --description "docker-runner"

```

```bash
docker run --rm -v /Users/user/Documents/gitlab-cicd/gitlab-runner/config:/etc/gitlab-runner gitlab/gitlab-runner register \
  --non-interactive \
  --url "https://localhost/" \
  --token "TD1kkBT3NCLxKaFySnKx" \
  --executor "docker" \
  --docker-image alpine:latest \
  --description "docker-runner"


```
