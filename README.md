# SQL_Script
Bienvenido a mi repositorio de documentación personal de SQL. Este espacio está dedicado a consolidar conocimientos sobre el lenguaje de consulta estructurado, cubriendo desde la definición de datos hasta consultas complejas.
### 🚀 Objetivo
El propósito de este repositorio es servir como una bitácora técnica de aprendizaje y consulta rápida, documentando sentencias aplicables en entornos de bases de datos relacionales como MySQL.

1. Crear una Base de Datos
Se utiliza para generar el contenedor principal donde residirán todas nuestras tablas y datos.
```powershell
CREATE DATABASE sistema_gestion;
```
2. Crear una Tabla
Define la estructura de una entidad, especificando los nombres de columna, el tipo de dato que almacenarán y sus restricciones (como llaves primarias).
```sql
CREATE TABLE colaboradores (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(50) NOT NULL,
    cargo VARCHAR(50),
    fecha_ingreso DATE
);
```
3. Eliminar una Tabla
Borra una tabla de forma permanente, incluyendo toda su estructura y los datos que contenía. Se debe usar con precaución.
```sql
DROP TABLE colaboradores;
```

### ✍️ Manipulación de Datos (DML)
4. Insertar Registros
Permite agregar nuevas filas de información a una tabla existente. Es importante que los valores coincidan con el orden y tipo de dato de las columnas.
```sql
INSERT INTO colaboradores (nombre, cargo, fecha_ingreso) 
VALUES ('Juan Pérez', 'Administrador de Sistemas', '2024-01-15');
```

### 5. Consultar Datos (Básico)
Recupera información de una tabla. El uso del asterisco * indica que queremos traer todas las columnas disponibles
```sql
SELECT * FROM colaboradores;
```

6. Consultar Columnas Específicas
En lugar de traer toda la tabla, podemos solicitar solo los campos que necesitamos para optimizar la consulta.
```sql
SELECT nombre, cargo FROM colaboradores;
```
7. Actualizar Datos
Modifica los valores de registros que ya existen. Importante: Siempre se debe acompañar de una condición (WHERE) para no afectar a todos los registros de la tabla.
```sql
UPDATE colaboradores 
SET cargo = 'Senior Global Admin' 
WHERE id = 1;
```
8. Eliminar Registros
Borra filas específicas de una tabla según la condición indicada.
```sql
DELETE FROM colaboradores 
WHERE id = 1;
```

### 🔍 Filtrado y Control de Resultados
| Operador | Descripción | Ejemplo de Uso |
| :--- | :--- | :--- |
| **`=`** | Igual a | `WHERE nombre = 'Christian'` |
| **`<>`** o **`!=`** | Diferente de | `WHERE empresa != 'Vedata'` |
| **`>`** | Mayor que | `WHERE id > 10` |
| **`<`** | Menor que | `WHERE precio < 5000` |
| **`>=`** | Mayor o igual que | `WHERE fecha >= '2026-01-01'` |
| **`<=`** | Menor o igual que | `WHERE edad <= 36` |

### 🔍 Consultas con Filtros (DML)
Esta tabla resume las formas más comunes de filtrar datos en SQL para obtener resultados precisos.

| Tipo de Filtro | Descripción Técnica | Ejemplo de Sentencia |
| :--- | :--- | :--- |
| **Exacto** | Busca una coincidencia total con el valor. | `WHERE ciudad = 'Santiago'` |
| **Excluyente** | Trae todo excepto lo que coincida con el valor. | `WHERE estado != 'Inactivo'` |
| **Rango** | Filtra valores dentro de un límite (min y max). | `WHERE edad BETWEEN 18 AND 36` |
| **Patrón** | Busca textos que contengan una parte específica. | `WHERE nombre LIKE 'Tif%'` |
| **Lista** | Compara el campo contra varios valores posibles. | `WHERE id IN (1, 5, 10)` |
| **Nulidad** | Filtra registros que tienen campos vacíos. | `WHERE email IS NULL` |
| **Múltiple** | Combina dos o más condiciones obligatorias. | `WHERE cargo = 'Admin' AND id > 5` |

Ejemplo**
18. Uso de IN (Filtro por Lista)
Es mucho más eficiente que usar muchos OR. Se usa para buscar registros que coincidan con cualquiera de los elementos de una lista.
```sql
-- Selecciona colaboradores que trabajen en cualquiera de estas empresas
SELECT * FROM colaboradores 
WHERE empresa IN ('Vedata', 'Aurys', 'MFS');
```
