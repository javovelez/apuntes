## Creando un nuevo módulo

Un módulo se crea sobre una carpeta que debe contener como mínimo los siguientes dos archivos:

1. ``__init__.py`` en el cual se deben importar las carpertas que contengan modelos

2. ``__manifest__.py`` en el cual se declaran el nombre del módulo, dependencias, datos, vistas y otras configuraciones.


El directorio que contiene los directorios que contienen los archivos __init__.py y __manifest__.py debe estar dentro de la lista del addonspath del archivo de configuración. El nombre técnico del módulo será el nombre del directorio que contiene los archivos mencionados.

Ejemplo:

~~~
~/extra-addons/mis_modulos
~~~ 
Ese directorio deberá estar en el addons path. Todos los directorios dentro de este que contengan los archivos __init__.py y __manifest__.py serán vistos por odoo.

Manifest en detalle:

~~~
name: A string with the addon module title.  
description: A long text with the description of the features, usually in RST format.  
author: The author's name. It is a string, but can contain a comma-separated list of names.  
depends: A list of the addon modules it depends on. They will be automatically installed before this module is.  
application: A Boolean flag, declaring whether the addon module should be featured as an app in the apps list.  

~~~

Es recomendable también que se completen los siguientes keys que son relevantes en la odoo app store.

~~~
summary: A string displayed as a subtitle for the module.  
version: By default, 1.0. It should follow versioning rules (see http:/ / semver. org/ for details). It is good practice to use the Odoo version before our module version, for example, 12.0.1.0.
license: By default considered to be LGPL-3. website: A URL to get more information about the module. This can help people find more documentation or the issue-tracker to file bugs and suggestions.  
category: A string with the functional category of the module, which defaults to Uncategorized. A list of existing categories can be found in the security Groups form (at Settings | User | Groups), in the Application field drop-down list.  
~~~

Para generar un esqueleto del módulo puedo usar el comando ``.../pathAOdoo/odoo/odoo.bin scaffold nombre_del_modulo``. Lo debo ejecutar con el virtualevn corespondiente en caso de que se use uno, y parado en el directorio en el que voy a crear el módulo  ya que crea todos los templates con el string que le pasamos al comando scaffold  como argumento de directorio. 

Para comenzar el desarrolo de un módulo primero que editaremos es "views/nombreDeLaVista.xml" que contendrá las vistas del módulo y crearemos el topmenu el cual es el punto de entrada a esta aplicación.

~~~
<odoo>
  <data>
    <!-- explicit list view definition -->
    <!-- actions opening views on models -->
    <menuitem name="Certificados" id="certificates.menu_root"/>
    <!-- server action to the one above -->
    <!-- Top menu item -->
    <menuitem name="Certificados" id="certificates.menu_root"/>
    <!-- menu categories -->
    <menuitem name="Certificados" id="certificates.certificados" parent="certificates.menu_root" action="certificates.action_window"/>
    <!-- actions -->
  </data>
</odoo>
~~~

Luego creamos los modelos nuevos de nuestra app para que se correspondan con las acciones.

Luego necesitamos crear los grupos de seguridad. Para esto creamos primero la catería de nuestra app en ``security/library_security.xml``

~~~
<record id="module_certificates_category" model="ir.module.category">
    <field name="name">Certificados</field>
</record>
~~~

Luego creamos los tipos de usuario dentro de la categoría

~~~
    <!-- Certificate User Group -->
    <record id="certificates_group_user" model="res.groups">
        <field name="name">User</field>
        <field name="category_id"
                ref="module_certificates_category"/>
        <field name="implied_ids"
                eval="[(4, ref('base.group_user'))]"/>
    </record>
    <!-- Library Manager Group -->
    <record id="library_group_manager" model="res.groups">
    <field name="name">Manager</field>
    <field name="category_id"
        ref="module_library_category"/>
    <field name="implied_ids"
        eval="[(4, ref('library_group_user'))]"/>
    <field name="users"
        eval="[(4, ref('base.user_root')),
        (4, ref('base.user_admin'))]"/>
    </record>
~~~~

implied_ids is a one-to-many relational field, and contains a list of groups that will also apply to users belonging to this group. It uses a special syntax that will be explained in Chapter 5, Import, Export, and Module Data. In this case, we are using code 4 to add a link to base.group_user, the basic internal user group.

Luego configuramos control de acceso de cada grupo con el archivo ir.model.acces.csv

