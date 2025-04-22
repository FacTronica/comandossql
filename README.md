# Comandos Sql
Un Resumen de comandos Sql

## PERMISOS => MYSQL
````
mariadb -u root -p
CREATE USER 'usuario'@'localhost' IDENTIFIED BY 'suclave';
GRANT ALL PRIVILEGES ON *.* to 'usuario'@'localhost';
FLUSH PRIVILEGES;
quit;
````

## EXPORTAR BASE DE DATOS COMPLETA:
````
mysqldump --user=$USUARIO --password=$CLAVE $BD > $ARCHIVO.sql;
````

## EXPORTAR SOLO UNA TABLA DE UNA BASE DE DATOS:
````
mysqldump --user=$USUARIO --password=$CLAVE $BD $TABLA > $ARCHIVO.sql;
o
mysqldump -u $USUARIO -p $CLAVE $BD $TABLA > $ARCHIVO.sql;
````

## EXPORTAR SOLO LA ESTRUCTURA
````
mysqldump -u $USUARIO -p --no-data $BD $TABLA > $ARCHIVO.sql
````

## EXPORTAR SOLO LOS DATOS
````
mysqldump -u $USUARIO -p --no-create-info $BD $TABLA > $ARCHIVO.sql
````

## EXPORTAR Y COMPRIMIR BASE DE DATOS
````
mysqldump -u $USUARIO -p $BASEDATOS $TABLA | gzip > $TABLA.sql.gz
````

## IMPORTAR UNA BASE DE DATOS:
````
mysql -u $USUARIO -p$CLAVE $BD < $ARCHIVO.sql;
````

### EJECUTAR COMANDOS SQL DESATENDIDO DESDE CONSOLA
````
mysql --host=localhost --user=usuariobd --password=clavebd -e "create database basedatos";
mysql --host=localhost --user=usuariobd --password=clavebd -e "truncate table basedatos.tbl_tabla";
````
 
