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
** 🔗 Uniones entre Tablas (JOINS)**
Los JOINS se utilizan para combinar filas de dos o más tablas basándose en una columna relacionada entre ellas (normalmente una Foreign Key). Es lo que permite, por ejemplo, unir un ID de cliente con su nombre real guardado en otra tabla.
| Tipo de JOIN | Funcionamiento | Resultado |
| :--- | :--- | :--- |
| **INNER JOIN** | Devuelve solo las filas con coincidencias en ambas tablas. | Solo registros "con pareja". |
| **LEFT JOIN** | Devuelve todas las filas de la tabla izquierda y las coincidencias de la derecha. | Todo lo de la izquierda + nulos a la derecha. |
| **RIGHT JOIN** | Devuelve todas las filas de la tabla derecha y las coincidencias de la izquierda. | Todo lo de la derecha + nulos a la izquierda. |
| **CROSS JOIN** | Combina cada fila de la primera tabla con todas las filas de la segunda. | Producto cartesiano (todas las combinaciones). |

Unión Estricta (INNER JOIN)
Es el más utilizado. Solo muestra los registros donde el valor del "puente" existe en ambas tablas. Si un dato no tiene pareja en la otra tabla, no aparece.
```sql
-- Trae el nombre del colaborador y el nombre de su departamento
SELECT colaboradores.nombre, departamentos.nombre_depto
FROM colaboradores
INNER JOIN departamentos ON colaboradores.departamento_id = departamentos.id;
```
** Unión Prioritaria Izquierda (LEFT JOIN)**
Asegura que no se pierda ningún dato de la tabla principal (la que va después del FROM). Si no hay coincidencia en la tabla de la derecha, mostrará NULL.
```sql
-- Trae TODOS los clientes y, si tienen, sus servicios contratados
SELECT clientes.nombre, servicios_microsoft.nombre_servicio
FROM clientes
LEFT JOIN servicios_microsoft ON clientes.id = servicios_microsoft.cliente_id;
```
Unión con Alias de Tabla
Cuando usamos JOINS, los nombres de las tablas pueden ser largos. Usamos alias (letras cortas) para que el código sea más limpio y fácil de leer.
```sql
-- 'c' es el alias para colaboradores y 'd' para departamentos
SELECT c.nombre, d.nombre_depto
FROM colaboradores AS c
INNER JOIN departamentos AS d ON c.departamento_id = d.id;
```

- 📝 Nota sobre FULL OUTER JOIN
En MySQL, la sentencia FULL OUTER JOIN (que trae todo de ambas tablas aunque no coincidan) no existe de forma nativa. Para lograr este resultado, los ingenieros solemos usar una combinación de LEFT JOIN, RIGHT JOIN y la sentencia UNION. ¡Es un buen truco para tu sección de notas!


### 📊 Agrupamiento y Funciones de Agregación
Estas sentencias permiten realizar cálculos sobre múltiples filas para devolver un único valor de resumen. Son la base para generar reportes y estadísticas.
### 📊 Tabla de Funciones de Agregación

Estas funciones realizan un cálculo sobre un conjunto de valores y devuelven un solo valor. Son esenciales para generar estadísticas y resúmenes.

| Función | Propósito Técnico | Ejemplo de Uso |
| :--- | :--- | :--- |
| **`COUNT()`** | Cuenta el número total de registros o valores no nulos. | `COUNT(id_usuario)` |
| **`SUM()`** | Suma todos los valores de una columna numérica. | `SUM(monto_pago)` |
| **`AVG()`** | Calcula el promedio aritmético de los valores. | `AVG(precio_licencia)` |
| **`MIN()`** | Identifica el valor mínimo de un conjunto. | `MIN(fecha_ingreso)` |
| **`MAX()`** | Identifica el valor máximo de un conjunto. | `MAX(costo_total)` |

Agrupar Resultados (GROUP BY)
Se utiliza para agrupar filas que tienen los mismos valores en columnas específicas. Es obligatorio usarlo cuando seleccionamos una columna normal junto a una función de agregación.
```sql
-- Cuenta cuántos colaboradores hay en cada departamento
SELECT departamento_id, COUNT(*) 
FROM colaboradores 
GROUP BY departamento_id;
```
Filtrar Grupos (HAVING)
Es similar al WHERE, pero se usa exclusivamente para filtrar los resultados después de haber sido agrupados. Se utiliza con funciones de agregación.
```sql
-- Muestra departamentos que tienen más de 5 colaboradores
SELECT departamento_id, COUNT(*) 
FROM colaboradores 
GROUP BY departamento_id 
HAVING COUNT(*) > 5;
```
Cálculo de Totales y Promedios
Permite obtener métricas financieras o de rendimiento de forma rápida.
```sql
-- Obtiene el total de ingresos y el promedio de ventas por servicio
SELECT servicio_nombre, SUM(monto) AS total, AVG(monto) AS promedio
FROM facturacion
GROUP BY servicio_nombre;
```
📝 Diferencia Clave: WHERE vs HAVING
Para que tu documentación sea impecable, aquí tienes una nota técnica fundamental:

- WHERE: Filtra filas antes de que ocurra el agrupamiento. No puede usar funciones como SUM() o COUNT().

- HAVING: Filtra los grupos después de que se han realizado los cálculos de agregación.