~~~
id,name,model_id:id,group_id:id,perm_read,perm_write,perm_create,perm_unlink
access_certificate_user,certificatesUser,model_certificates_certificates,certificates_group_user,1,0,0,0
access_book_manager,certficatesManager,model_certificates_certificates,certificates_group_manager,1,1,1,1
~~~

## Record rules o Reglas de registro.

Si, por ejemplo, no queremos que los usuarios normales (con solo acceso de lectura al modelo) pueda ver registros archivados, lo que se utiliza en estos casos son las reglas de registro.

Las reglas de registro pertenecen al modelo ir.rule y las podemos generar por ejemplo en nuestro escribiendo el siguiente código en ``security/library_security.xml``.

~~~
    <data noupdate="1">
        <record id="book_user_rule" model="ir.rule">
            <field name="name">Library Book User Access</field>
            <field name="model_id" ref="model_library_book"/>
            <field name="domain_force">
                        [('active','=',True)]
            </field>
            <field name="groups" eval="[(4,ref('library_group_user'))]"/>
        </record>
    </data>
~~~

el ``<data noupdate="1">`` significa que este registro se creará en la instalación del módulo pero no será reescrito en las actulizaciones del módulo. Esto se hace para que las reglas que se creen customizadas luego de la instalación del módulo no se pierdan al realizar un update.

Para construir vistas personalizadas agregamos:

 ~~~
<record id="view_form_book" model="ir.ui.view">
    <field name="name">Book Form</field>
    <field name="model">library.book</field>
    <field name="arch" type="xml">       
    </field>
</record>
 ~~~


 Existe uno en especial llamado "ondelete", este atributo tiene 3 funciones:

- Cascade. Permite eliminar los registros ligados a una clase, cuando tiene un campo one2many.
- Restrict. Impide la eliminación de la fila referenciada. 
- Set null. Modifica el campo a Nulo ó False, cuando el registro de la relación sea Eliminado.

## Vista form

Las vistas se diagraman como si fuera un documento, con encabezado y cuerpo, En el encabezado suelen ir los botones de acción.

~~~
<form>
    <header>
        <!-- Buttons will go here -->
    </header>
    <sheet>
    <!-- Content goes here: -->
        <group>
            <field name="name" />
        </group>
    </sheet>
</form>
~~~

### Botones

~~~
<header>
    <button name="button_check_isbn" type="object"
                string="Check ISBN" />
</header>
~~~

The string with the text to display on the button
The type of action it performs
name is the identifier for that action
class is an optional attribute to apply CSS styles, as in regular HTML

Luego debemos definir la funcion ``button_check_isbn`` dentro del modelo para que cuando se presione el botón se ejecute ese código.

### Vista search

Sirve para defenir por qué campos se realizarán las búsquedas y con qué criterios.

~~~
    <record id="view_search_book" model="ir.ui.view">
        <field name="name">Book Filters</field>
        <field name="model">library.book</field>
        <field name="arch" type="xml">
        <search>
            <field name="publisher_id"/>
            <filter name="filter_inactive"
                    string="Inactive"
                    domain="[('active','=',True)]"/>
            <filter name="filter_active"
                    string="Active"
                    domain="[('active','=',False)]"/>
            </search>
        </field>
    </record>
~~~

## Controladores.

Sirve para realozar páginas de frontend dentro. 

1. En controllers se crea la ruta y el se asocia a un modelo

~~~
from odoo import http
class Books(http.Controller):
    http.route('/library/books', auth='user')
    def list(self, **kwargs):
        Book = http.request.env['library.book']
        books = Book.search([])
        return http.request.render(
        'library_app.book_list_template', {'books': books})
~~~

2. Creamos la vista template, es el código qweb para renderizar la vista.

~~~
<odoo>
    <data>
      <template id="book_list_template" name="Book List">
        <div id="wrap" class="container">
          <h1>Books</h1>
            <t t-foreach="books" t-as="book">
              <div class="row">
                <span t-field="book.name" />,
                <span t-field="book.date_published" />,
                <span t-field="book.publisher_id" />
              </div>
            </t>
        </div>
      </template>
    </data>
</odoo>
~~~ 

3. Debemos asegurarnos agregar la vista al manifest principal y importar el directorio controllers al init principal y el nombre del archivo de controller en el init de la carpeta controllers.

## Herencia de modelos

