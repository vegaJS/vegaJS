# 👋 Bienvenido a mi sitio

Soy un desarrollador curioso. Este es mi pequeño centro de enlaces.

| Sección     | Enlace                          | Descripción                       |
|-------------|----------------------------------|-----------------------------------|
| 🧠 Apuntes  | [Ver apuntes](./apuntes/)       | Mis notas sobre temas de estudio |
| 💼 Portfolio| [Ver proyectos](./portfolio/)    | Algunos proyectos personales     |
| 📓 Blog     | [Ir al blog](./blog/)            | Donde escribo lo que aprendo     |

# 📑 Tabla de Contenido – Guía de Oracle SQL

1. [Fundamentos de SQL](#1-fundamentos-de-sql)  
2. [Consultas básicas](#2-consultas-básicas)  
3. [Filtros y condiciones](#3-filtros-y-condiciones)  
4. [Ordenar y limitar resultados](#4-ordenar-y-limitar-resultados)  
5. [Funciones de fila](#5-funciones-de-fila)  
6. [Agrupación y agregación](#6-agrupación-y-agregación)  
7. [Subconsultas](#7-subconsultas)  
8. [Joins y relaciones entre tablas](#8-joins-y-relaciones-entre-tablas)  
9. [Combinación de resultados](#9-combinación-de-resultados)  
10. [Definición de datos (DDL)](#10-definición-de-datos-ddl)  
11. [Manipulación de datos (DML)](#11-manipulación-de-datos-dml)  
12. [Vistas](#12-vistas)  
13. [Índices](#13-índices)  
14. [Secuencias y sinónimos](#14-secuencias-y-sinónimos)  
15. [Seguridad básica](#15-seguridad-básica)  
16. [Consultas jerárquicas](#16-consultas-jerárquicas)  
17. [Expresiones y funciones avanzadas](#17-expresiones-y-funciones-avanzadas)


# 📘 Guía de Oracle SQL

---


## 1. Fundamentos de SQL

En esta sección aprenderás los conceptos esenciales para comenzar a trabajar con Oracle SQL: cómo se estructura una consulta, qué tipos de datos existen y qué operadores puedes utilizar para construir condiciones y realizar cálculos.

---

### 📌 Sintaxis básica

Una consulta SQL en Oracle suele escribirse de esta forma:

```sql
SELECT columnas
FROM tabla
[WHERE condición]
[GROUP BY columnas]
[HAVING condición_agregada]
[ORDER BY columnas]
[FETCH FIRST n ROWS ONLY];
```

Cada cláusula tiene su función:

- `SELECT`: Elige las columnas que quieres visualizar.
- `FROM`: Indica la tabla (o tablas) de donde se extraen los datos.
- `WHERE`: Filtra las filas antes de agruparlas.
- `GROUP BY`: Agrupa las filas con valores similares.
- `HAVING`: Filtra los grupos después de agrupar.
- `ORDER BY`: Ordena los resultados.
- `FETCH FIRST`: Limita la cantidad de filas devueltas (alternativa moderna a `ROWNUM`).

#### 🔄 Orden lógico de ejecución

Oracle ejecuta internamente las cláusulas en un orden diferente al que se escriben. Este es el orden lógico real:

| Paso | Cláusula        | Función principal                                 |
|------|------------------|---------------------------------------------------|
| 1    | `FROM`           | Carga y combina las tablas necesarias.           |
| 2    | `WHERE`          | Filtra las filas antes del agrupamiento.         |
| 3    | `GROUP BY`       | Agrupa las filas por una o más columnas.         |
| 4    | `HAVING`         | Filtra los grupos resultantes del `GROUP BY`.    |
| 5    | `SELECT`         | Selecciona y calcula los valores de salida.      |
| 6    | `ORDER BY`       | Ordena los resultados finales.                   |
| 7    | `FETCH FIRST` / `ROWNUM` | Limita la cantidad de filas mostradas. |

##### 🧠 Ejemplo completo:

```sql
SELECT department_id, AVG(salary) AS promedio
FROM employees
WHERE job_id = 'SA_REP'
GROUP BY department_id
HAVING AVG(salary) > 5000
ORDER BY promedio DESC
FETCH FIRST 3 ROWS ONLY;
```

---

### 🗃️ Tipos de datos en Oracle

Oracle soporta distintos tipos de datos para almacenar texto, números, fechas, y otros formatos:

#### 🔹 Tipos de texto

| Tipo           | Descripción                                     |
|----------------|-------------------------------------------------|
| `CHAR(n)`      | Cadena de longitud fija.                        |
| `VARCHAR2(n)`  | Cadena de longitud variable.                    |
| `NCHAR(n)`     | Igual que `CHAR`, pero para caracteres Unicode. |
| `NVARCHAR2(n)` | Igual que `VARCHAR2`, pero Unicode.             |
| `CLOB`         | Texto largo (hasta 4GB).                        |
| `NCLOB`        | CLOB Unicode.                                   |

#### 🔸 Tipos numéricos

| Tipo              | Descripción                                       |
|-------------------|---------------------------------------------------|
| `NUMBER(p,s)`     | Número con precisión (`p`) y escala (`s`).        |
| `FLOAT(p)`        | Número en coma flotante (IEEE estándar).          |
| `BINARY_FLOAT`    | Número en coma flotante de precisión simple.      |
| `BINARY_DOUBLE`   | Número en coma flotante de precisión doble.       |

#### 🟢 Tipos de fecha y hora

| Tipo           | Descripción                                            |
|----------------|--------------------------------------------------------|
| `DATE`         | Fecha con hora (hasta segundos).                      |
| `TIMESTAMP`    | Fecha con fracciones de segundo.                      |
| `TIMESTAMP WITH TIME ZONE` | Incluye zona horaria.                    |
| `TIMESTAMP WITH LOCAL TIME ZONE` | Ajusta automáticamente la zona.    |
| `INTERVAL YEAR TO MONTH`   | Rango de tiempo en años/meses.           |
| `INTERVAL DAY TO SECOND`   | Rango de tiempo en días/segundos.        |

#### 🟣 Tipos binarios y otros

| Tipo      | Descripción                                |
|-----------|--------------------------------------------|
| `BLOB`    | Datos binarios (imágenes, archivos, etc.)  |
| `RAW(n)`  | Datos binarios sin procesar.               |
| `LONG`    | Cadena de caracteres muy larga (obsoleto). |
| `ROWID`   | Identificador único de fila en una tabla.  |
| `UROWID`  | Versión extendida de `ROWID`.              |

---

### ➕ Operadores lógicos y aritméticos

#### 🔸 Operadores aritméticos

Se utilizan para realizar cálculos sobre columnas numéricas:

| Operador | Descripción       | Ejemplo            |
|----------|-------------------|--------------------|
| `+`      | Suma              | `salario + 100`    |
| `-`      | Resta             | `salario - 50`     |
| `*`      | Multiplicación    | `cantidad * precio`|
| `/`      | División          | `salario / 12`     |

#### 🔹 Operadores lógicos

Permiten construir condiciones en las cláusulas `WHERE`, `HAVING`, o incluso dentro de expresiones:

| Operador | Descripción                                | Ejemplo                                 |
|----------|--------------------------------------------|-----------------------------------------|
| `=`      | Igualdad                                   | `department_id = 10`                    |
| `<>`     | Distinto de                                 | `job_id <> 'SA_REP'`                    |
| `>`      | Mayor que                                   | `salary > 3000`                         |
| `<`      | Menor que                                   | `hire_date < TO_DATE('2020-01-01', 'YYYY-MM-DD')` |
| `>=`     | Mayor o igual que                           | `bonus >= 1000`                         |
| `<=`     | Menor o igual que                           | `salary <= 5000`                        |
| `BETWEEN`| Dentro de un rango (incluye los extremos)   | `salary BETWEEN 3000 AND 5000`          |
| `IN`     | Coincide con alguno de una lista            | `job_id IN ('SA_REP', 'IT_PROG')`       |
| `LIKE`   | Coincide con un patrón (usa `%` o `_`)      | `first_name LIKE 'A%'`                  |
| `IS NULL`| Es nulo                                      | `commission_pct IS NULL`                |
| `IS NOT NULL` | No es nulo                             | `manager_id IS NOT NULL`                |
| `AND`    | Todas las condiciones son verdaderas        | `salary > 3000 AND department_id = 50`  |
| `OR`     | Al menos una condición es verdadera         | `department_id = 10 OR department_id = 20` |
| `NOT`    | Niega una condición                         | `NOT job_id = 'AD_VP'`                  |

---

Con estos fundamentos puedes comenzar a construir consultas sólidas, manejar tipos de datos correctamente y aplicar condiciones lógicas y cálculos básicos.



---

## 2. Consultas básicas
- `SELECT` simple
- `DISTINCT`
- Alias de columnas (`AS`)
- Expresiones y funciones básicas
- Concatenación (`||`)
- Valores nulos

---

## 3. Filtros y condiciones
- `WHERE`
- Operadores: `=`, `<>`, `>`, `<`, `BETWEEN`, `LIKE`, `IN`, `IS NULL`
- Combinación lógica: `AND`, `OR`, `NOT`

---

## 4. Ordenar y limitar resultados
- `ORDER BY`
- `ROWNUM`, `FETCH FIRST`

---

## 5. Funciones de fila
- Funciones numéricas (`ROUND`, `MOD`, `FLOOR`, etc.)
- Funciones de texto (`UPPER`, `LOWER`, `SUBSTR`, etc.)
- Funciones de fecha (`SYSDATE`, `ADD_MONTHS`, `MONTHS_BETWEEN`, etc.)
- Funciones de conversión (`TO_CHAR`, `TO_DATE`, `CAST`)

---

## 6. Agrupación y agregación
- `GROUP BY`
- Funciones agregadas (`SUM`, `AVG`, `MIN`, `MAX`, `COUNT`)
- `HAVING`

---

## 7. Subconsultas
- En cláusulas `SELECT`, `FROM`, `WHERE`
- Subconsultas correlacionadas
- Uso de `IN`, `ANY`, `ALL`, `EXISTS`

---

## 8. Joins y relaciones entre tablas
- `INNER JOIN`
- `LEFT`, `RIGHT`, `FULL OUTER JOIN`
- `CROSS JOIN`
- Sintaxis antigua con `(+)`

---

## 9. Combinación de resultados
- `UNION`, `UNION ALL`
- `INTERSECT`, `MINUS`

---

## 10. Definición de datos (DDL)
- `CREATE TABLE`
- Restricciones: `NOT NULL`, `UNIQUE`, `CHECK`, `DEFAULT`, `PRIMARY KEY`, `FOREIGN KEY`
- `ALTER TABLE`
- `DROP TABLE`

---

## 11. Manipulación de datos (DML)
- `INSERT`
- `UPDATE`
- `DELETE`
- Transacciones: `COMMIT`, `ROLLBACK`, `SAVEPOINT`

---

## 12. Vistas
- `CREATE VIEW`
- `WITH CHECK OPTION`

---

## 13. Índices
- `CREATE INDEX`
- Índices únicos y compuestos

---

## 14. Secuencias y sinónimos
- `CREATE SEQUENCE`, `NEXTVAL`, `CURRVAL`
- `CREATE SYNONYM`

---

## 15. Seguridad básica
- Creación de usuarios
- Roles y privilegios
- `GRANT`, `REVOKE`

---

## 16. Consultas jerárquicas
- `CONNECT BY`, `START WITH`
- `LEVEL`
- `SYS_CONNECT_BY_PATH`

---

## 17. Expresiones y funciones avanzadas
- `CASE`, `DECODE`
- Funciones analíticas (`RANK`, `ROW_NUMBER`, etc.)
- CTEs con `WITH`

---
## 1. Fundamentos de SQL

Este apartado cubre los conceptos fundamentales para empezar a trabajar con Oracle SQL, incluyendo la estructura básica de las sentencias, los tipos de datos más utilizados y los operadores esenciales.

---

### 📌 Sintaxis básica

Una consulta SQL en Oracle suele seguir esta estructura general:

```sql
SELECT columna1, columna2
FROM nombre_tabla
WHERE condición
ORDER BY columna1;


Paso | Cláusula | Descripción
1 | FROM | Se identifica la(s) tabla(s) origen y se combinan si hay joins.
2 | WHERE | Se filtran las filas según las condiciones (antes de cualquier agrupación).
3 | GROUP BY | Se agrupan las filas por una o varias columnas.
4 | HAVING | Se filtran los grupos formados por GROUP BY.
5 | SELECT | Se seleccionan las columnas y se aplican funciones de agregación.
6 | ORDER BY | Se ordenan los resultados finales.
7 | FETCH / ROWNUM | Se limita el número de resultados (opcional).
