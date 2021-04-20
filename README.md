# Welcome to the k8s-hands-on wiki!

## Introducción al repositorio

Es una práctica simulando una aplicación real implementada con kubernetes. _No está enfocada en el código de la aplicación, sino en el deploy de la misma._ 

**1. Componentes**

**1.1 Servicios:**

**1.1.1 Servicio Front End**

Utiliza un servicio NodePort _(Permite exponer el puerto fuera del cluster)_

**1.1.2 Servicio Back End**

Utiliza un servicio Cluster IP _(Permite la comunicación entre pods)_

**1.2 Deployments**

* Un deployment para el front end, que cuenta con 3 replica sets
* Un deployment para el back end, que también cuenta con 3 replica sets

**1.3 Servidor nginx**

**2 Proceso:**

* 2.1. El usuario ingresa a la página
* 2.2.  Realiza un request al servicio nodeport 
* 2.3. El servicio nodeport realiza un request al Cluster IP
* 2.4. El cluster IP aplica balanceo round robin y deriva la petición a un POD en ejecución
* 2.5. El POD resuelve el request.

## ¿Cómo probar el funcionamiento?

### Precondiciones:

* Tener instalado kubectl
* Tener instalado docker
* Tener instalado minikube o algún otra herramienta para la adminsitración de clusters
* Tener creado un cluster

### How-To

**1)** Descargar el repo: `git clone https://github.com/rodrigoenzohernandez/k8s-hands-on.git`

**2)** Creamos las imágenes que serán consumidas por los manifiestos de k8s:

**2.1. Imagen Back End**

`rohernandez@rohernandezOmen:~/Escritorio/kubernetes/k8s-hands-on/backend$ docker build -t backend-k8s-hands-on:v1 -f Dockerfile .`

**2.2. Imagen Front End**

`rohernandez@rohernandezOmen:~/Escritorio/kubernetes/k8s-hands-on/frontend$ docker build -t frontend-k8s-hands-on:v1 -f Dockerfile .`

**3)** Aplicamos los manifiestos .yaml

**3.1 Backend**

`rohernandez@rohernandezOmen:~/Escritorio/kubernetes/k8s-hands-on/backend$ kubectl apply -f backend.yaml`

**3.2 Frontend**

`rohernandez@rohernandezOmen:~/Escritorio/kubernetes/k8s-hands-on/backend$ kubectl apply -f frontend.yaml`


**4)** Probamos el funcionamiento. En un navegador ingresamos la IP de nuestro equipo : el puerto que expone el servicio nodeport (Para saberlo lo realizamos con el comando `kubectl get svc`

Por ejemplo: http://192.168.0.161:32356/

![](https://github.com/rodrigoenzohernandez/k8s-hands-on/blob/master/wiki/Test.png)


_Para que el front se pueda comunicar con el back es necesario tener agregada la línea en el archivo /etc/hosts con la IP del cluster IP del servicio back y el nombre del servicio back_