``_inherit = 'library.book'`` En esta manera de heredar crear no se generan un nuevo modelo. Simplemente se le pueden agregar o modifica campos y funciones.

Se puede heredar de múltiples padres. Esto se realiza con **clases mixtas** las cuales se utilizan para implementar y reutilizar funcionalidades genericas como por ejemplo el sistema de mensajes en odoo.

Si sobreescribimos el campo _name, obtenemos lo que odoo llama **herencia prototipada**. Tengo todos los campos y métodos de la clase original pero con su propia tabla y datos. En la práctica esto solo se usa para para heredar clases mixtas abstractas.

Ejemplo:

En manifest
~~~
'depends': ['library_app', 'mail'],
~~~

En modelo:

~~~
class Member(models.Model):
_name = 'library.member'
_description = 'Library Member'
_inherit = ['mail.thread', 'mail.activity.mixin']
~~~

En vista:

~~~
<div class="oe_chatter">
<field name="message_follower_ids" widget="mail_followers"/>
<field name="activity_ids" widget="mail_activity"/>
<field name="message_ids" widget="mail_thread"/>
</div>
~~~

También tenemos la **herencia delegada**, que se hereda con ``_inherits`` en vez de ``_inherit``.

``_inherits = {'res.partner': 'partner_id'}.``

Esto nos permite crear un nuevo modelo que contiene y extiende el modelo padre. Cuando se crea un registro del nuevo modelo, se crea un registro del modelo padre y se enlaza mediante campos many2many. El hijo no puede existir sin el padre, pero el padre si sin el hijo.

También se puede realizar directamente en el campo id, agregando el modificador ``delegate=True``. 

~~~ 
from odoo import fields, models
    class Member(models.Model):
    _name = 'library.member'
    _description = 'Library Member'
    card_number = fields.Char()
    partner_id = fields.Many2one(
            'res.partner',
            delegate=True,
            ondelete='cascade',
            required=True)
~~~ 


It may be useful to note that the delegation could be replaced by a
combination of the following:
- A many-to-one filed to the parent record
- An override to the create() method, to automatically create and set the parent record
- The related fields to the parent fields, for the specific fields we
want to expose.

Sometimes, this is more appropriate than a full-fledged delegation inheritance. For instance, res.company does not inherit res.partner, but uses a res.partner record to store several of its fields.


## Herencia de vistas

~~~ 
<odoo>
    <record id="view_form_book_extend" model="ir.ui.view">
        <field name="name">Book: add Is Available? field</field>
        <field name="model">library.book</field>
        <field name="inherit_id" ref="library_app.view_form_book"/>
        <field name="arch" type="xml">
            <field name="isbn" position="after">
            <field name="is_available" />
            </field>
        </field>
    </record>
</odoo>
~~~ 

### Position

- inside (the default): Appends the content inside the selected node, which should be a container, such as <group> or <page>.

- after: Adds the content to the parent element, after the selected node.
- before: Adds the content to the parent element, before the selected node.
- replace: Replaces the selected node. If used with empty content, it deletes the element. Since Odoo 10, it also allows you to wrap an element with other markup by using $0 in the content to represent the element being replaced.
- attributes: Modifies attribute values for the matched element. The content should have one or more ``<attribute name="attrname">value<attribute>`` elements, such as ``<attribute name="invisible">True></attribute>``. If used with no body, as in ``<attribute name="invisible"/>``, the attribute is removed from the selected element.

Except for the attributes position, the preceding locators can be combined with a child
element with position="move". The effect is to move the child locator target node to the
parent locator's target position.

~~~ 
<field name="target_field" position="after">
    <field name="my_field" position="move"/>
</field>
~~~ 

### XPath

In some cases, we may not have an attribute with a unique value to use as the XML node selector. 

``<field name="isbn"> element is //field[@name]='isbn'.``

~~~ 
<expr="//field[@name='isbn']" position="after">
<field name="is_available" />
</xpath>
~~~ 

## Modificando datos (extendiendo datos existentes).

Unlike Views, regular data records don't have an XML arch structure and can't be extended using XPath expressions. However, they can still be modified by replacing the values in their fields.

The ``<record id="x" model="y">`` data loading elements actually perform an insert or update operation on Model y: If record x does not exist, it is created; otherwise, it is updated/written over.

