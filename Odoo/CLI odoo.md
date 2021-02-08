# CLI odoo

``-d base ``

``-i base,account,project, ``

``adhoc -i ``

``--db-filter=``

``-p8070``

``-c /path/odoo13.conf``

``--log-level=debug``

``-u my_library ``

``--without-demo=all ``

``--load-language es_AR``

``sudo service postgresql start``

``psql -l``

``sudo service postgresql start``

``sudo systemctl odoo/postgreql start``

Para entrar a una db

``psql test-12.0``

Una vez dentro de la db puedo ver una tabla

``\d library_book;``

Para salir``\q`` y luego presione ENTER  de psql o ``ctrl+d``

``dropdb nombredelabaseaeliminar``

``pg_dump 13-base > prueba.sql (funciona)``

Para restaurar

``psql -U username -W -h host basename < basename.sql``

``createdb -O username maybeOtherName``

``psql  -W  TIMSATEST < TIMSATEST.sql``

El filestore (la carpeta donde se guardan todos los archivos binarios como imágenes) se encuentra en:

``/opt/odoo/.local/filestore``

Para crear una base de datos e inicializarla con los datos demo:

``createdb testdb && odoo-bin -d testdb``

Para duplicar una base de datos:
~~~
createdb -T dbname newdbname
cd ~/.local/share/Odoo/filestore
cp -r dbname newdbname
~~~

El uso de ``createdb -T`` solo funciona si no sesiones activas de en la base de datos.

Otra manera de hacer un buckup:
~~~
$pg_dump -Fc -f dbname.dump dbname
$tar cjf dbname.tgz dbname.dump ~/.local/share/Odoo/filestore/dbname
~~~
Y de restaurarla:
~~~
$ tar xf dbname.tgz
$ pg_restore -C -d dbname dbname.dump
~~~
Inicio de odoo 13 en mi wsl:

~~~
$ sudo service start postgresql
$ /home/javo/odoo13/odoo-venv/bin/python3 /home/javo/odoo13/odoo13/odoo-bin -c /home/javo/odoo13/odoo13.conf -d timsatest -u timsa_test --dev xml
~~~

Otra manera de traer actualizaciones:

~~~
$ git checkout 12.0
$ git tag 12.0-before-update-$(date --iso)
$ git pull –-ff-only origin 12.0
$ ./odoo-bin -c myodoo.cfg --stop-after-init -u base
~~~

El modificador --ff-only generará una falla si tenemos commits locales que no están en el repo remoto. Si fallara y quiero mantener los cambios ejecuto solo ``git pull``. Sino ejecuto ``git reset
--hard origin/12.0`` y descarto las modificaciones locales.

Y si quiero volver a la versión anterior:
~~~
$ git reset --hard 12.0-before-update-$(date --iso)
~~~

Tiro la base de datos dañada y restauro el bakup.

# Odoo Shell
~~~
/home/javo/odoo13/odoo-venv/bin/python3 /home/javo/odoo13/odoo13/odoo-bin shell -d iss
~~~

Para salir de la consola presionamos ``ctrl+D``

