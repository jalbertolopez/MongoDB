# MongoDB

_Memoria de comandos para MongoDB_

## Table of Contents

1. [ Creación de usuarios ](#creacion)
2. [ Importar y Exportar ](#importar)

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
