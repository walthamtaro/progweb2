# Gates

Es una funcionalidad para aplicar autorización en Laravel. Sencillamente indica si un usuario tiene el _permiso_ para realizar una operación/acción en la aplicación.

Se definen en el __boot__ del __App\Providers\AuthServiceProvider__ usando el _facade_: 

```php 
use Illuminate\Support\Facades\Gate;

```

Siempre utiliza una instancia del usuario que está registrado y opcionalmente puede recibir argumentos adicionales.

La sintaxis de un gate:

```php #
Gate::define('update-post', function (User $user, Post $post) {
        return $user->id === $post->user_id;
    });
```

En este ejemplo se crea una _gate_ nombrada _update-post_ que recibe una instancia del modelo _user_ y del modelo _post_. Regresa una comprobación del id del usuario ($user->id) que debe coincidir de forma idéntica (===) con el id del usuario registrado en el post ($post->user_id). Si estos coinciden entonces se puede realizar lo que diga el método en el que es llamado de lo contrario se negará la acción.

## Código completo del AuthServiceProvider

```php #
use App\Policies\PostPolicy;
use Illuminate\Support\Facades\Gate;
 
/**
 * Register any authentication / authorization services.
 *
 * @return void
 */
public function boot()
{
    $this->registerPolicies();
 
    Gate::define('update-post', [PostPolicy::class, 'update']);
}
```

## En el controlador

El gate ya está creado ahora debe ser utilizado en el método de controlador donde sea necesario. En este ejemplo iría en el método asociado al _update_. Hay que usar el _facade_ del Gate.

```
use Illuminate\Support\Facades\Gate;

```

Entre las líneas 3 a 5 está el uso del gate _update-post_ al cuál solo se le envía el argumento _$post_ porque el del usuario lo recibe automáticamente. 

```php #
public function update(Request $request, Post $post)
    {
    	// El ! indica negación, allows es permitir
    	//diría algo como: si no se permite...
        if (! Gate::allows('update-post', $post)) {
            abort(403);
        }
 
        // Update the post...
    }
```

En este ejemplo se entiende que al acceder al método _update_ del controlador asociado al modelo _post_ primero se revisa _si no se permite el 'update-post' a este usuario_. En ese caso se llama al método _Abort()_ y se indica el código de error a mostrar que es el 403 (acceso prohibido).

Código completo del controlador:

````php #
<?php
 
namespace App\Http\Controllers;
 
use App\Http\Controllers\Controller;
use App\Models\Post;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Gate;
 
class PostController extends Controller
{
    /**
     * Update the given post.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \App\Models\Post  $post
     * @return \Illuminate\Http\Response
     */
    public function update(Request $request, Post $post)
    {
        if (! Gate::allows('update-post', $post)) {
            abort(403);
        }
 
        // Update the post...
    }
}
```