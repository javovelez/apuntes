## Creando un nuevo módulo

Un módulo se crea sobre una carpeta que debe contener como mínimo los siguientes dos archivos:

1. ``__init__.py`` en el cual se deben importar las carpertas que contengan modelos

2. ``__manifest__.py`` en el cual se declaran el nombre del módulo, dependencias, datos, vistas y otras configuraciones.

Luego el directorio debe estar dentro de la lista del addonspath del archivo de configuración.

Para poder visualizar el módulo desde odoo debemos entrar en modo desarrollador y dirigirnos a aplicaciones y hacer click sobre "actualizar lista de aplicaciones

Ejemplo de manifest:

~~~
{
'name': "My library",
'summary': "Manage books easily",
'description': """Long description""",
'author': "Your name",
'website': "http://www.example.com",
'category': 'Uncategorized',
'version': '12.0.1',
'depends': ['base'],
'data': ['views.xml'],
'demo': ['demo.xml'],
}
~~~