As an example, we will change the name of the User security group to  ibrarian. We will modify the library_app.library_group_user record. For this, we will add the library_member/security/library_security.xml file, along with the following code:
~~~
<odoo>
<!-- Modify Group name -->
    <record id="library_app.library_group_user" model="res.groups">
        <field name="name">Librarian</field>
    </record>
</odoo>
~~~
Este código debe incluirse en el manifest en "data".

## Extendiendo métodos de python.

Simplemente sobreescrivo el método, donde debo tener cuidado en no modificar el orden de los argumentos para no romper otra lógica al rededor del mismo método. Se puede llamar al método original dentro del método nuevo con ``super()._nombre_del_metodo().


## Extendiendo web controllers

Creamos un ``/controllers/main.py`` dentro delo directorio del módulo que extá extendiendo código de otros modelos.

## Importar y exportar datos con xml y csv

When using an external identifier in a data file, we can choose to use either the complete identifier or just the external identifier name. Usually, it's simpler to just use the external identifier name, but the complete identifier enables us to reference data records from other modules. When doing so, make sure that those modules are included in the module dependencies to ensure that those records are loaded before ours.

There are some cases where the complete ID is needed, even if referring to
an XML ID from the same module.

One way to do this is to use the Settings | Technical | Sequences & Identifiers | External Identifiers menu, which was shown earlier. We can also use the Developer menu for this.

As you may recall from Chapter 1, Quick Start Using the Developer Mode and Concepts, the Developer menu is activated in the Settings dashboard, in an option at the bottom-right. 

To find the external identifier for a data record, we should open the corresponding form view, select the Developer menu, and then choose the View Metadata option. This displays a dialog with the record's database ID and external identifier (also known as the XML ID).

To find the external identifier for view elements, such as form, tree, search, or action, the Developer menu is also a good source of help. For this, we can use the appropriate Edit View option to open a form with the details for the corresponding view. There, we will find an External ID field, providing the information we are looking for.

While CSV files provide a simple and compact format to represent data, XML files are more powerful and give more control over the loading process. For example, their  filenames are not required to match the model to be loaded. This is because the XML format is much richer and more information regarding what to load can be provided through the XML elements inside the file.


#### El atributo noupdate

This rewrite behavior is the default, but it can be changed so that some of the data is only imported at install time, and is ignored in later module upgrades. This is done using the noupdate="1" attribute in the <odoo> or <data> elements. This is useful for data that is to be used as initial configuration but is expected to be customized later, because these manually made customizations will be safe from module upgrades.

It is possible to have more than one <data> section in the same XML file. We can take advantage of this to separate data to import only once, with noupdate="1" and data that can be re-imported on each upgrade, with noupdate="0".

The noupdate attribute can be tricky when developing modules, because
changes made to the data later will be ignored. One solution is to, instead
of upgrading the module with the -u option, re-install it using the -i
option. Reinstalling from the command line using the -i option ignores
the noupdate flags on data records.

## Definiendo registros en XML

A more elaborate alternative for setting a field value is the eval attribute. It evaluates a Python expression and assigns the result to the field.

To handle dates, the following Python modules are available: time, datetime, timedelta, and relativedelta. For more information about these Python modules, see the documentation at https:/ / docs. python. org/ 3/ library/ datatypes. html.

~~~
<field name="date_published"
    eval="(datetime.now() + timedelta(-1))" />
~~~

Also available in the evaluation context is the ref() function, which is used to translate an external identifier into the corresponding database ID. This can be used to s et values for relational fields. As an example, we can use it to set the value for publisher_id: 

~~~
<field name="publisher_id" eval="ref('res_partner_packt')" />
~~~

### Setting values on many-to-one relation fields
For many-to-one relation fields, the value to write is the database ID for the linked record. In XML files, we usually know the XML ID for the record, and we need to have it translated into the actual database ID.

One way is to use the eval attribute with a ref() function, like we just did in the previous section.

A simpler alternative is to use the ref attribute, which is available for <field> elements.

Using it to set the value for the publisher_id many-to-one field, we would write the following:

``<field name="publisher_id" ref="res_partner_packt" />``

### Setting values on to-many relation fields
For one-to-many and many-to-many fields, instead of a single ID, a list of related IDs is expected. Furthermore, several operations can be performed—we may want to replace the current list of related records with a new one, or append a few records to it, or even unlink some records.

LA FUNCIÓN ``ref()`` TRANSFORMA XML_IDs EN IDs

