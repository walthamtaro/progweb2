---
label: CRUD 
order: 100
---

# El CRUD en un controlador

> CRUD (Create, Read, Update, Delete) es un acrónimo para las maneras en las que se puede operar sobre información almacenada. Es un nemónico para las cuatro funciones del almacenamiento persistente. CRUD usualmente se refiere a operaciones llevadas a cabo en una base de datos o un almácen de datos, pero también pude aplicar a funciones de un nivel superior de una aplicación como soft deletes donde la información no es realmente eliminada, sino es marcada como eliminada a tráves de un estatus.

Referencia: [CRUD](https://developer.mozilla.org/es/docs/Glossary/CRUD)

## En la consola Artisan...

El CRUD está asociado a una tabla de la base de datos. Cree el modelo que la represente.

```
php artisan make:modelo Anecdota
```

Un controlador gestionará las acciones del CRUD. Agrega _--resource_ para crear los métodos _por defecto_ para un CRUD.

```
php artisan make:controller AnecdotaController  --resource
```

Utiliza _route:list_ para ver las rutas que se generaron con el controlador. Si no las ves, aún no las has definido en el archivo de rutas.

```
php artisan route:list
```

## En el controlador...

La instrucción anterior genera los métodos: __index(), create(), store(), show(), edit(), update() y destroy()__. Cada una tiene una funcionalidad para el CRUD. 

- __Index()__. Carga la página desde donde esperaríamos encontrar las acciones del CRUD. Estas acciones pueden estar en otras páginas tambien, todo depende del diseño. También funciona como la página _maestro_ en una plantilla del tipo _maestro-detalle_.
- __create()__. Esta página carga el formulario para _inserciones_ en la base de datos.
- __store()__. Aquí es a donde apunta el _action_ del formulario _create()_. En esta se ejecuta la _inserción_.
- __show()__. Esta página carga la información de un _registro_ de la base de datos asociado a este CRUD. Es la página _detalle_ de la plantilla _maestro-detalle_.
- __edit()__. Carga el formulario para la edición de un registro.
- __update()__. Realiza la operación con la base de datos para la actualización. Es el _action_ del formulario _edit()_. 
- __destroy()__. Elimina un registro.

No es necesario usar todas, de nuevo, dependerá del diseño. Tampoco es necesario usar el _--resource_ al crear el controlador, puedes escribirlo en el archivo generado. Tampoco es necesario que se llamen igual, pero es la convención.

```php #
namespace App\Http\Controllers;

use Illuminate\Http\Request;

class AnecdotaController extends Controller
{
    /**
     * Display a listing of the resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function index()
    {
        //
    }

    /**
     * Show the form for creating a new resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function create()
    {
        //
    }

    /**
     * Store a newly created resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return \Illuminate\Http\Response
     */
    public function store(Request $request)
    {
        //
    }

    /**
     * Display the specified resource.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function show($id)
    {
        //
    }

    /**
     * Show the form for editing the specified resource.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function edit($id)
    {
        //
    }

    /**
     * Update the specified resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function update(Request $request, $id)
    {
        //
    }

    /**
     * Remove the specified resource from storage.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function destroy($id)
    {
        //
    }
}
```

## En la ruta...

Todos los métodos del controlador generan una ruta. Para no indicar una por una las que forman parte del resource, utilice:

```php #
use App\Http\Controllers\AnecdotaController;
 
Route::resource('anecdotas', AnecdotaController::class);
```

!!!
Note que 'Anecdotas' está en plural, y AnecdotaController en singular. La razón es que tendremos URL's en las que queremos "ver" un solo elemento como en el SHOW donde la URL sería _/anecdotas/{anecdota}_. Es solo una convención no obligatoria.
!!!

!!!
Es posible usar otro nombre para las URL, por ejemplo 'historias' en lugar de 'anecdotas', pero en la mayoría de los casos cambiarlo sería confuso. Las URL's serían del tipo _/historias/{anecdota}_. El controlador sería el mismo. 
!!!

Las URL's que se generarían serían las siguientes:

| Verb    | URI | Action  | Route Name |
|---------|-----|---------|------------|
| GET | /anecdotas | index |  anecdotas.index | 
| GET | /anecdotas/create | create | anecdotas.create |
| POST    | /anecdotas | store |  anecdotas.store |
| GET | /anecdotas/{anecdota} | show |    anecdotas.show |
| GET | /anecdotas/{anecdota}/edit |   edit |   anecdotas.edit |
| PUT/PATCH  | /anecdotas/{anecdota} | update |  anecdotas.update |
| DELETE | /anecdotas/{anecdota} | destroy | anecdotas.destroy |

El "verb" es el método asociado a la URL, que típicamente son los GET y POST más PUT, PATCH para insertar y editar respetivamente, mientras que DELETE será para las eliminaciones.

El "Action" corresponde al método del controlador asociado a la URI.

El "route name" es la forma corta de usar la URI en los enlaces.

!!!
Las partes entre llaves "{}" indican que ese es un dato dinámico. Ej. /anecdotas/{anecdota} en la aplicación en uso sería algo como _/anecdotas/15_ donde {anecdota} se sustituye por el ID de la anécdota.
!!!



## Convenciones...

- Usar _--resource_ genera los métodos del CRUD pero no es obligatorio.
- Los nombres de los métodos no son obligatorios.
- No es necesario utilizarlos todos.
- Los formularios deberán llevar la protección CSRF. Ver [CSRF protection](https://laravel.com/docs/9.x/csrf#main-content) del manual de Laravel para los detalles.

## Videos de referencia para realizar un CRUD

- [Idea del proyecto](https://www.loom.com/share/34d1d27909ac4acba62a579cb4da4985?sharedAppSource=personal_library)
- [La base de datos](https://www.loom.com/share/f7a3a14799e2418a8a18d633e142146a?sharedAppSource=personal_library)
- [La configuración inicial](https://www.loom.com/share/3ac5a4978de44b9e976d567f090fbe6b?sharedAppSource=personal_library)
- [Entendiendo el MVC](https://www.loom.com/share/01a2ad8f02c4448bbe3f93f6355d8b98?sharedAppSource=personal_library)
- [Creando un controlador con RESOURCE](https://www.loom.com/share/90d929cca403475d90101f09f3cc02a2?sharedAppSource=personal_library)
- [Sobre los modelos](https://www.loom.com/share/66d3b34a2bf64b8d9e88db6d4a47cf34?sharedAppSource=personal_library)
- [Sobre los controladores](https://www.loom.com/share/8dd7265facf14c9aa13256e6915e4ed4?sharedAppSource=personal_library)
- [Sobre las vistas](https://www.loom.com/share/884532b479bf44218b345bff95647ab7?sharedAppSource=personal_library)
