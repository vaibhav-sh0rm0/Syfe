##  Run a Production grade Wordpress app on Kubernetes ##
![](https://img.shields.io/badge/-Task--1-brightgreen)

### Objectives:


- Create PersistentVolumeClaims and PersistentVolumes
ReadWriteMany volumes for deployment scaling


- Create DockerFile for Wordpress, Mysql and Nginx. All the request will proxy pass from nginx to wordpress.
- We using lua in Nginx, so compile a opensresty with lua while building docker container image for Nginx.
- Use below configure option in opensresty build

```
1./configure --prefix=/opt/openresty \
2 --with-pcre-jit \
3 --with-ipv6 \
4 --without-http_redis2_module \
5 --with-http_iconv_module \
6 --with-http_postgres_module \
7 -j8
```
- Apply should be using Helm chart like helm install my-release my-repo/wordpress
- Clean up helm delete my-release



### Steps ###
- Create PersistentVolumeClaims and PersistentVolumes for the Wordpress application. PersistentVolumeClaims are used to request storage resources from the cluster, while PersistentVolumes are used to define the actual storage resources.

- Set up ReadWriteMany volumes for deployment scaling, as multiple pods may need to access the same data.

- Create Dockerfiles for Wordpress, Mysql and Nginx. The Dockerfile for Wordpress should include the necessary configuration settings and plugins for your app.

- Configure Nginx to proxy all requests to Wordpress.

- Apply the Helm chart using the following command:

```helm install my-release my-repo/wordpress
 ```