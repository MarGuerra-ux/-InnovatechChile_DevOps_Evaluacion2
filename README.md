# InnovatechChile - DevOps Evaluación 2

## Descripción
Sistema de gestión de despachos para InnovatechChile. Permite consultar órdenes de compra y despacho a través de una arquitectura de microservicios desplegada en AWS.

## Arquitectura
- **Frontend:** React + Vite + Tailwind CSS, desplegado en EC2 pública (nginx, puerto 80)
- **Backend:** Spring Boot (Java 17), desplegado en EC2 privada (puerto 8080)
- **Base de datos:** MySQL 8, con volumen persistente mysql_data
- **CI/CD:** GitHub Actions con build y push a DockerHub, deploy automático vía SSH

## Infraestructura AWS
| Componente | IP Pública | IP Privada |
|---|---|---|
| EC2 Frontend | 3.222.207.216 | 10.0.1.99 |
| EC2 Backend | - | 10.0.2.136 |

## Requisitos
- Docker y Docker Compose
- Cuenta DockerHub
- AWS EC2 (Amazon Linux 2023)
- Java 17, Node.js 20

## Despliegue

### Frontend (EC2 Pública)
`\ash
docker pull onichanmarco/frontend-devops:latest
docker rm -f frontend
docker run -d -p 80:80 --name frontend onichanmarco/frontend-devops:latest
`\

### Backend (EC2 Privada)
`\ash
cd ~/app
docker-compose down
docker-compose up -d
`\

## CI/CD
El pipeline se activa al hacer push a la rama \deploy\.

- **cicd-frontend.yml**: Build y push de imagen frontend a DockerHub, deploy en EC2 frontend
- **cicd-backend.yml**: Build y push de imagen backend a DockerHub, deploy en EC2 backend

### Secretos requeridos en GitHub
| Secret | Descripción |
|---|---|
| DOCKERHUB_USERNAME | Usuario DockerHub |
| DOCKERHUB_TOKEN | Token DockerHub |
| FRONTEND_EC2_IP | IP pública EC2 frontend |
| BACKEND_EC2_IP | IP privada EC2 backend |
| EC2_SSH_KEY | Clave privada PEM |

## Imágenes Docker
- Frontend: \onichanmarco/frontend-devops:latest\
- Backend: \onichanmarco/backend-devops:latest\

## Endpoints API
| Método | Endpoint | Descripción |
|---|---|---|
| GET | /api/v1/ventas | Listar todas las ventas |
| GET | /api/v1/ventas/{id} | Obtener venta por ID |
| POST | /api/v1/ventas | Crear nueva venta |
| PUT | /api/v1/ventas/{id} | Actualizar venta |
| DELETE | /api/v1/ventas/{id} | Eliminar venta |

## Integrantes
- MarGuerra-ux
