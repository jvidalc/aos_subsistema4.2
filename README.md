# Subsistema 4. Gesti贸n de notificaciones 馃敂
Env铆o de **notificaciones** relacionadas con el funcionamiento del taller. 
Este subsistema es el encargado de notificar a los clientes el estado de los diferentes trabajos.

## 馃搵 Descripci贸n del servicio de notificaciones

### Implementaci贸n
Hemos empleado la herramienta [swagger-codegen](https://github.com/swagger-api/swagger-codegen) para implementar nuestra API. En concreto usando el framework Spring Boot.

Hemos utilizado la imagen de mysql de Docker Hub como tecnolog铆a de persistencia de datos para nuestro servicio.

### Imagen Docker Hub

- [Link](https://hub.docker.com/repository/docker/jvidalc/aos_subsistema4_notificaciones)
- Imagen: `jvidalc/aos_subsistema4_notificaciones:latest`

>***NOTA***: 
>- `Debido a problemas de integraci贸n de la base de datos de nuestro servicio para el resto de grupos, hemos creado una versi贸n de la imagen con un mock para las peticiones de la API` 


### Endpoints de la API de notificaciones

- /AOS4/notificacion
  - GET: Devuelve en un array todas las notificaciones de un cliente
  - POST: Crea una notificaci贸n especificando las propiedades requeridas.
  - OPTIONS: Proporciona la lista de los m茅todos HTTP soportados.

- /AOS4/notificacion/{notificacionId}
  - GET: Recupera una notificaci贸n espec铆fica identificada por su ID.
  - PUT: Modifica una notificaci贸n espec铆fica identificada por su ID.
  - DELETE: Elimina una notificaci贸n espec铆fica identificada por su ID.
  - OPTIONS: Proporciona la lista de los m茅todos HTTP soportados.


## 鈿欙笍 Despliegue de todos los servicios del taller

### docker-compose

Para desplegar el servicio con docker-compose hemos considerado las siguientes decisiones de dise帽o:
- Un contenedor por imagen de servicio, cada uno con puerto externo diferente asignado.
- Hemos creado una network com煤n a todos los serivicios: `network: taller`
- Un contenedor con la imagen de mysql:8 para la persistencia de datos
- Una `network: ss4-mysql` para conectar nuestro servicio de notificaciones con el contenedor de persitencia de datos mysql.
  
Para ejecutar el servicio se deber谩 hacer uso del archivo `docker-compose.yml` disponible en el repositorio. A continuaci贸n se proceder谩 a ejecutar el siguiente comando sobre su directorio:
```
docker-compose up -d
```

### kubernetes

Hemos empleado la herramienta [minikube](https://minikube.sigs.k8s.io/docs/start/) para el despliegue de los serivicios del taller.

Para desplegar el servicio con kubernetes hemos considerado las siguientes decisiones de dise帽o:
- Hemos creado un **deployment** por cada servicio del taller, asignando un puerto de acceso de pod distinto a cada uno para evitar posibles conflictos.
- Un **servicio** por cada deployment. Teniendo en cuenta que necesit谩bamos acceder a los puertos de cada uno de los servicios para probarlos, consideremos que los puertos sean tipo 麓NodePort麓, a pesar de que no sea la opci贸n m谩s optima para un servicio backend de una API rest.
- Tambi茅n creamos un `PersistentVolumeClaim` para dotar de persistencia al despliegue de la persistencia de datos en mysql.

De cara a la implementaci贸n del servicio en un cluster de kubernetes se puede usar la template `kubernetes-deployment.yml`.
```
kubectl apply -f .\kubernetes-deployment.yaml
```

### Despliegue en nube p煤blica

- Despliegue en Amazon Web Services usando Elastic Kubernetes Services. Hemos decidido desplegar la aplicaci贸n en la nube publica de Amazon. 

![Captura](./capturas/amazon-web-services.png "Amazon Web Services. Elastic Kubernetes Services.")

- Hemos intentado el despliegue de la aplicaci贸n con cada uno de los servicios en la nube, pero a pesar de disponer el cr茅dito necesario, el portal de azure no nos ha permitido desplegarlo sin introducir un m茅todo de pago. 

![Captura](./capturas/azure-1.jpeg "Amazon Web Services. Elastic Kubernetes Services.")
![Captura](./capturas/azure-2.jpeg "Amazon Web Services. Elastic Kubernetes Services.")

### PostMan 

- Hemos usado Postman para probar las peticiones a la api del servicio.

![Captura](./capturas/postman.png "Amazon Web Services. Elastic Kubernetes Services.")
