# Práctica 6 Normalización de bases de datos
## 🎯 Ejercicio 1: Selección y Análisis de Datasets

### 🖼 Dataset 1: Netflix Movies and TV Shows

1. Estructura original:
Columnas: 12
Registros: 8807

Tipos de datos presentes:
INTEGER
VARCHAR
DATE
TEXT

Ejemplo de 5 registros representativos:

<img width="1919" height="878" alt="image" src="https://github.com/user-attachments/assets/acc0bc9e-596e-44e7-a424-a7ae6c686204" />

2. Identificación de problemas de normalización:
Columnas director, cast, country, date_added, listed_in tienen múltiples valores (violación de 1FN)
Redundancia de datos en type, date_added, duration, rating, relase_year
Posibles anomalías (inserción, actualización, eliminación)
No se puede registrar un país sin show
Actualizar el nombre de un director implica modificar varias filas
Eliminar un show puede borrar también la información del directo

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

## 🐳 Ejercicio 4: Implementación con Docker

• Capturas de pantalla de contenedores en ejecución


• Evidencia de normalización exitosa dentro de Docker



 

# 📁 **Estructura del Proyecto**

```
mi_restaurante/
│
├── app/
│   ├── __init__.py
│   ├── main.py            # Rutas Flask
│   ├── database.py        # Conexión ORM (SQLAlchemy)
│   ├── models.py          # Modelos ORM
│   ├── static/
│   │   └── style.css
│   └── templates/
│       ├── base.html
│       ├── index.html
│       ├── login.html
│       ├── register.html
│       ├── menu.html
│       ├── mi_pedido.html
│       └── dashboard.html
│
├── .env
├── .gitignore
├── requirements.txt
└── README.md
```
