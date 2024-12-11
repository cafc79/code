# Contenedores, Kubernetes y GKE

## 1. Contenedores en Docker
### Construcción de la Imagen
Asegúrate de que el [Dockerfile](https://pages.github.com/) está en la raíz del directorio de tu proyecto, al igual que el archivo [.jar](https://pages.github.com/)
1. En la terminal, navega al directorio y Construye la Imagen ejecutando:
```ruby
> docker build -t mi-app .
```

> [!NOTE]
> - [ ] -t mi-app imagen: Especifica el nombre de la imagen.
> - [ ] .: Indica que Docker debería buscar el Dockerfile en el directorio actual.
 
### Despliegue de un Contenedor

2. Ejecuta el Contenedor: Crea y ejecuta el contenedor con el siguiente comando:
```ruby
> docker run -d -p 3000:3000 mi-app
```

3. Prueba el Despliegue: Si la aplicación está expuesta en el puerto 3000, abre un navegador o usa curl para verificar:
```ruby
> curl http://localhost:3000
```
> [!IMPORTANT]
> Recuerda que el mapeo de puertos, debe coincidir entre el puerto que expone el contenedor, con el puerto en el cual del host al que se hace referencia en el request


## 2.  Kubernetes en GKE
Para desplegar una aplicación en Google Kubernetes Engine (GKE) usando un manifiesto de Kubernetes, necesitas seguir estos pasos clave. Detallo cada etapa desde la preparación del entorno hasta el despliegue:

### 1. Configuración Inicial de GKE
#### 1.  Autenticarte en Google Cloud Platform (GCP):

Asegúrate de tener configurado el CLI de GCP (gcloud).
Inicia sesión:
```ruby
gcloud auth login
```

Selecciona el proyecto donde desplegarás la aplicación:
```ruby
gcloud config set project [PROJECT_ID]
```

#### 2.  Crear un Clúster de GKE:
Configura el clúster de GKE:
```ruby
gcloud container clusters create [CLUSTER_NAME] \
  --num-nodes=3 \
  --region=[REGION]
```

Ejemplo:
```ruby
gcloud container clusters create my-cluster --num-nodes=3 --region=us-central1
```

#### 3.  Configurar credenciales locales:

Descarga el archivo de credenciales del clúster para interactuar con Kubernetes:
```ruby
gcloud container clusters get-credentials [CLUSTER_NAME] --region [REGION]
```

Verifica la conectividad al clúster:
```ruby
kubectl get nodes
```

Esto mostrará los nodos del clúster si todo está configurado correctamente.

### 2. Preparar el Manifiesto de Kubernetes
Asegúrate de que tu manifiesto YAML esté listo y correctamente configurado. Por ejemplo:

Manifiesto de Despliegue y Servicio
```ruby
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
  labels:
    app: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: gcr.io/[PROJECT_ID]/my-image:latest
        ports:
        - containerPort: 80
```
```ruby
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  type: LoadBalancer
```

### 3. Construir y Subir la Imagen al Registro de Contenedores
Crear la imagen Docker:
Desde tu directorio de proyecto:
```ruby
docker build -t gcr.io/[PROJECT_ID]/my-image:latest .
```

Subir la imagen al Container Registry:

Asegúrate de autenticar Docker con GCP:
```ruby
gcloud auth configure-docker
```
Sube la imagen:
```ruby
docker push gcr.io/[PROJECT_ID]/my-image:latest
```

### 4. Desplegar la Aplicación en GKE
Aplicar el manifiesto:
Usa el comando kubectl apply para desplegar tu aplicación:
```ruby
kubectl apply -f [MANIFEST_FILE].yaml
```

Ejemplo:
```ruby
kubectl apply -f my-app.yaml
```

Verifica el despliegue:
Asegúrate de que los pods estén funcionando:
```ruby
kubectl get pods
```

Revisa el servicio para obtener la dirección IP externa:
```ruby
kubectl get service my-service
```

### 5. Probar la Aplicación
Una vez que el servicio esté expuesto, accede a la aplicación desde la dirección IP externa proporcionada por el servicio LoadBalancer.

### 6. Limpieza (Opcional)
Si quieres eliminar los recursos desplegados:
```ruby
kubectl delete -f [MANIFEST_FILE].yaml
```
Si no necesitas el clúster de GKE, elimínalo para evitar costos innecesarios:
```ruby
gcloud container clusters delete [CLUSTER_NAME] --region [REGION]
```

### Mejores Prácticas
1. Gestionar Versiones de la Imagen:
Usa etiquetas de versión específicas (my-image:v1.0.0) en lugar de latest para garantizar reproducibilidad.

2. Configurar Recursos y Límites:
Define los recursos de CPU y memoria para los contenedores:
```ruby
resources:
  requests:
    memory: "256Mi"
    cpu: "500m"
  limits:
    memory: "512Mi"
    cpu: "1"
```

3. Probar en Ambientes Locales:
Usa Minikube o Kind para validar el manifiesto antes de desplegar en GKE.

5. Habilitar Autoescalado:
Configura el autoescalador en tu clúster de GKE:
```ruby
gcloud container clusters update [CLUSTER_NAME] \
  --enable-autoscaling \
  --min-nodes=1 \
  --max-nodes=5
```ruby

Monitorizar la Aplicación:
5.  Usa Google Cloud Monitoring para rastrear el rendimiento y estado de tu aplicación.

> [!NOTE]
> Useful information that users should know, even when skimming content.

> [!TIP]
> Helpful advice for doing things better or more easily.

> [!IMPORTANT]
> Key information users need to know to achieve their goal.

> [!WARNING]
> Urgent info that needs immediate user attention to avoid problems.

> [!CAUTION]
> Advises about risks or negative outcomes of certain actions.> [!NOTE]
> Useful information that users should know, even when skimming content.
