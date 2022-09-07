Scaffold-DbContext "Server=SeguridadWebdb2022.mssql.somee.com; Database=SeguridadWebdb2022; User ID=Jose2022_SQLLogin_2; Password=awqh561; Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -DataAnnotations -Force

## **DATA TYPES**
Cadena de textos
```SQL
VARCHAR(n); -- 'Alex Quix'
```
Fechas y tiempos
```SQL
DATE; -- '08-31-2022'
DATETIME; -- '08/31/2022 08:15:05'
```
Numeros
```SQL
BIT; -- 1 o 0
SMALLINT; -- 1 hasta 32,767
INT; -- 1 hasta 2,147,487,647
DECIMAL(n,a); -- 1521.00
MONEY; -- 1,500.00
```

</br>
</br>


## **DQL(DATA QUERY LANGUAGE)**
---
## **SELECT**
---
```SQL
SELECT * FROM Alumnos
```

</br>
</br>


## **DDL(DATA DEFINITION LANGUAGE)**
---
## **CREATE**
---

**CREAR UNA BASE DE DATOS**
```SQL
CREATE DATABASE Escuela
```

**CREAR UNA TABLA**
```SQL
CREATE TABLE Alumno(
	Id INT PRIMARY KEY IDENTITY(1,1), -- Es un id auto incrementable
	Nombre VARCHAR(20) NULL,
	Apellido VARCHAR(20) NOT NULL,
	Edad INT NULL,
	Sexo VARCHAR(6)
)
```
**LLAVE FORANEA**
```SQL
CREATE TABLE Grado(
	IdGrado INT PRIMARY KEY
	-- ...
)
CREATE TABLE Alumno(
	-- ...
	IdGrado INT FOREIGN KEY REFERENCES Grado(IdGrado)
)
```
**CREAR UNA  VISTA**
```SQL
CREATE VIEW vPersonAddress
AS
SELECT A.AddressLine1, a.City, s.Name FROM Person.Address AS a
INNER JOIN Person.StateProvince AS s
	ON a.StateProvinceID = s.StateProvinceID
```
Obtener los datos de la vista
```SQL
SELECT * FROM vPersonAddress
```
**CREAR UN TRIGGER**
```SQL
CREATE TRIGGER trActualizarStock -- nombre del trigger
	ON FacturaDetalle -- la tabla a vigilar
	FOR INSERT -- con que accion se activara el trigger
AS
BEGIN
	-- ...
END
```

</br>

## **ALTER**
---

**AGREGAR UNA COLUMNA A UNA TABLA EXISTENTE**
```SQL
ALTER TABLE Alumno
ADD Direccion VARCHAR(100) NOT NULL;
```

**CAMBIAR UNA COLUMNA EXISTENTE**
```SQL
ALTER TABLE Alumno
ALTER COLUMN Direccion VARCHAR(10);
```

**ELIMINAR UNA COLUMNA**
```SQL
ALTER TABLE Alumno
DROP COLUMN Nombre, Apellido, Edad, Sexo;
```
**LLAVES FORANEAS**
</BR>

Agrega una llave foranea a una columna existente
```SQL
ALTER TABLE Alumno
ADD CONSTRAINT FK_AlumnoIdGrado FOREIGN KEY(IdGrado) REFERENCES Grado(IdGrado);
```
Elimina la llave foranea
```SQL
ALTER TABLE Alumno
DROP CONSTRAINT FK_AlumnoIdGrado;
```



</br>
</br>



## **DML(DATA MANIPULATION LANGUAGE)**
---
## **UPDATE**
---
**ACTUALIZAR LOS DATOS DE UNA TABLA**
```SQL
UPDATE Alumno
-- Las datos a actualizar
SET Nombre = 'AlexQuix',
	Edad = 19
WHERE Id = 1; -- Solo se actualizara en este id
```

</br>

## **INSERT**
---
**INSERTAR DATOS EN LA TABLA**
```SQL
-- Seleccionar las columnas a insertar datos
INSERT INTO Alumno (Nombre, Apellido, Edad, Sexo)
-- Los valores que se insertaran
VALUES (
	'Alex',
	'Quix',
	21,
	'Hombre'
)
```

</br>

## **DELETE**
---
**ELIMINAR DATOS EN LA TABLAS**
```SQL
DELETE Alumno
WHERE Id = 7;S
```

</br>
</br>

