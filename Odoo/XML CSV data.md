# Cargando datos con archivos .xml (odoo13)

Es posible cargar datos al instalar un módulo creado por nosotros. Generalmemnte se encuentra en la carpeta ``./data`` y debe declararse en el archivo __manifest__ del módulo.

La carga se realiza indicándo el modelo sobre el cual queremos crear o modificar un registro y un código que será único para cada registro. 
Los XML ID están formados de la siguiente manera:

&lt;module_name>.&lt;record_id>

El <module_name> (conocido como name space) será el modulo que creó el id y el <record_id> lo debemos saber en caso de querer modificar un registro existente, o lo debemos definir nosotros si lo está creando nuestro módulo. Si nosotros estamos creando el id el space name será el nombre de nuestro modulo.

Desde el código se puede acceder a estos registros mediante el método self.env.ref() y debemos pasarle el XML ID completo (con space name). En caso de no contar con el space name Odoo asumirá que es el del módulo en que se encuentra el codigo que se está ejecutando.

Ejemplo:



Si queremos cambiar el nombre a la compañia escibiremos lo siguiente dentro de ``./data/data.xml`` 

&lt;record id="base.main_company" model="res.company">

&lt;field name="name">Packt publishing</field>

</record> 


y luego deberemos agregar este archivo al manifest del módulo.

Si queremos saber si un registro existente tiene XML ID lo podemos encontrar estando en modo de desarrollador dentro del registro haciendo click en el ícono del bug y seleccionando " view metadata"

Para campos one2many y many2many (x2many de manera general) devemos usar el atributo "eval". Un campo X2many espera una lista de tuplas, en el cual el primer valor de la tupla indica la operación que se desea realizar. El atributo "eval="[]" nos permite utilizar la función ref o el cual devuelve el id asociado a un XML ID que uno le pasa como dato.

- (2, id, False): elimina  de la base de datos el registro del id seleccionado.

- (3, id, False): esta operación remueve el id del campo x2many sin borrar el registro. Borra solo el id de la lista de ids del campo relacional.

- (4, id, False): esto genera un link a un rgistro existente con id conocido.

- (5, False, False): esto elimina todos los link del registro pero dejando intactos todos lo registros.

- (6, False, [id, ...]): Esto elimina las referencias existentes, y las remplaza por la lista.

Adicionalmente existen estas estas tuplas para "eval" que no se utilizan para agreagar datos en xml ya que al hacer update al módulo, duplican información.

- (0, False, {'key': value}): crea nuevo registro con los valores pasados por dicionario.
- (1, id, {'key': value}): para escribir sobre un registro existente.



Ejemplo:

    <field name='gross_income_jurisdiction_ids' eval="[(6,0,[
    ref('base.state_ar_x'),
    ref('base.state_ar_b'),
    ref('base.state_ar_y')]
    )]"/> 
                                                    
## Flags nonupdate y force create

Cuando escribimos un ``record`` en un rachivo xml que carga información, la etiqueta ``<odoo noupdate="1"> `` sirve para crear el registro cuando el módulo se instala pero no será actualizado en los subsiguientes updates. Si un usuario borra el registro, este se volverá a crear al hacer un apdate.

La flag puede aplicarse también dentro del record.

La etiqueta forcecreate="false" evitará que se cree nuevamente el registro al realizar un update. Si no se la explicita, por defecto es true.

Podemos forzar la creación de un registro que tenga flag nonupdate=1 se puede inicializando odoo con un ``-i nombre_del_modulo``

Si al hacer un update en un archivo xml, dejara de existir un determinado xml id, entonces odoo eliminará el registro con es xml id por considerarlo obsoleto.

# Cargar información a través de archivos .csv

se debe indicar en el manifest para instalarlo. El nombre del archivo debe coincidir con el modelo sobre el que queremos crear registros. Para cargar valores escalares si es necesario se pueden cargar entre comillas para evitar la confusión con puntos  comas pertenecientes al número.

Cuando se escribe sobre campos x2many Odoo trata de implementar la columna como un xml id.