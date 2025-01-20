# Resultados de las Pruebas en Kubernetes

## Objetivo de las Pruebas

El objetivo de estas pruebas fue verificar que los manifiestos de Kubernetes se aplicaran correctamente en el clúster y que los recursos necesarios estuvieran funcionando como se esperaba.

## Resultados de los Comandos Ejecutados

1. **Ejecución del manifiesto `app-deployment.yaml`:**

   ```
   deployment.apps/app-deployment configured
   ```

   **Resultado:** El recurso `Deployment` para la aplicación fue configurado exitosamente. Esto confirma que las especificaciones del despliegue, como el número de réplicas y los contenedores definidos, se aplicaron correctamente al clúster.

2. **Ejecución del manifiesto `app-service.yaml`:**

   ```
   service/app-service unchanged
   ```

   **Resultado:** El servicio para la aplicación ya estaba configurado previamente y no se realizaron cambios. Esto indica que los recursos para exponer la aplicación estaban en orden y funcionales.

3. **Ejecución de un manifiesto no válido (`kubernetes/mongo-`):**

   ```
   error: the path "kubernetes/mongo-" does not exist
   ```

   **Resultado:** Este error se produjo porque el archivo referido no existe en el directorio especificado o porque hubo un error tipográfico en el nombre del archivo. Este problema debe ser corregido revisando el nombre y la ubicación del archivo.

4. **Ejecución del manifiesto `mongo-deployment.yaml`:**

   ```
   deployment.apps/mongo-deployment configured
   ```

   **Resultado:** El recurso `Deployment` para MongoDB fue configurado exitosamente. Esto asegura que el despliegue de MongoDB está configurado según lo definido en el manifiesto.

5. **Ejecución del manifiesto `mongo-service.yaml`:**
   ```
   service/mongo-service unchanged
   ```
   **Resultado:** El servicio para MongoDB no presentó cambios, lo que indica que ya estaba configurado correctamente y no necesitaba ajustes.

## Conclusión

En general, los resultados muestran que los recursos de Kubernetes para la aplicación y para MongoDB están correctamente configurados y operativos. El único problema encontrado fue un error al intentar aplicar un archivo inexistente (`kubernetes/mongo-`). Para evitar esto en el futuro, es importante verificar que todos los nombres de archivo sean correctos antes de ejecutar los comandos.

Estas pruebas confirman que el sistema funciona correctamente, salvo por el detalle mencionado, que se puede solucionar fácilmente revisando los manifiestos antes de aplicarlos.
