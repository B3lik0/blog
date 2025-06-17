# 🧪 Blog - Ambiente de Pruebas

> **⚠️ IMPORTANTE: Este es un ambiente de desarrollo y pruebas. NO utilizar en producción.**

Este proyecto es una aplicación de blog desarrollada con Laravel 12 y configurada para ejecutarse en un ambiente de desarrollo utilizando Docker. Es un entorno completamente funcional diseñado para pruebas, desarrollo y aprendizaje.

## 📋 Características

- **Framework**: Laravel 12.x
- **PHP**: 8.2 FPM Alpine
- **Base de datos**: MySQL 8.0.1
- **Servidor web**: Nginx (stable-alpine)
- **Frontend**: Livewire + Flux UI
- **Gestión de dependencias**: Composer
- **Containerización**: Docker + Docker Compose

## 🚀 Inicio Rápido

### Prerrequisitos

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) instalado y ejecutándose
- [Git](https://git-scm.com/) para clonar el repositorio
- Al menos 4GB de RAM disponible
- Puertos 8080, 8090 y 3306 libres en tu sistema

### Instalación

1. **Clonar el repositorio**
   ```bash
   git clone <url-del-repositorio>
   cd blog
   ```

2. **Construir y ejecutar los contenedores**
   ```bash
   docker-compose up --build
   ```

3. **Esperar a que todos los servicios estén listos**
   - El primer inicio puede tomar varios minutos
   - Verifica que todos los contenedores estén corriendo

4. **Acceder a la aplicación**
   - **Aplicación web**: http://localhost:8080
   - **phpMyAdmin**: http://localhost:8090

## 🛠️ Servicios Disponibles

### Aplicación Web
- **URL**: http://localhost:8080
- **Descripción**: Interfaz principal del blog
- **Funcionalidades**:
  - ✅ Registro de usuarios
  - ✅ Inicio de sesión
  - ✅ Dashboard de usuario
  - ✅ Gestión de sesiones

### phpMyAdmin
- **URL**: http://localhost:8090
- **Usuario**: `root`
- **Contraseña**: `root`
- **Base de datos**: `laravel`
- **Descripción**: Interfaz web para gestionar la base de datos MySQL

### Base de Datos MySQL
- **Host**: `localhost` (desde el host) / `mysql` (desde contenedores)
- **Puerto**: 3306
- **Usuario**: `root`
- **Contraseña**: `root`
- **Base de datos**: `laravel`

## 🐳 Arquitectura Docker

### Contenedores

| Servicio | Contenedor | Puerto | Descripción |
|----------|------------|--------|--------------|
| **web** | `laravel_server` | 8080 | Servidor Nginx |
| **app** | `laravel_php` | - | PHP-FPM 8.2 |
| **database** | `laravel_mysql` | 3306 | MySQL 8.0.1 |
| **phpmyadmin** | `laravel_phpmyadmin` | 8090 | Administrador de BD |
| **composer** | `blog-composer-1` | - | Gestor de dependencias |
| **artisan** | `blog-artisan-1` | - | CLI de Laravel |

### Volúmenes
- `./src` → `/var/www/html` (código fuente)
- `./mysql_data` → `/var/lib/mysql` (datos de MySQL)

## 📁 Estructura del Proyecto

```
blog/
├── docker/                     # Configuración Docker
│   ├── dockerfiles/            # Archivos Dockerfile
│   │   ├── nginx.dockerfile
│   │   ├── php.dockerfile
│   │   └── composer.dockerfile
│   ├── nginx/                  # Configuración Nginx
│   │   └── default.conf
│   └── mysql/                  # Configuración MySQL
│       └── .env
├── src/                        # Código fuente Laravel
│   ├── app/
│   ├── resources/
│   ├── routes/
│   └── ...
├── mysql_data/                 # Datos persistentes MySQL (auto-generado)
├── docker-compose.yml          # Configuración de servicios
├── .dockerignore              # Archivos excluidos de Docker
└── README.md                  # Este archivo
```

## 🔧 Comandos Útiles

### Gestión de Contenedores
```bash
# Iniciar servicios
docker-compose up -d

# Detener servicios
docker-compose down

# Ver logs
docker-compose logs -f

# Reconstruir contenedores
docker-compose up --build

# Limpiar todo (¡CUIDADO! Borra datos)
docker-compose down -v
docker system prune -a
```

### Comandos Laravel
```bash
# Ejecutar comandos Artisan
docker-compose exec php php artisan migrate
docker-compose exec php php artisan cache:clear
docker-compose exec php php artisan config:cache

# Acceder al contenedor PHP
docker-compose exec php bash

# Instalar dependencias Composer
docker-compose run composer install
```

### Base de Datos
```bash
# Ejecutar migraciones
docker-compose exec php php artisan migrate

# Revertir migraciones
docker-compose exec php php artisan migrate:rollback

# Seeders (si están configurados)
docker-compose exec php php artisan db:seed

# Acceso directo a MySQL
docker-compose exec mysql mysql -u root -proot laravel
```

## 🧪 Datos de Prueba

### Usuarios de Prueba
Puedes crear usuarios de prueba utilizando el formulario de registro en:
http://localhost:8080/register

### Base de Datos
- La base de datos se inicializa automáticamente
- Las migraciones se ejecutan al iniciar
- Los datos persisten en el volumen `mysql_data`

## 🔒 Configuración de Seguridad (Solo para Desarrollo)

> **⚠️ ESTAS CREDENCIALES SON SOLO PARA DESARROLLO**

- **MySQL Root**: `root` / `root`
- **Aplicación**: Sin configuración de producción
- **Debug**: Habilitado
- **Logs**: Visibles y detallados

## 🐛 Solución de Problemas

### Problemas Comunes

1. **Puerto ocupado**
   ```bash
   # Verificar puertos en uso
   netstat -an | findstr :8080
   netstat -an | findstr :8090
   netstat -an | findstr :3306
   ```

2. **Contenedores no inician**
   ```bash
   # Ver logs detallados
   docker-compose logs [nombre-servicio]
   
   # Reconstruir desde cero
   docker-compose down -v
   docker-compose up --build
   ```

3. **Error de base de datos**
   ```bash
   # Verificar estado de MySQL
   docker-compose exec mysql mysql -u root -proot -e "SHOW DATABASES;"
   
   # Ejecutar migraciones
   docker-compose exec php php artisan migrate
   ```

4. **Limpiar caché**
   ```bash
   docker-compose exec php php artisan cache:clear
   docker-compose exec php php artisan config:clear
   docker-compose exec php php artisan view:clear
   ```

### Logs Importantes
```bash
# Logs de Laravel
docker-compose exec php tail -f storage/logs/laravel.log

# Logs de Nginx
docker-compose logs laravel_server

# Logs de MySQL
docker-compose logs laravel_mysql
```

## 📝 Notas de Desarrollo

- Este ambiente está configurado para **desarrollo y pruebas únicamente**
- Las credenciales están hardcodeadas para facilitar las pruebas
- Los logs están en modo debug para facilitar el desarrollo
- No incluye configuraciones de seguridad para producción
- Los datos persisten entre reinicios en el volumen `mysql_data`

## 🤝 Contribución

Este es un proyecto de pruebas. Si encuentras problemas o tienes sugerencias:

1. Reporta issues detallados
2. Incluye logs relevantes
3. Especifica tu sistema operativo y versión de Docker

## 📄 Licencia

Este proyecto es solo para fines educativos y de desarrollo.

---

**🚨 Recordatorio**: Este es un ambiente de desarrollo. No utilizar en producción sin las configuraciones de seguridad apropiadas.

