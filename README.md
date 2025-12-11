# Guía de Instalación y Despliegue de la Aplicación GeoUrbis
## 1. Requisitos Previos
Antes de ejecutar la aplicación, instala:

### PostgreSQL + PostGIS
https://www.postgresql.org/download  
https://postgis.net/install/

### GeoServer
https://geoserver.org/

### Visual Studio Code + Live Server
Extensión Live Server:
https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer

## 2. Configurar Base de Datos en PostgreSQL/PostGIS

1. Crear una base de datos:

```sql
CREATE DATABASE sigweb;
```

2. Activar PostGIS:

```sql
CREATE EXTENSION postgis;
```

3. Importar el archivo incluido de la base de datos del repositorio:

```
base_de_datos.sql
```

El archivo base_de_datos.sql incluye siguientes tablas necesarias:
- Barrios
- Manzanas
- Tipo de actividad 
- Construcciones
- Instituciones
- Escenarios deportivos y culturales

## 3. Configuración de GeoServer

### 3.1 Crear Workspace
Nombre: `sigweb`  
Namespace URI: `http://sigweb`

### 3.2 Crear DataStore (PostGIS)
Ir a:

```
Data → Stores → Add new Store → PostGIS
```

Configurar:

- host: localhost  
- port: 5432  (o el numero de puerto de tu base de datos)
- database: sigweb  
- schema: public  
- user/pass según tu instalación  

### 3.3 Publicar las capas
Publicar como **WMS + WFS**:

| Capa | Tipo |
|------|------|
| Barrios | Polygon |
| Manzanas | Polygon |
| Construcciones | Polygon |
| Perimetro | Polygon|
| Tipo de actividad | Polygon|
| Instituciones | Point |
| Escenarios deportivos | Point |

### 3.4 Estilos SLD
Los estilos SLD están en:

```
/geoserver_styles/
```

Cargar cada uno mediante:

```
GeoServer → Styles → Upload
```

## 4. Ejecutar la Aplicación Web

1. Abrir el proyecto en **Visual Studio Code**  
2. Abrir el archivo:

```
/index.html
```

3 Click derecho → **Open with Live Server**

La aplicación se ejecutará en:

```
http://127.0.0.1:5500/
```
> Live Server es recomendado para cargar módulos ES6 y JSON locales.

## 5. Conexiones esperadas

### GeoServer:
```
http://localhost:8080/geoserver/
```

### WMS:
```
http://localhost:8080/geoserver/sigweb/wms
```

### WFS:
```
http://localhost:8080/geoserver/sigweb/ows?service=WFS
```

### Archivos JSON locales:
```
/data/Json/
```

### Íconos personalizados:
```
/img/iconos/
```

## 6. Problemas Comunes

### No aparecen capas
- Revisar conexión con PostGIS  
- Revisar workspace  
- Revisar estilos  

### Error CORS en GeoServer
Editar:

```
GEOSERVER_HOME/webapps/geoserver/WEB-INF/web.xml
```

### JSON no carga
Debe ejecutarse con **Live Server**, no abriendo el HTML directamente.

## 7 Estructura del Proyecto

```
├── css/
├── data/
│   └── Json/
├── img/
│   └── iconos/
├── js/
│   ├── layers/
│   ├── filters/
│   ├── search/
│   ├── main.js
├── geoserver_styles/
├── Html
│   └── index.html
├── README.md
└── base_de_datos
```



