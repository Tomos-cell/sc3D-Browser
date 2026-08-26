## Introduction
sc3D-Browser is a web-based viewer for single-cell and 3D chromatin genomic data, originally designed to demonstrate multi-modal data sets.

## Citation
submiting...

## Dependencies
* docker
* sqlite

## Installation

#### 1. Install docker and load docker image
```
git clone https://github.com/Tomos-cell/sc3D-Browser
docker pull sdas124v/sc3d_django
docker pull sdas124v/sc3d_nginx
```

#### 2. Run sc3D-Browser container (for mac or linux user)
```
db="/d/desktop/database" // the absolute path of database folder
ip="localhost" // input "localhost" to run locally or IP to run remotely
nproc=8 // according to your cpu number

docker network create sc3d_network --driver bridge
docker run -d --name sc3d_django_container --security-opt seccomp=unconfined --restart always --volume ${db}:/app/database --expose 8001 -e DJANGO_ALLOWED_HOSTS=${ip} -e DB=${db} -e UWSGI_PROCESSES=${nproc} -e OPENBLAS_NUM_THREADS=${nproc} --network sc3d_network sc3d_django
docker run -d --name sc3d_nginx_container --restart always -p 8788:80 --volume ${db}/cifdata:/app/database/cifdata --network sc3d_network sc3d_nginx
docker exec -it sc3d_django_container python manage.py makemigrations 
docker exec -it sc3d_django_container python manage.py migrate 
``` 
#### (for windows user)
```
$db="D:\desktop\database" // the absolute path of database folder
$ip="localhost" // input "localhost" to run locally or IP to run remotely
$nproc=8 // according to your cpu number

docker network create sc3d_network --driver bridge
docker run -d --name sc3d_django_container --security-opt seccomp=unconfined --restart always --volume ${db}:/app/database --expose 8001 -e DJANGO_ALLOWED_HOSTS=${ip} -e DB=${db} -e UWSGI_PROCESSES=${nproc} -e OPENBLAS_NUM_THREADS=${nproc} --network sc3d_network sc3d_django
docker run -d --name sc3d_nginx_container --restart always -p 8788:80 --volume ${db}/cifdata:/app/database/cifdata --network sc3d_network sc3d_nginx
docker exec -it sc3d_django_container python manage.py makemigrations
docker exec -it sc3d_django_container python manage.py migrate
```

#### 3、Access web
If you run it locally, please access at http://localhost:8989. 

If you run it remotely, please access http://ip:8989.

If you need help about registering and uploading, please access http://ip:8989/doc.
