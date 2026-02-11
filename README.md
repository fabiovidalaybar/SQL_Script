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
