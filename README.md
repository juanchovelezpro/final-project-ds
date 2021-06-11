# App in a Kubernetes Cluster

Para poder ejecutar esta aplicación se deben realizar los siguientes pasos:

1.  Un cluster local de kubernetes (para este caso se utilizó microk8s)
Para instalar microk8s se pueden seguir los pasos aquí: https://microk8s.io/

2. Para poder garantizar el correcto funcionamiento de la aplicación se deben tener habilitados los siguientes add-ons:
	- dns
	- metallb
	Los puede habilitar con el siguiente comando
`microk8s enable dns metallb`
En esta parte se pedirá un rango de direcciones para el LoadBalancer y lo puede especificar así: 192.168.200.10-192.168.200.254 o cualquier otro rango de direcciones que desee.

3. Clonar este repositorio:
`git clone https://github.com/juanchovelezpro/final-project-ds.git`

4.  Una vez clonado el repositorio accedemos al directorio /final-project-ds/k8s que es el lugar donde se encuentran los archivos de configuración para el despliegue de las aplicaciones y los servicios.

5. Dentro del directorio k8s ejecutaremos los siguientes comandos:

- microk8s status: Con este comando verificados el estado de nuestro cluster.
- microk8s kubectl get nodes: Verificamos que el nodo se encuentre Ready, para que pueda desplegar los contenedores en los pods y tambien desplegar los servicios.

Ahora se procede a desplegar la aplicación con los siguientes comandos:

    microk8s kubectl apply -f db-deployment.yaml
    microk8s kubectl apply -f db-service.yaml
    microk8s kubectl apply -f app-deployment.yaml
    microk8s kubectl apply -f app-service.yaml

Una vez ejecutados estos comandos, revisamos el estado de los pods con el comando:

`microk8s kubectl get pods`

Y se debe esperar hasta que los pods se encuentren corriendo tanto la aplicación como la base de datos (se deben de tener 4 pods ya que la base de datos cuenta con 2 debido a la replica al igual que la aplicación)

Por último se debe ejecutar el comando

`microk8s kubectl get services`

Con el cual podremos mirar los servicios disponibles en el cluster y se podrá encontrar el servicio pythonapp-svc el cual muestra una IP externa gracias al LoadBalancer y con dicha IP y un puerto especificado en este caso el 8000 se podrá acceder a la aplicación web.


## Para desplegar en ambiente de producción

Para desplegar en un ambiente de producción se pueden considerar los servicios que se brindan en la nube uno de ellos puede ser GKE (Google Kubernetes Engine) que permite realizar lo mismo que se hizo en este repositorio solo que en un cluster que se encuentra en la nube y que tiene visibilidad en internet. 

Algunas consideraciones importantes tambien para desplegar en producción son las siguientes:
- La disponibilidad, es decir que tantas máquinas para desplegar el cluster.
- Escalabilidad
- La seguridad y la gestión del acceso en general al cluster.
- La capacidad de memoria, CPU y espacio de almacenamiento requerido para desplegar lo necesario.

## Que hariamos si tuvieramos mas tiempo?

- Implementariamos la funcionalidad del /health.
- Desplegariamos la aplicación en un cluster en la nube como lo es GKE.
