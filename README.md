# SQL_Script
Bienvenido a mi repositorio de documentación personal de SQL. Este espacio está dedicado a consolidar conocimientos sobre el lenguaje de consulta estructurado, cubriendo desde la definición de datos hasta consultas complejas.
### 🚀 Objetivo
El propósito de este repositorio es servir como una bitácora técnica de aprendizaje y consulta rápida, documentando sentencias aplicables en entornos de bases de datos relacionales como MySQL.

###Definición de estructura (DDL)
Crear una Base de Datos
Se utiliza para generar el contenedor principal donde residirán todas nuestras tablas y datos.
```sql
CREATE DATABASE nombre_de_la_bd;
```
Modificar Propiedades de la Base de Datos Se utiliza principalmente para cambiar el conjunto de caracteres (charset) o la colación (cómo se comparan los textos).
```sql
ALTER DATABASE nombre_de_tu_bd CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```
- **📝 Nota sobre el Renombrado de Bases de Datos
Importante: En MySQL no existe una sentencia directa como RENAME DATABASE. Para cambiar el nombre de una base de datos, la práctica estándar es exportar los datos (dump), crear una nueva base de datos con el nombre deseado e importar los datos en ella. Esto se hace para proteger la integridad de los esquemas y las conexiones activas.**

Eliminar una Base de Datos Borra la base de datos completa y todo su contenido de forma irreversible.
```sql
DROP DATABASE nombre_de_tu_bd
CHARACTER SET utf8mb4 
COLLATE utf8mb4_unicode_ci;
```
- **CHARACTER SET y COLLATE: ¿Qué hace?: Define qué "abecedario" usa la base de datos (utf8mb4 permite emojis y caracteres especiales) y cómo se comparan (COLLATE define si "A" es igual a "a").**
En caso de haber creado la BD sin CHARACTER SET y COLLATE se puede modificar con la tabla con la siguiente sentencia:
```sql
ALTER DATABASE nombre_de_tu_bd 
CHARACTER SET utf8mb4 
COLLATE utf8mb4_unicode_ci;
```
Si solo una tabla necesita un soporte de caracteres especial, se puede ajustar individualmente sin afectar al resto de la base de datos.
```sql
ALTER TABLE usuarios 
CONVERT TO CHARACTER SET utf8mb4 
COLLATE utf8mb4_unicode_ci;
```

🛡️ Tabla Comparativa de Constraints
Restricciones de Integridad (Constraints)
Las restricciones o constraints son las reglas fundamentales que definimos en las columnas de nuestras tablas para garantizar que la información sea precisa, confiable y coherente.
Podemos imaginarlas como las validaciones de un formulario: su función es actuar como una barrera de seguridad que impide la entrada de "datos basura" (información incompleta, registros duplicados o valores fuera de lógica). Es una buena práctica aplicarlas en campos críticos para el negocio, como identificadores o correos electrónicos, manteniendo la flexibilidad en campos opcionales donde la información no es estrictamente obligatoria.

| Constraint | Función Principal | Ejemplo de Uso | Aplicación Común |
| :--- | :--- | :--- | :--- |
| **NOT NULL** | Prohíbe valores vacíos en la columna. | `nombre VARCHAR(50) NOT NULL` | Nombres, RUT, contraseñas. |
| **UNIQUE** | Impide que existan dos valores iguales. | `email VARCHAR(100) UNIQUE` | Correos, nombres de usuario. |
| **PRIMARY KEY** | Identificador único de cada registro. | `id INT PRIMARY KEY` | IDs de tablas, códigos internos. |
| **FOREIGN KEY** | Relaciona una tabla con otra. | `REFERENCES clientes(id)` | Conectar pedidos con clientes. |
| **DEFAULT** | Asigna un valor si no se ingresa uno. | `estado VARCHAR(20) DEFAULT 'Activo'` | Fechas, estados de cuenta. |
| **CHECK** | Valida una condición lógica específica. | `CHECK (edad >= 18)` | Precios, rangos de edad, sueldos. |

Ejemplo de aplicación de Constraints
```sql
CREATE TABLE servicios_microsoft (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre_servicio VARCHAR(100) NOT NULL,
    licencia_id VARCHAR(50) UNIQUE,
    costo DECIMAL(10,2) CHECK (costo > 0),
    estado VARCHAR(20) DEFAULT 'Operativo'
);
```
Gestión de Tablas
Crear una Tabla
Define la estructura de una entidad, especificando los nombres de columna, el tipo de dato que almacenarán y sus restricciones (como llaves primarias).
```sql
CREATE TABLE colaboradores (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(50) NOT NULL,
    cargo VARCHAR(50),
    fecha_ingreso DATE
);
```
Renombrar una Tabla: Cambia el nombre de una tabla existente sin perder los datos que contiene.
```sql
RENAME TABLE nombre_antiguo TO nombre_nuevo;
```
Vaciar una Tabla (TRUNCATE): A diferencia de DELETE, esta sentencia borra todos los datos de la tabla y reinicia los contadores (como el auto_increment), pero mantiene la estructura.
```sql
TRUNCATE TABLE usuarios;
```
Eliminar una Tabla
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
**Modificación de Columnas (ALTER TABLE)**:Aquí es donde ajustamos el diseño de una tabla que ya ha sido creada.

Agregar una nueva Columna Añade un campo adicional a una tabla existente.
```sql
ALTER TABLE usuarios ADD COLUMN correo VARCHAR(100);
```
**Modificar el Tipo de Dato de una Columna** Cambia las propiedades de una columna (por ejemplo, aumentar el límite de caracteres).
```sql
ALTER TABLE usuarios MODIFY COLUMN nombre VARCHAR(150) NOT NULL;
```
Renombrar una Columna Cambia el nombre de un campo específico.
```sql
ALTER TABLE usuarios CHANGE COLUMN nombre nombre_completo VARCHAR(150);
```
**Eliminar una Columna** Quita permanentemente un campo y todos los datos que contenía de la tabla.
```sql
ALTER TABLE usuarios DROP COLUMN correo;
```
### Consultar Datos (Básico)
Recupera información de una tabla. El uso del asterisco * indica que queremos traer todas las columnas disponibles
```sql
SELECT * FROM colaboradores;
```

Consultar Columnas Específicas
En lugar de traer toda la tabla, podemos solicitar solo los campos que necesitamos para optimizar la consulta.
```sql
SELECT nombre, cargo FROM colaboradores;
```
Actualizar Datos
Modifica los valores de registros que ya existen. Importante: Siempre se debe acompañar de una condición (WHERE) para no afectar a todos los registros de la tabla.
```sql
UPDATE colaboradores 
SET cargo = 'Senior Global Admin' 
WHERE id = 1;
```
Eliminar Registros
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
Uso de IN (Filtro por Lista)
Es mucho más eficiente que usar muchos OR. Se usa para buscar registros que coincidan con cualquiera de los elementos de una lista.
```sql
-- Selecciona colaboradores que trabajen en cualquiera de estas empresas
SELECT * FROM colaboradores 
WHERE empresa IN ('Vedata', 'Aurys', 'MFS');
```
