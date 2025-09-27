# N8N con Docker y PostgreSQL - Configuración Completa

Este proyecto configura una instancia de N8N con base de datos PostgreSQL utilizando Docker Compose, incluyendo autenticación básica y configuración segura.

## Grupo 5
- Roberto Maldonado
- David Saa
- Anthony Loyaga
- Nelson Lopez

## Generación de Contraseñas Seguras

Todas las contraseñas del archivo `.env` fueron generadas usando un generador de contraseñas seguro:

**Generador utilizado:** https://www.avast.com/random-password-generator#pc

<img width="1555" height="624" alt="image-4" src="https://github.com/user-attachments/assets/f2e1c116-ad08-4442-ba98-5fbf152c0c32" />

## Estructura del Proyecto

```
├── docker-compose.yml     # Configuración de servicios Docker
├── .env                   # Variables de entorno (credenciales)
└── README.md             # Este archivo
```

## Configuración

### Variables de Entorno (.env)

```properties
# Configuración de PostgreSQL para n8n
N8N_DB_NAME=n8n_database
N8N_DB_USER=n8n_user
N8N_DB_PASSWORD=PASSWORD
```

## Sistemas de Autenticación

### Importante: Basic Auth Deprecado

En versiones anteriores, al desplegar N8N en contenedores se configuraban **variables de ambiente** para habilitar **BASIC AUTH** como mecanismo de autenticación inicial.  

Desde **n8n 1.0+** (incluyendo la versión **1.15.1**), este método fue **deprecado** y reemplazado por un **sistema de gestión de usuarios** más seguro y robusto.

### Sistema de Autenticación Actual

#### **Owner Account** (Sistema Moderno de N8N)
- **Propósito**: Primera cuenta de administrador y sistema de usuarios integrado
- **Cuándo se configura**: En el primer acceso a N8N (proceso interactivo obligatorio)
- **Función**: Administrador principal de workflows, usuarios y configuraciones
- **Características**:
  - Sistema de usuarios completo
  - Gestión de permisos granular
  - Autenticación robusta
  - Posibilidad de múltiples usuarios

### Flujo de Autenticación Actual

1. **Navegador** → `http://localhost:5678`
<img width="980" height="925" alt="image" src="https://github.com/user-attachments/assets/7241f545-59de-4ded-a160-92a19ccde498" />

3. **N8N carga** → Pantalla de configuración inicial (si es primera vez)
4. **Owner Account** → Configuración **obligatoria** del administrador principal:
   - Email
   - Nombre
   - Apellido
   - Contraseña
5. **N8N Dashboard** → Acceso completo a la aplicación

## Acceso a la Aplicación

1. **Iniciar servicios**
```bash
docker-compose up -d
```
1. **URL**: http://localhost:5678
2. **Primera vez**: Configuración **obligatoria** del Owner Account:
   - Email
   - Nombre
   - Apellido
   - Contraseña
     
<img width="1128" height="826" alt="image" src="https://github.com/user-attachments/assets/8a05ac26-815e-4646-9bf7-010a3b8f54c5" />


3. **Accesos posteriores**: Login con las credenciales del Owner Account

<img width="1181" height="829" alt="image" src="https://github.com/user-attachments/assets/fdc524b3-1f43-479a-bb2b-226f575b4001" />

4. **Dashboard inicial n8n**

<img width="1911" height="863" alt="image" src="https://github.com/user-attachments/assets/e37fe50b-d924-4a33-b6b1-c981eda44d4b" />


## Comandos de Uso

### Iniciar servicios
```bash
docker-compose up -d
```

### Ver logs
```bash
docker-compose logs -f n8n
```

### Detener servicios
```bash
docker-compose down
```

### Reiniciar servicios
```bash
docker-compose restart
```

## Servicios Incluidos

### N8N
- **Puerto**: 5678
- **Versión**: 1.15.1
- **Base de datos**: PostgreSQL
- **Autenticación**: Sistema de usuarios integrado (Owner Account)

### PostgreSQL
- **Puerto**: 5432 (interno)
- **Versión**: 15
- **Base de datos**: n8n_database
- **Usuario**: n8n_user


## Notas Importantes

- **Backup**: Los datos se almacenan en `n8n_data/` y `postgres_n8n_data/`
- **Seguridad**: Nunca exponer el archivo `.env` en repositorios públicos
- **Producción**: Considerar autenticación más robusta (OAuth, SSO)
- **Red**: Los servicios se comunican a través de `n8n_network`

## Conclusiones

- **Despliegue completo y seguro**:
La configuración mediante Docker Compose permite levantar de manera consistente y reproducible la instancia de N8N junto con PostgreSQL, asegurando persistencia de datos y aislamiento de servicios. Esto facilita tanto el desarrollo como la puesta en producción.

- **Seguridad en credenciales**:
Todas las contraseñas se generan mediante un generador seguro, garantizando que no se utilicen contraseñas débiles. Además, se enfatiza la importancia de no exponer el archivo .env en repositorios públicos.

- **Persistencia de datos**:
Los datos de N8N y PostgreSQL se almacenan en volúmenes dedicados (n8n_data/ y postgres_n8n_data/), lo que asegura que la información sobreviva a reinicios o actualizaciones de contenedores.

- **Facilidad de operación**:
La inclusión de comandos claros para iniciar, detener, reiniciar y monitorear los servicios (docker-compose up, down, restart, logs) simplifica la administración diaria de la plataforma.