~~~
<field name="author_ids"
            eval="[(6, 0,
                    [ref('res_partner_alexandre'),
                    ref('res_partner_holger')]
                    )]"
            />
~~~

The complete list of available commands is as follows:

~~~
(0, _ , {'field': value}) creates a new record and links it to this one.
(1, id, {'field': value}) updates the values on an already linked record.
(2, id, _) removes the link to and deletes the id related record.
(3, id, _) removes the link to, but does not delete, the id related record. This
is usually what you will use to delete related records on many-to-many fields.
(4, id, _) links an already existing record. This can only be used for many-tomany fields.
(5, _, _) removes all the links, without deleting the linked records.
(6, _, [ids]) replaces the list of linked records with the provided list.
~~~

for cases where we need to modify just a particular field of a user
interface element, we should do it using a <record> element instead.

These are convenient shortcuts for frequently used models, with a more compact notation compared to the regular <record> elements.

For reference, these are the shortcut elements available, along with the corresponding models they load data into:

~~~
<act_window> is for the window action model, ir.actions.act_window
<menuitem> is for the menu items model, ir.ui.menu
<report> is for the report action model, ir.actions.report.xml
<template> is for QWeb templates stored in the ir.ui.view model
~~~

It is important to note that, when used to modify existing records, the shortcut elements overwrite all the fields. This differs from the <record> basic element, which only writes to the fields provided. So,for cases where we need to modify just a particular field of a user interface element, we should do it using a <record> element instead.

### Otras acciones en archivos XML de datos.

Delete

~~~
<delete
    model="res.partner"
    search="[('id','=',ref('library_app.res_partner_daniel'))]"
/>

Lo siguiente hace lo mismo

<delete model="res.partner" id="library_app.res_partner_daniel" />
~~~

Llamada a métodos

~~~
<data noupdate="1">
    <function
        model="res.users"
        name="_init_data_user_note_stages"
        eval="[]" />
</data>
~~~

This calls the _init_data_user_note_stages method of the res.users Model, passing no arguments. The argument list is provided by the eval attribute, which is an empty list in this case.

## Estructurando la información de la aplicación.

There are a few more attributes that can be used in advanced cases:

_rec_name  
_table  
_log_access=False  
_auto=False  
_inherit  
_inherits  
_order = 'name, date_published desc'  

### Modelos y clases en python

Los modelos en Odoo son guardados en un registro centralm disponiblen en el entorno "env". Este es un diccionario que guarda referencias a todas las clasese de modelos disponibles en la base de datos y la manera de refernciarlos es con el nombre del modelo (_name). Especificamente en un método de un modelo podemos recuperar la clase del modelo con self.env['model.name']

### Modelos abstractos y transitorios. (transient and abstract)

Los modelos visto hasta ahora se basan en la clase models.Model 
Este tipo de modelos tienen persistencia permanente en la base de datos.

- Modelos transitorios: se basan en la clase models.TransientModel y son usados para interacción de usuario tipo wizard. Un job de vaciado (vacuum) borra toda la información de las tablas periódicamente.

- Modelos abstractos: se basan en la clase models.AbstractModel y no tienen datos pesristentes. Se usan para características reutilizables como la clase mail

### Atributos comunes de compos

 - string
 - default: podemos darle un valor o una función
 - help: muestra texto de ayuda en ui
 - readonly: boolean
 - required: boolean
 - index: boolean nos permite crear un índice para el campo en la db. Esto permite búsquedas más rápidas pero a un costo de tiempo de escritura.
