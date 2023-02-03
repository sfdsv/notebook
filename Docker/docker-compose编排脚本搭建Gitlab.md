docker-compose.yml
```
version: '3'
services:
  gitlab:
    image: 'gitlab/gitlab-ce:latest'
    container_name: 'gitlab'
    restart: always
    hostname: 'gitlab.hcsci.cn'
    environment:
      TZ: 'Asia/Shanghai'
      GITLAB_OMNIBUS_CONFIG: |
        external_url 'https://gitlab.hcsci.cn'
        gitlab_rails['gitlab_ssh_host'] = 'gitlab.hcsci.cn'
        nginx['redirect_http_to_https'] = true
        nginx['ssl_certificate'] = "/etc/gitlab/ssl/gitlab.hcsci.cn_bundle.crt"
        nginx['ssl_certificate_key'] = "/etc/gitlab/ssl/gitlab.hcsci.cn.key"
        letsencrypt['enable'] = false
    ports:
      - '80:80'
      - '443:443'
      - '22:22'
    volumes:
      - ./gitlab/config:/etc/gitlab
      - ./gitlab/data:/var/opt/gitlab
      - ./gitlab/logs:/var/log/gitlab
```

```
docker compose up -d
```
