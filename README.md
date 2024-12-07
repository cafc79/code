# Contenedores, Kubernetes y GKE

## Construcción de la Imagen
Asegúrate de que el [Dockerfile](https://pages.github.com/) está en la raíz del directorio de tu proyecto, al igual que el archivo [.jar](https://pages.github.com/)
1. En la terminal, navega al directorio y Construye la Imagen ejecutando:
> docker build -t mi-app .
> [!NOTE]
- [ ] -t mi-app imagen: Especifica el nombre de la imagen.
- [ ] .: Indica que Docker debería buscar el Dockerfile en el directorio actual.
 
## Despliegue de un Contenedor

2. Ejecuta el Contenedor: Crea y ejecuta el contenedor con el siguiente comando:
> docker run -d -p 3000:3000 mi-app
3. Prueba el Despliegue: Si la aplicación está expuesta en el puerto 3000, abre un navegador o usa curl para verificar:
> curl http://localhost:3000
![image](https://github.com/user-attachments/assets/73840019-6d25-406c-aa75-a5430ea5b708)


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

> [!TIP]
> Helpful advice for doing things better or more easily.

> [!IMPORTANT]
> Key information users need to know to achieve their goal.

> [!WARNING]
> Urgent info that needs immediate user attention to avoid problems.

> [!CAUTION]
> Advises about risks or negative outcomes of certain actions.