- copy: boolean si es False cuando copiemos un registro este campo no se va a copiar.
- groups: permite limitar el acceso y visibilidad del campo a solamente los grupos defenidos acá. Se pasa con una lista de XML IDs separados por como: ``groups='base.group_user,base.group_system'.```
- states: espera un diccionario mapeando valores de la interfaz de usuario dependiendo de los valores del campo state. puede ser usado con readonly, required, and invisible, for example, ``states={'done':[('readonly',True)]}``.

Note that the states field attribute is equivalent to the attrs attribute in
views. Also, note that views support a states attribute, but it has a different use: it accepts a comma-separated list of states to control when the element should be visible.

### Nombre de campos especiales.

Los siguientes campos se crean automáticamente a no ser que tengamos el campo ``_log_access=False``: create_uid create_date write_uid write_date

- active: booleano nos permite desactivar registros. Si está en False automaticamente se ignoarará en cualquier consulta.  Este filtro por defecto puede desactivar agregando ``{'active_test': False}`` al contecxto actual.

- state: representa el estado del ciclo de vida del registro. Nos permite tener diferentes comportamientos de la UI dependiendo del estado del registro.  Dynamically modify the view: fields can be made readonly, required, or invisible in specific record states.

- parent_id and parent_path (type Integer and Char): have special meaning
for parent or child hierarchical relations

### Relaciones entre modelos.

#### Many2one

``publisher_id = fields.Many2one('res.partner', string='Publisher')``

- ondelete

- context

- domain

- auto_join=True

- delegate=True

#### One2many

The One2many fields accept three positional arguments:  
- The related model (comodel_name keyword argument).  
- The field in that model referring to this record (inverse_name keyword argument). 
- The field label (string keyword argument).

~~~
from odoo import fields, models
    class Partner(models.Model):
        _inherit = 'res.partner'
        published_book_ids = fields.One2many(
                        'library.book', # related model
                        'publisher_id', # field for "this" on related model
                        string='Published Books')
~~~

The additional keyword arguments available are the same as for many-to-one fields: context, domain, and ondelete (here acting on the many side of the relationship).

#### Many-to-many relationships

On some occasions, we may need to override these automatic defaults. One such case is when the related models have long names, and the name for the automatically generated relationship table is too long, exceeding the 63-character PostgreSQL limit. In these cases, we need to manually choose a name for the relationship table to conform to the table name size limit.

Another case is when we need a second many-to-many relationship between the same
models. In these cases, we need to manually provide a name for the relationship table so that it doesn't collide with the table name already being used for the first relationship.

#### Relaciones jerárquicas

Parent-child tree relationships are represented using a many-to-one relationship with the same model, used for each record to references its parent. The inverse one-to-many relation corresponds to the record's direct children.

Odoo provides improved support for these hierarchical data structures, with the additional child_of and parent_of operators available in domain expressions. These opera tors are available as long as the model has a parent_id field (or has a _parent_name valid Model definition).

We can enable faster querying on the hierarchy tree by setting the  parent_store=True Model attribute and adding the parent_path helper field. This fields stores additional information about the hierarchy tree structure that is leveraged for faster queries.

#### Relaciones flexibles usando campos de referencia.

Regular relational fields reference one fixed co-model. The Reference field type does not have this limitation and supports flexible relationships, so that the same field is not restricted to always be pointing to the same destination model.

As an example, we will use it in our book category model to add a reference to a
highlighted book or author. So, the field could refer to either a book or a partner:

~~~
# class BookCategory(models.Model):
    highlighted_id = fields.Reference(
            [('library.book', 'Book'), ('res.partner', 'Author')],
            'Category Highlight',
            )
~~~

Here are a few additional technical details about reference fields that can be useful:
- Reference fields are stored in the database as a model,id string 
- the read() method, meant for use from external applications, returns themformatted as a ('model_name', id) tuple, instead of the usual (id,'display_name') pair for many-to-one fields.

#### Campos computados.

~~~
# class Book(models.Model):
    	publisher_country_id = fields.Many2one(
    	    'res.country', string='Publisher Country',
    	    compute='_compute_publisher_country',
    	    )
    	
        @api.depends('publisher_id.country_id')
    	def _compute_publisher_country(self):
    	    for book in self:
    	        book.publisher_country_id = book.publisher_id.country_id
~~~

The preceding code adds the publisher_country_id field, and the
_compute_publisher_country method used to compute it. The function name was
passed to the field as a string argument, but it may also be passed a callable reference (the function identifier, without the surrounding quotes). In that case, we need to make sure the function is defined in the Python file before the field is.

The @api.depends decorator is needed when the computation depends on other fields, as it usually does

In our case, our field should be recomputed whenever the country_id of the
book's publisher_id is changed.

#### Searching and writing to computed fields

The computed field we just created can be read, but it can't be searched or written to. By default, computed field values are written on the fly, and are not stored in the database. That's why we can't search them like we can regular fields.

~~~
# class Book(models.Model):
    publisher_country_id = fields.Many2one(
                    'res.country', string='Publisher Country',
                    compute='_compute_publisher_country',
                    # store = False, # Default is not to store in db
                    inverse='_inverse_publisher_country',
                    search='_search_publisher_country',
                )
~~~ 

#### Campos relcionados

By default, related fields are read only, so the inverse write operation won't be available. To enable it, set the readonly=False field attribute.

It's also worth noting that these Related fields can also be stored in a database using store=True, just like any other computed field. 

### Modelos base 

The base module provides two kinds of Models:  
- Information Repository, ir.* models  
- Resources, res.* models  

Information Repository is used to store data needed by Odoo:

- ir.actions.act_window for Windows Actions
- ir.ui.menu for Menu Items
- ir.ui.view for Views
- ir.model for Models
- ir.model.fields for Model Fields
- ir.model.data for XML IDs

The resources contain basic data about the world, which can be useful for applications in general. These are the more important resource models:

- res.partner for business partners, such as customers, suppliers, and so on, and addresses
- res.company for company data
- res.currency for currencies
- res.country for countries
- res.users for application users
- res.groups for application security groups

## Recordsets, Trabajando con información de modelos.

Para ejecutar un shell de una base de datos:

~~~
$ ./odoo-bin shell -d 12-library
~~~

En mi caso con virtualenv
~~~
/home/javo/odoo13/odoo-venv/bin/python3 /home/javo/odoo13/odoo13/odoo-bin shell -d iss
~~~

Acá self representará el registro del usuario administrador, lo que se puede confirmar tecleando ``self`` en la consola

Para salir de la consola presionamos ``ctrl+D``

### Atributos del hambiente (enviroment)

Podemos inspeccionar el hambiente actual con ``self.env``

El hambiente de ejecución en self.env tiene los siguientes atributos disponibles:

- env.cr is the database cursor being used.
- env.user is the record for the current user.
- env.uid is the ID for the session user. It's the same as env.user.id.
- env.context is an immutable dictionary with a session context.

El hambiente provee acceso al registro donde todos los modelos instalados están disponibles. Por ejemplo ``self.env['res.partner']`` devuelve una referencia a todos al modelo partner. Podemos usar ``search()`` o ``browse()`` para recuperar recordsets (conjunto de registros de un modelo)

~~~
self.env['res.partner'].search([('name', 'like', 'Ad')])
>>>> res.partner(10,35,3)
~~~

En este ejemplo el recordset obtenido contiene tres registros. con IDs 10; 35 y 3.

### The enviroment context

El contexto es un diccionario que contiene la información de la sesión.

### Modificando el el hambiente de ejecución de los registros.

El hambiente de ejecución de los registros es inmutable, pero se puede crear un hambiente modificado y luego ejecutar accioner usándolo.

- env.sudo(user) se ejecuta con un registro de usuario y devuelve un hambiente con ese usuario. Si no se provee un usuario se usará el superusuario __system__ que permite saltar reglas de seguridad para operaciones específicas.

- env.with_context(<dictionary>) reemplaza el contexto con uno nuevo.
- env.with_context(key=value,...)  modifica el contexto actual seteando valores para algunas de sus keys.

- Adicionalmente tenemos  la fucnción env.ref() tomando un string con un identificador externo y devolviendo el registro correspondiente.

~~~
>>> self.env.ref('base.user_root')
res.users(1,)
~~~

### Consultando información con recordsets y dominios.

Dentro de un método o de una sesión de shell, self representa el modelo actual y solo podemos acceder a los registreos de ese modelo. Para acceder a otros modelos deberíamos usar self.env. Por ejemplo self.env['res.partner'] devuelve una referencia al modelo partner (el cual es un registro vacio)

Podemos usar search() y browse() para para traer recordsets. search() usa una expresión de dominio el criterio de selección de dominios. Si usamos search() con con un dominio vacio traerá todos los registros activos.

Se pueden usar las siguientes palabras claves como argumento:

- order: string con una lista de campos separados por coma.
- limit: limita la cantidad máxima de registros que trae.
- offset: ignora los primeros n resultados 

Para saber el número de registros encontrados en una búsqueda podemos usar el método search_count()

El método browse() toma una lista de IDs y devuelve el recordset con esos registros.

~~~
>>> self.env['res.partner'].search([('name', 'like', 'Ag')])
res.partner(9, 31)
>>> self.env['res.partner'].browse([9, 31])
res.partner(9, 31)
~~~

    certificate_id = fields.Many2one(
        'certificates.certificates', 
        string='Certificado TIMSA', 
        required=True, 
        ondelete='restrict', 
        )
    cert_name = fields.Char(
        'Certificado',
        related='certificate_id.name', # no es delegate sino related
    )