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




# Configuration 
-  PersistentVolumeClaims and PersistentVolumes for the Wordpress application. PersistentVolumeClaims are used to request storage resources from the cluster, while PersistentVolumes are used to define the actual storage resources.

   ``` kubectl create namespace wordpress ```

- Set up ReadWriteMany volumes for deployment scaling, as multiple pods may need to access the same data.

-  Dockerfiles for Wordpress, Mysql and Nginx. The Dockerfile for Wordpress should include the necessary configuration settings and plugins for your app.
To use my dockerfile 
``` 
git clone https://github.com/vaibhav-sh0rm0/Syfe/blob/db80362b824c166315871e6280d492a40658c17a/Dockerfile.txt

```
   Build Dockerfile 


``` docker build -t wordpress-app . ```

Start Docker Container
``` 
docker run -p 80:80 -p 443:443 -p 8000:8000 -d wordpress-app
 ```
- Configure Nginx to proxy all requests to Wordpress.

- Apply the Helm chart using the following command:

    First, add the official WordPress Helm chart repository:
   ``` 
    helm repo add bitnami https://charts.bitnami.com/bitnami
    ```
    Install the WordPress chart with the following command:
    ``` 
    helm install my-release bitnami/wordpress
    ```
 
 Access the WordPress site by getting the external IP of the WordPress service:
   ``` 
   kubectl get svc my-release-wordpress --template="{{range .status.loadBalancer.ingress}}{{.ip}}{{end}}"
 ```
