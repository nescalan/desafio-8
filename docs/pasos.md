# Pasos para Desplegar la Aplicación en Kubernetes

En este documento explico los pasos que seguí para convertir un proyecto que utilizaba un `Dockerfile` en un despliegue funcional en Kubernetes. Este trabajo fue realizado como parte de un desafío académico, donde aprendí a manejar manifiestos de Kubernetes para orquestar aplicaciones.

## 1. Creación del Deployment de la Aplicación

Primero, definí el manifiesto para el **Deployment** de la aplicación. Este manifiesto se encarga de especificar cómo se debe ejecutar el contenedor de la aplicación en Kubernetes. Aquí están los puntos clave que trabajé:

- Utilicé la imagen de Docker llamada `desafio-8:latest`, que previamente construí y subí al repositorio de imágenes en GitHub.
- Configuré una réplica para mantener una instancia de la aplicación siempre corriendo.
- Asigné variables de entorno, en este caso, la conexión a la base de datos (`MONGO_URI`).
- Definí recursos mínimos y máximos de CPU y memoria para asegurar que la aplicación funcione de manera estable en el clúster.
- Abrí el puerto 3000 para permitir la comunicación con la aplicación.

El manifiesto de este Deployment quedó así:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: app
  template:
    metadata:
      labels:
        app: app
    spec:
      containers:
        - name: app
          image: desafio-8:latest
          ports:
            - containerPort: 3000
          env:
            - name: MONGO_URI
              value: "mongodb://mongo-service:27017/nestdb"
          resources:
            requests:
              memory: "128Mi"
              cpu: "250m"
            limits:
              memory: "256Mi"
              cpu: "500m"
```

## 2. Creación del Deployment para MongoDB

Para la base de datos, configuré un **Deployment** separado que ejecuta MongoDB. En este manifiesto, establecí:

- La imagen oficial de MongoDB (`mongo:latest`).
- Una réplica para mantener un contenedor activo.
- Una variable de entorno para inicializar la base de datos llamada `nestdb`.
- Recursos de CPU y memoria para garantizar que el contenedor de MongoDB funcione adecuadamente.
- Apertura del puerto 27017 para que otros servicios puedan comunicarse con la base de datos.

El manifiesto para MongoDB quedó de esta manera:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongo-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongo
  template:
    metadata:
      labels:
        app: mongo
    spec:
      containers:
        - name: mongo
          image: mongo:latest
          ports:
            - containerPort: 27017
          env:
            - name: MONGO_INITDB_DATABASE
              value: nestdb
          resources:
            requests:
              memory: "256Mi"
              cpu: "500m"
            limits:
              memory: "512Mi"
              cpu: "1000m"
```

## 3. Creación de los Servicios

Para exponer tanto la aplicación como MongoDB, creé dos **Servicios** de Kubernetes:

### Servicio para la Aplicación

- Utilicé un tipo `NodePort` para que la aplicación pueda ser accedida desde fuera del clúster.
- Configuré el puerto 3000 para exponer el servicio.

El manifiesto quedó así:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: app-service
spec:
  type: NodePort
  selector:
    app: app
  ports:
    - protocol: TCP
      port: 3000
      targetPort: 3000
      nodePort: 30000
```

### Servicio para MongoDB

- Utilicé un tipo `ClusterIP` para que MongoDB sea accesible únicamente dentro del clúster.
- Configuré el puerto 27017 para la comunicación.

El manifiesto quedó así:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mongo-service
spec:
  type: ClusterIP
  selector:
    app: mongo
  ports:
    - protocol: TCP
      port: 27017
      targetPort: 27017
```

## 4. Aplicación de los Manifiestos

Con los manifiestos creados, apliqué los recursos al clúster usando `kubectl`:

```bash
kubectl apply -f app-deployment.yaml
kubectl apply -f mongo-deployment.yaml
kubectl apply -f app-service.yaml
kubectl apply -f mongo-service.yaml
```

## 5. Verificación

Finalmente, verifiqué que los recursos se desplegaron correctamente:

```bash
kubectl get all
```

## Conclusión

Este proceso me permitió comprender cómo transformar un `Dockerfile` y un archivo `docker-compose.yml` en recursos de Kubernetes completamente funcionales. Fue interesante aprender a configurar variables de entorno, asignar recursos y exponer servicios, lo cual es esencial para el despliegue de aplicaciones modernas. ¡Una experiencia muy enriquecedora! 😊
