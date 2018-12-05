# 用docker一键构建开发环境

这个项目提供了一键构建LNMP开发环境，基于Linux，建立本地开发环境Nginx、MySQL、PHP;
Nginx可以设定多个虚拟主机;



## 安装步骤


1. Install Docker. If you’re running a Linux distribution, use its package manager. If you’re using either macOS or Windows, download the respective Docker package installers: [Docker for Mac](https://docs.docker.com/docker-for-mac/) or [Docker for Windows](https://docs.docker.com/docker-for-windows/).

2. git clone https://git.jinxidao.com/docker/php-docker-dev.git, you can clone it in your any path.

3. cp file docker-compose.yml.demo to docker-compose.yml `cp docker-compose.yml.demo docker-compose.yml`

4. In `docker-compose.yml` change the default php volumes  setting, `/PATH/TO/YOUR/DOCUMENT/ROOT`, to suit your web root path . 

5. More than likely, there will be a `/PATH/TO/YOUR/DOCUMENT/ROOT/site1.com/` directory in your source. So change  `docker/nginx/site1.conf` setting to be `root /var/www/html/site1.com;`.

6. Setting  `/etc/hosts`, add `127.0.0.1 www-local.site1.com`.

7. Make sure `/PATH/TO/YOUR/DOCUMENT/ROOT` has index.html and index.php file.

8. Build the configuration by running: `docker-compose up`.

9. Open browser urls: `http://localost` and `http://www-local.site1.com`.

10. Check log file by runnig `docker-compose logs -f`.


### 验证

To check that the build is working, run `docker-compose ps`.
This should give you output similar to the below.

```console
     Name               Command          State          Ports
---------------------------------------------------------------------
ycf-nginx-local   nginx -g daemon off;   Up      0.0.0.0:80->80/tcp
ycf-php-local     php-fpm                Up      9000/tcp

```

If you’d like to see more detailed information, check the log file by running `docker-compose logs`.
To check that the files are inside the container, run the following command, substituting `ycf-nginx-local` to match your directory name:

```console
docker exec -it ycf-nginx-local ls -lahrt /var/www/html

```

### 重新rebuild

* 跟新git仓库
* `docker-compose build`

#my config
