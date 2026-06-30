# Backend Ventas (Spring Boot)

Este es el microservicio de Ventas para la plataforma de Innovatech Chile. Fue construido usando Java y Spring Boot.

## Requisitos de Entorno (AWS Secrets)

Para que el backend pueda conectarse a la base de datos (AWS RDS), necesita las siguientes variables de entorno:
- `DB_ENDPOINT`: Endpoint de la base de datos RDS.
- `DB_PORT`: Puerto de la base de datos (usualmente 3306).
- `DB_NAME`: Nombre de la base de datos.
- `DB_USERNAME`: Usuario administrador.
- `DB_PASSWORD`: Contraseña.

En ECS, estas variables deben configurarse en la *Task Definition* utilizando **AWS Secrets Manager** para que no queden expuestas.
El proyecto está configurado para ejecutarse en el puerto **8083**.

## Arquitectura y Despliegue en AWS (EP3)

El backend está empaquetado mediante un `Dockerfile` multi-etapa que primero compila el `.jar` con Maven y luego lo ejecuta utilizando una imagen base de Java 17.

### Integración Continua y Despliegue Continuo (CI/CD)

El repositorio cuenta con un pipeline en GitHub Actions (`.github/workflows/aws-deploy.yml`).

**Flujo Automático:**
Cada vez que se hace un `push` a la rama `main`:
1. Se autentica en AWS mediante GitHub Secrets.
2. Se compila la imagen Docker de Ventas.
3. Se sube la imagen al registro de **Amazon ECR**.
4. Se invoca a **Amazon ECS** para actualizar el servicio con la nueva imagen (Despliegue automático).

**Requisitos Previos en GitHub Secrets:**
- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_SESSION_TOKEN` (Necesario si usas AWS Academy)