## **CLAUSULAS**
---
> Nota: estos ejemplos usan la base de datos AdventWorks2019 de Microsoft
### **WHERE**
```SQL
SELECT * FROM Person.Person
WHERE FirstName = 'Roberto';
```
### **ORDER BY**
Desendente
```SQL
SELECT * FROM Person.Person
ORDER BY FirstName DESC;
```
Ascendente
```SQL
SELECT * FROM Person.Person
ORDER BY FirstName ASC;
```
### **GROUP BY**
Agrupa los valores repetidos, trabajan de la mano con las funciones de agregacion
```SQL
SELECT ToCurrencyCode, SUM(AverageRate) TotalRate FROM Sales.CurrencyRate
GROUP BY ToCurrencyCode
```
### **TOP**
Numero de registros que se mostraran
```SQL
SELECT TOP 10 * FROM Person.Person;
```
### **DISTINCT**
Muestra los registros que son distintos
```SQL
SELECT DISTINCT FirstName FROM Person.Person
```

</br>
</br>

## **FUNCTIONS**
---
## **STRINGS**
---
### **CONCAT**
```SQL
SELECT CONCAT(FirstName, ' ', LastName) [Nombre Completo] 
	FROM Person.Person;
```
### **LOWER**
```SQL
SELECT LOWER(FirstName) FROM Person.Person; 
```
### **UPPER**
```SQL
SELECT UPPER(FirstName) FROM Person.Person; 
```
</br>


## **NUMBER**
---
### **AVG**
```SQL
SELECT AVG(SafetyStockLevel) FROM Production.Product;
```
### **SUM**
```SQL
SELECT SUM(SafetyStockLevel) FROM Production.Product;
```

</br>
</br>

## **OPERADORES LOGICOS**
---
- OR
- AND
- NOT
- \> o >=
- < o <=
- LIKE
- BETWEEN
```SQL
SELECT * FROM Person.Person
WHERE BusinessEntityID > 20 AND BusinessEntityID < 30
	OR FirstName = 'Ken';
```
Seria lo mismo a
```SQL
SELECT * FROM Person.Person
WHERE BusinessEntityID BETWEEN 20 AND 30
```
Coincidencia de palabras
```SQL
-- Inicio de una palabra
SELECT FirstName FROM Person.Person
WHERE FirstName LIKE 'K%';

-- Final de una palabra
SELECT FirstName FROM Person.Person
WHERE FirstName LIKE '%ron';

-- Si contiene la siguiente palabra
SELECT FirstName FROM Person.Person
WHERE FirstName LIKE '%alt%';
```


</br>
</br>

# **MISC**
## **STORED PROCEDURE**
---
**CREAR UN PROCEDIMIENTO ALMACENADO**
```SQL
CREATE PROCEDURE spGetPagination 
AS
BEGIN
	SELECT TOP 30 * FROM Person.Password
	WHERE BusinessEntityID BETWEEN 1 AND 30;
END
```
**PROCEDIMIENTO ALMACENADO CON VARIABLES**
```SQL
CREATE PROCEDURE spGetPagination 
	@Init INT,
	@End INT
AS
BEGIN
	SELECT TOP 30 * FROM Person.Password
	WHERE BusinessEntityID BETWEEN @Init AND @End;
END
```
**EJECUTAR UN PROCEDIMIENTO ALMACENADO**
```SQL
EXEC spGetPagination 30, 100;
```

</br>
</br>


## **UNIR TABLAS**
**SE MOSTRARAN LOS DATOS QUE COINCIDAN**
```SQL
SELECT * FROM Alumno AS a
INNER JOIN Grado AS g 
	ON a.IdGrado = g.Id; -- las llaves
```
**LOS DATOS QUE NO COINCIDAN SE RELLENARAN CON UN NULL**
```SQL
SELECT * FROM Alumno AS a
RIGHT JOIN Grado AS g 
	ON a.IdGrado = g.Id;
```

</br>
</br>


## **VARIABLES**
```SQL
-- declara variables
DECLARE @Precio INT = 100;

-- cambiar valor
SET @Precio = 200;

-- Mostrar 
SELECT @Precio;
```

</br>
</br>


## **CONDICIONALES**
```SQL
@Pelicula VARCHAR(50) = 'Batman';
IF(@Pelicula LIKE 'Bat%')
BEGIN
	SELECT 'TRUE';
END
	ELSE
BEGIN
	SELECT 'FALSO';
END
```