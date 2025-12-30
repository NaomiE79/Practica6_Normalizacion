# Práctica 6 Normalización de bases de datos

# 🖼 Dataset 1: Netflix Movies and TV Shows

## 🎯 Ejercicio 1: Selección y Análisis de Datasets

```
1. Estructura original:
Columnas: 12
Registros: 8807

Tipos de datos presentes:
INTEGER
VARCHAR
DATE
TEXT
```

![Ejemplo de 5 registros representativos:](docs/imagenes/img1.png)

```
2. Identificación de problemas de normalización:
Columnas director, cast, country, date_added, listed_in tienen múltiples valores (violación de 1FN)
Redundancia de datos en type, date_added, duration, rating, relase_year
Posibles anomalías (inserción, actualización, eliminación)
No se puede registrar un país sin show
Actualizar el nombre de un director implica modificar varias filas
Eliminar un show puede borrar también la información del directo
```

3. Diagrama de dependencias funcionales:

          |---------|
          | show_id |
          |---------|
               ↓
          |------+-- ----+--------+------+--------+----------+---------+--------+----------+-----------+-------------|
          | type | title |director| cast |country | date_add | release | rating | duration | listed_in | description |
          |------+-------+--------+------+--------+----------+---------+--------+----------+-----------+-------------|

## 🎯 Ejercicio 2: Proceso de Normalización Manual

**Estructura original**

| show_id| type | title | director | cast | country | date_added | release_year | rating | duration | listed_in | description |
|--------|------|-------|----------|------|---|--------|------|-------|----------|------|---|

**Estructura resultante**

| show_id| type | title | date_added | release_year | rating | duration | description |
|--------|------|---------|------|---|--------|------|-------|

| show_id|  director |
|------|-------|

| show_id| cast | 
|------|---|

| show_id| country | 
|------|---|

| show_id| listed_in | 
|--------|------|

**Ejemplo de datos en la estructura original**

| show_id| type | title | director | cast | country | date_added | release_year | rating | duration | listed_in | description |
|--------|------|-------|----------|------|---|--------|------|-------|----------|------|---|

| s5 | TV Show | Kota Factory	| Mayur More | Jitendra Kumar, Ranjan Raj, Alam Khan, Ahsaas Channa, Revathi Pillai, Urvi Singh, Arun Kumar | India | September 24, 2021 | 2021 | TV-MA	| 2 Seasons | International TV Shows, Romantic TV Shows, TV Comedies | In a city of coaching centers known to train Indiaâ€™s finest collegiate minds, an earnest but unexceptional student and his friends navigate campus life |

**Ejemplo de datos en la estructura resultante**

| show_id| type | title | date_added | release_year | rating | duration | description |
|--------|------|---------|------|---|--------|------|-------|
|s5 | TV Show | Kota Factory	| September 24, 2021 | 2021 | TV-MA	| 2 Seasons | In a city of coaching centers known to train Indiaâ€™s finest collegiate minds, an earnest but unexceptional student and his friends navigate campus life |

| show_id|  director |
|------|-------|
|s5 | Mayur More |

| show_id| cast | 
|------|---|
| s5 | Jitendra Kumar |
| s5 | Ranjan Raj |
| s5 | Alam Khan |
| s5 | Ahsaas Channa |
| s5 | Revathi Pillai |
| s5 | Urvi Singh |
| s5 | Arun Kumar |

| show_id| country | 
|------|---|
| s5 | India |

| show_id| listed_in | 
|--------|------|
| s5 | International TV Shows |
| s5 | Romantic TV Shows |
| s5 | TV Comedies |
 
## ⚙️ Ejercicio 3: Automatización del Proceso de Normalización

# 📁 **Estructura del Proyecto**

