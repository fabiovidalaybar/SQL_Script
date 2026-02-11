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
```powershell
CREATE TABLE colaboradores (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(50) NOT NULL,
    cargo VARCHAR(50),
    fecha_ingreso DATE
);
```
3. Eliminar una Tabla
Borra una tabla de forma permanente, incluyendo toda su estructura y los datos que contenía. Se debe usar con precaución.
```powershell
DROP TABLE colaboradores;
```

### ✍️ Manipulación de Datos (DML)
4. Insertar Registros
Permite agregar nuevas filas de información a una tabla existente. Es importante que los valores coincidan con el orden y tipo de dato de las columnas.
```powershell
INSERT INTO colaboradores (nombre, cargo, fecha_ingreso) 
VALUES ('Juan Pérez', 'Administrador de Sistemas', '2024-01-15');
```

### 5. Consultar Datos (Básico)
Recupera información de una tabla. El uso del asterisco * indica que queremos traer todas las columnas disponibles
```powershell
SELECT * FROM colaboradores;
```

6. Consultar Columnas Específicas
En lugar de traer toda la tabla, podemos solicitar solo los campos que necesitamos para optimizar la consulta.
```powershell
SELECT nombre, cargo FROM colaboradores;
```
7. Actualizar Datos
Modifica los valores de registros que ya existen. Importante: Siempre se debe acompañar de una condición (WHERE) para no afectar a todos los registros de la tabla.
```powershell
UPDATE colaboradores 
SET cargo = 'Senior Global Admin' 
WHERE id = 1;
```
8. Eliminar Registros
Borra filas específicas de una tabla según la condición indicada.
```powershell
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
