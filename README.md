# MongoDB

_Memoria de comandos para MongoDB_

## Tabla de Contenido

1. [ Creación de usuarios ](#creacion)
2. [ Manejo de la Base ](#manejo)
3. [ Insert ](#insertar)
4. [ Find ](#buscar)
5. [ Update ](#actualizar)
6. [ Delete ](#borrar)
7. [ Proyecciones ](#proyeccion)
8. [ Agregaciones ](#agregacion)
9. [ Índices ](#indices)
10. [ Importar y Exportar ](#importar)

<a name="creacion"></a>
## Creación de usuarios

1. Se requiere primero hacer uso de la db admin:

```
use admin
```

2. Ejecuta el siguiente comando, el cuál crea un usuario que puede administrar cualquier base de datos, adecuar los permisos dependiendo del uso a la BD:

```
db.createUser({
    user: "<user>",
    pwd: "<password>",
    roles: [ { role: "userAdminAnyDatabase", db: "admin" },
    { role: "dbAdminAnyDatabase", db: "admin" },
    { role: "readWriteAnyDatabase", db: "admin" } ]
})
```

Tambien se puede crear un usuario por medio de la ejecucion de comando:

```
db.runCommand({
    createUser : "user_read_write",
    pwd : "myultrastrongsecretpassword",
    customData : {},
    roles : [ { } ]
});
```

_Se puede consultar la documentación de MongoDB [Enable Access Control](https://docs.mongodb.com/manual/tutorial/enable-authentication/) para mayor información._

<a name="manejo"></a>
## Manejo de la BD

Para usar y crear una BD:

```
use database
```

Para crear una coleccion. Nota: Los documentos no pueden tener mas de 16MB.

```
db.createCollection("")
```

Para listar los posibles comandos que podemos ejecutar en una coleccion:

```
db.collection.help()	
```

<a name="insertar"></a>
## Insert

Insertar un solo documento JSON	:

```
db.collection.insertOne({json})	
```

Insertar con un Arreglo de JSONs:

```
"db.getCollection(<""coleccion"">).insert([
    {<objeto JSON del documento>},
    {<objeto JSON del documento>},
    {<objeto JSON del documento>}
])"	
```

Insertar con una instrucción un arreglo de JSONs:

```
db.collection.insertMany([{ ... }, { ... }])	
```

<a name="buscar"></a>
## Find


Muestra un documento de la colección:	

```
db.collection.findOne()	
```

Muestra todos los documentos que cumplan con el filtro:	

```
db.collection.find( { status:"A" } )
```

Cuenta los registros:

```
db.collection.find( { status:"A" } ).count()
```

Lee los documentos en base a los criterios de consulta, si no se especifican los <campos devueltos> se mostraran todos. Puede incluir lo siguiente en el filtro [Query](https://docs.mongodb.com/manual/reference/operator/query/):

```
db.<colección>.find(	
     {<criterios de consulta>},		
     {<campos devueltos>}	
).limit(<n max de documentos devueltos>)
```

Ejemplo: Busca en la coleccion los mayores de 18, solo mostrar los campos name y address, solo traer 5:

```
db.collection.find(
     { age: {$gt:18} },
     { name:1, address:1 }
).limit(5)
```

Tambien se puede usar el comando para traer la colección:

```
db.getCollection( 'user' ).find( { age : { $eq:5 } } )		
```

<a name="actualizar"></a>
## Update


Actualiza un documento:

```
db.collection.updateOne(
	{_id:ObjectID('444c4d4..d4')},
	{$set:{qty:130}}
)
```

Se recibe un JSON para actualizar:
	
```
db.<colección>.updateMany(
    {<condiciones filtro>},
    { $set: <JSON valores a actualizar>}
)
```

Ejemplo.  A todos los usuarios mayores de 18 se pondra estatus rechazados:

```
db.users.updateMany(
    {age:{$lt:18} },
    {$set:{status:"reject" } }
)
```

<a name="borrar"></a>
## Delete

Borra el primer documento que cumplan con el filtro:

```
db.collection.deleteOne({status:"A"})	
```

Borra todos los documentos de la base de datos:

```
db.collection.deleteMany({})	
```

Puede incluir los siguientes operadores en la condicion [Operadores](https://docs.mongodb.com/manual/reference/operator/update/): 

```
db.<colección>.deleteMany(
    {<condiciones filtro>}
)
```

<a name="proyeccion"></a>
## Proyecciones

Ejemplo de una proyección, por defecto trae 20 registros si no se especifica:


```
"db.getCollection(<colección>).find(
    {<criterios de selección>},
    {<proyección>}
).limit(<n>)
.sort({<campos de ordenamiento>})"
```

_Se puede consultar la documentación de MongoDB [Proyecciones](https://docs.mongodb.com/manual/tutorial/query-documents/#specify-conditions-using-query-operators) para mayor información._



<a name="agregacion"></a>
## Agregaciones

Partiendo de la siguiente colección de ejemplo:

```
{ _id  : '1', name : 'Renato Cacho', rides: 10 },
{ _id  : '2', name : 'Sergio Robles', rides: 7 }
```

Este sería un ejemplo de agregación:

```
db.users.aggregate([{
    $match: {},
    $group: {
        _id: “001”,
       totalRides: { $sum: “$rides” }
    }
}])
```

_Se puede consultar la documentación de MongoDB [Agregaciones](https://docs.mongodb.com/manual/aggregation/) para mayor información._

<a name="indices"></a>
## Índices

Para buscar en texto primero debemos crear un indice en el campo de texto en el que queremos buscar:

```
db.users.createIndex( { name: 'text' } )	
```

La búsqueda usando el índice se haría de la siguiente manera:

```
db.users.find({
    $text: {
        $search: 'de',
        $caseSensitive: true
    }
})
```

Para ver los índices:

```
db.users.getIndexes()	
```

Para configurar un Tiempo de vida en los documentos TTL:

```
db.<coleccion>.createIndex(
    { <campo fecha>: 1 },
    { expireAfterSeconds: <cantidad de segundos> }
)	
```

_Se puede consultar la documentación de MongoDB [TTL](https://docs.mongodb.com/manual/core/index-ttl/) para mayor información._


<a name="importar"></a>
## Importar y Exportar

Si no es una base de datos local y esta en un servidor podremos especificar las opciones:

```
mongodump --db test --host mongodb.example.net --port 27017	
```

Si tiene contraseña:

```	
mongodump --db test --host mongodb1.example.net --port 27017 --username user --password "pass"	
```

Exportar una colección específica:

```
mongodump --collection myCollection --db test	
```

Para hacer un backup en una carpeta especifica usamos:

```
mongodump --db mobility --out C:\..........<ruta>	
```

Y para hacer el restore es lo mismo:

```
mongorestore -db C:\..........<ruta>
```

_Se puede consultar la documentación de MongoDB [Backup](https://docs.mongodb.com/manual/tutorial/backup-and-restore-tools/) para mayor información._