```
normalizacion-db/
│
├── data/
│   ├── raw/                    # Datasets originales
│   │   ├── dataset1.csv
│   │   ├── dataset2.csv
│   │   └── dataset3.csv
│   │
│   └── normalized/             # Datos normalizados exportados
│       ├── dataset1/
│       ├── dataset2/
│       └── dataset3/
│
├── scripts/
│   ├── normalize_dataset1.py   # Script específico dataset 1
│   ├── normalize_dataset2.py   # Script específico dataset 2
│   ├── normalize_dataset3.py   # Script específico dataset 3
│   └── utils.py                # Funciones auxiliares reutilizables
│
├── sql/
│   ├── ddl/                    # Scripts de creación de tablas
│   │   ├── dataset1_schema.sql
│   │   ├── dataset2_schema.sql
│   │   └── dataset3_schema.sql
│   │
│   └── dml/                    # Scripts de inserción de datos
│       ├── dataset1_data.sql
│       ├── dataset2_data.sql
│       └── dataset3_data.sql
│
├── docs/
│   ├── analisis_original.md
│   ├── proceso_normalizacion.md
│   └── diagramas_er/
│
├── requirements.txt            # Dependencias Python
├── README.md                   # Documentación del proyecto
└── Dockerfile                  # Opcional para Docker

```

## 🐳 Ejercicio 4: Implementación con Docker

Nombre de mi contenedor: db_final

![Texto alternativo para la imagen](docs/imagenes/contenedor.png)

El nombre de la imagen en Docker es: mi_proyecto_db

![Texto alternativo para la imagen](docs/imagenes/imagen_docker.png)

![Texto alternativo para la imagen](docs/imagenes/install_pandas.png)

![Texto alternativo para la imagen](docs/imagenes/corre_py1.png)

![Texto alternativo para la imagen](docs/imagenes/carpeta_normalizado1_llena.png)

# 🖼 Dataset 2: E-commerce Sales Data

## 🎯 Ejercicio 1: Selección y Análisis de Datasets

```
1. Estructura original:
Columnas: 8
Registros: 25900

Tipos de datos presentes:
INTEGER
VARCHAR
DATE
TIME
FLOAT
```

Ejemplo de 5 registros representativos:

![Texto alternativo para la imagen](docs/imagenes/img2.png)

```
2. Identificación de problemas de normalización:
Columna InvoiceDate tienen múltiples valores (violación de 1FN)
Atributos como InvoiceDate, CustomerID, Country dependen solo de InvoiceNo, no de StockCode (violación de 2FN)
Country depende de CustomerID, que depende de InvoiceNo (violación de 3FN)
Redundancia de datos en Quantity, InvoiceDate, UnitPrice, Country y customerID
Posibles anomalías (inserción, actualización, eliminación)
No se puede insertar un cliente sin compra
Actualizar el país implica modificar varias filas
Eliminar el último producto de una factura borra también la información del cliente
```

3. Diagrama de dependencias funcionales:

![Texto alternativo para la imagen](docs/imagenes/diagrama2.png)

## 🎯 Ejercicio 2: Proceso de Normalización Manual

**Estructura original**

| InvoiceNo | StockCode | Description | Quantity | InvoiceDate | UnitPrice | CustomerID | Country |
|----------|-----------|-------------|----------|-------------|-----------|------------|---------|

**Estructura resultante**

### Tabla factura

| InvoiceNo | InvoiceDate | InvoiceHour | CustomerID | Country |
|--------|--------|---------|--------|-------|

### Tabla productos

| StockCode |  Description |
|------|-------|

### Tabla detalleVenta

| InvoiceNo | StockCode | Quantity | UnitPrice |
|------|---|------|---|

**Ejemplo de datos en la estructura original**

| InvoiceNo | StockCode | Description | Quantity | InvoiceDate | UnitPrice | CustomerID | Country |
|----------|-----------|-------------|----------|-------------|-----------|------------|---------|
| 536374 | 21258 | VICTORIAN SEWING BOX LARGE | 32 | 12/01/2010  09:09:00 a. m. | 10.95 | 15100 | United Kingdom |

**Ejemplo de datos en la estructura resultante**

### Tabla factura

| InvoiceNo | InvoiceDate | InvoiceHour | CustomerID | Country |
|--------|--------|---------|--------|-------|
| 536374 | 12/01/2010 | 09:09:00 | 15100 | United Kingdom |

### Tabla productos

| StockCode |  Description |
|------|-------|
| 21258 | VICTORIAN SEWING BOX LARGE |

### Tabla detalleVenta

| InvoiceNo | StockCode | Quantity | UnitPrice |
|------|---|------|---|
| 536374 | 21258 | 32 | 10.95 |

## 🐳 Ejercicio 4: Implementación con Docker

![Texto alternativo para la imagen](docs/imagenes/corre_py2.png)

![Texto alternativo para la imagen](docs/imagenes/carpeta_normalizado2_llena.png)

