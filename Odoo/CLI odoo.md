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
