
Esto solo es un copy-paste de mis apuntes de SQL tomados en Notion, para ver la version mas estilizada dirigirse a  
https://lucky-coat-1d0.notion.site/SQL-28240705a289467bba8f2268ec97ff5b?pvs=4
---


Las *DataBase* en SQL funcionan de una manera a la cual se llamada **CRUD,** lo cual son las siglas para Crear, Leer, Actualizar y Borrar (Create, Read, Update and Delete). Por lo cual dividiremos cada uno en estos respectivamente

---

# Crean

### Crear tabla

```sql
INSERT INTO Users (name, email) 
	VALUES ('Chuck', 'csev@umich.edu')
```

### Crear tabla con *primary key*

Para ahorrar espacio y aumentar la velocidad de procesamiento se unas la ***llaves primarias***, esto hara que a partir de ***llaves secundarias*** de otras tablas puedan acceder a los datos de esta. Sabemos que debemos pasar los datos a otra tabla si en una de sus columnas tienen ***strings*** que se repiten mas de una vez.

```sql
CREATE TABLE "Artist" (
    "id" INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL UNIQUE, 
    "name" TEXT)
```

### Crear tabla para relationship databases *Many to Many*

Si necesitamos hacer relaciones ***Many to Many,*** necesitaremos una tabla extra como la siguiente

```sql
CREATE TABLE Member (
    user_id     INTEGER,
    course_id   INTEGER,
		role        INTEGER,
    PRIMARY KEY (user_id, course_id)
);
```

# Leen

# Estructura basica

La estructura basica de SQL es la siguiente:

![Untitled](images/sqlnotion1.png)

- Usa **SELECT** para elegir las columnas que deseas devolver.
- Usa **FROM** para elegir las tablas donde se encuentran las columnas que deseas.
- Usa **WHERE** para filtrar determinada información.

Un ejemplo podria ser el siguiente

```sql
SELECT *
FROM movie_data.movies
WHERE Genre__1 = 'Action'
```

En el anterior ejemplo estaremos buscando dentro de nuestra base de datos, vamos a desglosar lo que nos quiere decir.

## **SELECT**

En **SELECT** tenemos las columnas que queremos devolver, en este caso tenemos un ‘*’, el cual representa que queremos devolver todas las columnas de nuestra tabla.

## **FROM**

Despues en **FROM** tenemos asignado que tabla queremos devolver, en este caso es la tabla de *movies.*

## **WHERE**

Por ultimo, en **WHERE** tenemos nuestra condicional, en este caso colocamos que nos devuelva los casos donde la columna *Genre__1* tenga el valor de *‘Action’.* 

Tambien puedes usarlo algo como ``WHERE Genre__1 LIKE '*Act%'` (en algunas bases de datos el ‘%’ podria ser un ‘*’) l*o cual devolvera todos los que inicien con la palabra ***Act***. 

> Como podemos usar condicionales tambien podremos utilizar operadores binarios como seria el *AND.*
> 

> El equivalente del ***************no es igual que*************** en SQL es el ‘<>’
> 

# Nomenclaturas avanzadas

## SELECT

SELECT *e*s el comando que sirve para decirle a nuestra base de datos que columnas o atributos queremos que nos devuelva en la consulta

### Devolver todas las columnas

```sql
SELECT *
```

### Devolver columanas especificas

```sql
SELECT column1, column2
```

## FROM

****FROM**** es el comando que le dira a nuestra base de datos de que tabla queremos que nos devuelva los datos

### Tabla desde donde vendran nuestros datos

```sql
FROM movie_data.movies
```

## WHERE

*****WHERE***** es un metodo de agregar condicionales, donde solo nos devolvera datos con valores especificos

### Condicionales de seleccion de datos

```sql
WHERE Genre__1 = 'Action'
```

### Condicional con operadores binarios

Aqui vemos el uso de los operadores de *AND* y *OR*, ademas del *igual*, *mayor que* y *diferente que.*

```sql
WHERE Genre__1 = 'Action' AND calification > 8.5
```

```sql
WHERE Genre__1 <> 'Action' OR Genre__2 = 'Adventure'
```

<aside>
💡 Toma en cuenta que apesar de haber escrito los anteriores comandos como individuales no funcionaran a menos que tengas una estructura como:
SELECT *
FROM movie_data.movies
WHERE Genre__1 = 'Action' —Aqui podrias cambiarlo por otros como ORDER BY

</aside>

## ORDER BY

*****ORDER BY***** es un comando que nos ayudara a ordenar los valores de nuestra consulta, podemos especificarle si lo queremos de manera ascendente o descendente.

### Ordenar datos devueltos de tabla

```sql
SELECT * 
FROM Users 
ORDER BY email
```

### Ordenar datos devueltos de tabla en descendente

```sql
SELECT * 
FROM Users 
ORDER BY name DESC --Tambien puede ser ASC
```

### Ordenar datos con jerarquias

Puedes hacer que haya jerarquias, donde se ordenen primero ciertos datos y que los demas datos tambien tengan cierto orden de la siguiente manera

```sql
SELECT User.name, roles.position, Course.title
FROM User JOIN Member JOIN Course JOIN roles
ON Member.user_id = User.id AND Member.course_id = Course.id AND Member.role = roles.id

ORDER BY Course.title, roles.position DESC, User.name
/*En este ORDER BY podemos ver que se ordenaran con una 
jerarquia de izquiera a derecha, donde primero se ordenara 
por cursos, luego por roles y en ultimo por roles*/
```

## JOIN

### Unir dos bases de datos relacionadas ***many to one***

Si queremos unir dos tablas, las cuales estan relacionadas por *keys *****podemos usar ***join,*** lo que hara esto es unir las tablas solo si las ***id*** coinciden. A este tipo de uniones se les llama ***many to one*.**

```sql
select Album.title, Artist.name 
from Album join Artist 
on Album.artist_id = Artist.id

--**select {}** nos dice que columnas veremos
--**from {} join {}** nos dice que tablas uniremos
--**on** condiciona que nuestras tablas se unan segun las ID

/*Si no colocaramos la clausula ****on**** las dos tablas se 
unirian haciendo todas las combinaciones posibles*/
```

### Union compleja de bases de datos relacionadas

```sql
SELECT track.title, Artist.name, Album.title, Genre.name
FROM Album JOIN Artist JOIN Genre JOIN Track
on Album.artist_id = Artist.id AND 
	Track.album_id = Album.id AND Track.genre_id = Genre.id
```

# Actualizan

## Actualizar datos

```sql
UPDATE Users SET name="Charles" WHERE email='csev@umich.edu'
```

# Eliminan

## Eliminar datos de tabla

```sql
DELETE FROM Users WHERE email='ted@umich.edu'
```

---

# Extras

## Comentarios

Ademas podemos agregar comentarios a nuestras consultas, usando ‘/*’ y ‘*/’ para un bloque de comentarios y ‘*—*’ para un comentario de una linea.

![Untitled](images/sqlnotion2.png)

## Constantes

Tambien llamadas **alias,** son nombres que se les asigna a las columnas para que sea mas facil trabajar con ellos. Para hacer esto usamos la clausula ***AS.***

![Untitled](images/sqlnotion3.png)

Aqui podemos ver como *field1* ahora lo llamamos *last_name* para que sea mas facil y reconocible trabajar con el

<aside>
💡 Cuando quieras colocar mas de un comando en SQL debera tener ‘;’ al final de cada linea

</aside>