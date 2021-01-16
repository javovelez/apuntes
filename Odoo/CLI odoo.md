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

Inicio de odoo 13 en mi wsl:

/home/javo/odoo13/odoo-venv/bin/python3 /home/javo/odoo13/odoo13/odoo-bin -c /home/javo/odoo13/odoo13.conf -d timsatest -u timsa_test --dev xml