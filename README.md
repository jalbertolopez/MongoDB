# MongoDB

_Memoria de comandos para MongoDB_

### Creación de usuarios

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

