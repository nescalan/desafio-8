# Desafío 8

## Descripción

El objetivo de este proyecto es desplegar una aplicación basada en NestJS junto con su base de datos en un entorno Kubernetes. Este repositorio contiene todos los manifiestos, documentación y evidencias necesarias para cumplir con los requisitos planteados en el desafío.

## Enlace a la Aplicación Base

La aplicación utilizada como base para este proyecto está disponible en el siguiente repositorio:
[app-template-nestjs](https://github.com/yosoyfunes/app-template-nestjs)

## Requisitos

1. Transformar los servicios de `docker-compose` utilizados en el desafío No. 5 anterior en manifiestos de Kubernetes.
2. Documentar los pasos y procesos realizados de forma clara y comprensible.
3. Generar evidencia de pruebas exitosas.

## Estructura del Repositorio

```
/desafio-8
│
├── app/                        # Código fuente clonado de la aplicación base.
├── kubernetes/                 # Archivos de manifiestos Kubernetes.
│   ├── app-deployment.yaml     # Manifiesto del Deployment de la aplicación.
│   ├── app-service.yaml        # Manifiesto del Service para exponer la aplicación.
│   ├── mongo-deployment.yaml   # Manifesto del Deployment de la base de datos.
│   └── mongo-service.yaml      # Manifiesto del Service para exponer la basde de datos.
├── docs/                       # Documentación adicional.
│   ├── diagrama.png            # Diagrama de alto nivel de la solución.
│   └── pasos.md                # Pasos detallados para el despliegue.
├── README.md                   # Documentación principal.
└── evidencias/                 # Resultados de pruebas exitosas.
```

## Instrucciones de Uso

### 1. Clonar el repositorio

```bash
git clone https://github.com/nescalan/desafio-8.git
cd desafio-8
```

### 2. Preparar el entorno

Asegúrate de tener instalado:

- Kubernetes (minikube, kind, o un cluster existente).
- kubectl.

### 3. Aplicar los manifiestos

```bash
kubectl apply -f kubernetes/
```

### 4. Verificar el estado de los recursos

```bash
kubectl get all
```

### 5. Acceder a la aplicación

La aplicación estará disponible en la URL configurada en el manifiesto `app-service.yaml`. Si usas Minikube:

```bash
minikube service app-service
```

## Contribuciones

Las contribuciones a este proyecto son bienvenidas. Por favor, abre un issue o un pull request con tus sugerencias o mejoras.

## Licencia

Este proyecto utiliza la misma licencia que el repositorio base [app-template-nestjs](https://github.com/yosoyfunes/app-template-nestjs).
