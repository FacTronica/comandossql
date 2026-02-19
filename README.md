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
## PERMISOS PARA TABLA ESPECIFICA
````
GRANT SELECT , INSERT , UPDATE , REFERENCES ON basedatos.tabla TO 'usuario'@'localhost';
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

## RESPALDAR TODAS LAS BASES DE DATOS
````
#!/bin/bash

# ===============================
# CONFIGURACIÓN
# ===============================
USER="--usuario---"
PASS="---clave---"
HOST="localhost"

# Carpeta donde se guardarán los respaldos
BACKUP_DIR="/home/admin/backup"

# Fecha para los archivos
FECHA=$(date +"%Y-%m-%d_%H-%M")

# Crear carpeta si no existe
mkdir -p "$BACKUP_DIR"

# ===============================
# OBTENER LISTA DE BASES DE DATOS
# ===============================
DATABASES=$(mysql -u"$USER" -p"$PASS" -h"$HOST" -e "SHOW DATABASES;" | grep -Ev "(Database|information_schema|performance_schema|mysql|sys)")

# ===============================
# RESPALDO INDIVIDUAL
# ===============================
for DB in $DATABASES; do
    echo "Respaldando base de datos: $DB"
    mysqldump -u"$USER" -p"$PASS" -h"$HOST" "$DB" > "$BACKUP_DIR/${DB}.sql"
done

echo "Respaldo completo finalizado"
````

## RESTAURAR TODAS LAS BASES DE DATOS 

````
#!/bin/bash

# ===============================
# CONFIGURACIÓN
# ===============================
USER="--usuario---"
PASS="---clave---"
HOST="localhost"

# Carpeta donde están los respaldos
BACKUP_DIR="/home/admin/backup"

# ===============================
# VALIDACIONES
# ===============================
if [ ! -d "$BACKUP_DIR" ]; then
    echo "El directorio de backup no existe: $BACKUP_DIR"
    exit 1
fi

# ===============================
# PROCESO DE RESTAURACIÓN
# ===============================
for FILE in "$BACKUP_DIR"/*.sql; do

    # Validar que existan archivos .sql
    [ -e "$FILE" ] || continue

    # Obtener nombre de la base de datos (sin .sql)
    DB_NAME=$(basename "$FILE" .sql)

    echo "--------------------------------------"
    echo "Restaurando base de datos: $DB_NAME"
    echo "Archivo: $FILE"

    # Crear base de datos si no existe
    mysql -u"$USER" -p"$PASS" -h"$HOST" -e "CREATE DATABASE IF NOT EXISTS \`$DB_NAME\` CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;"

    # Importar SQL
    mysql -u"$USER" -p"$PASS" -h"$HOST" "$DB_NAME" < "$FILE"

    if [ $? -eq 0 ]; then
        echo "Restauración exitosa: $DB_NAME"
    else
        echo "Error restaurando: $DB_NAME"
    fi

done

echo "======================================"
echo "Proceso de restauración finalizado"
````
