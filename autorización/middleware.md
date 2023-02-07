# Middleware

Los _middleware_ son como _puertas intermedias_ que permiten o niegan el acceso a un recurso. Formalmente son:

> Middleware provide a convenient mechanism for inspecting and filtering HTTP requests entering your application

El _middleware_ más notorio es el del _auth_ que redirecciona al login a los usuarios que no están _autentificados_, es decir, que no se han _logueado_ en el sistema. 

La instrucción en __Artisan__ para crear un middleware:

```
php artisan make:middleware EnsureTokenIsValid

```

El archivo resultante se almacena en __app/Http/Middleware__. 

La sintaxis del middleware:

```php #
<?php
 
namespace App\Http\Middleware;
 
use Closure;
 
class EnsureTokenIsValid
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @return mixed
     */
    public function handle($request, Closure $next)
    {
        if ($request->input('token') !== 'my-secret-token') {
            return redirect('home');
        }
 
        return $next($request);
    }
}
```

En este ejemplo se entiende que se creó un middleware con el nombre __EnsureTokenIsValid__ que se _asegura de que un token sea válido_. El método _handle($request, Closure $next)_ recibe la petición y tiene el recurso al cuál dirigirse si se _cruza_ el middleware. En la línea 18 se comprueba si el _token_ que viene del formulario ($request->input) es distinto al que está alojado en la cadena _my-secret-token; de ser así se redirecciona al _home_ del proyecto y sino se continúa a donde apuntaba el recurso ($next($request)).

## Registro del middleware

El middleware ya se creó ahora debe registrarse en el __kernel__ para su uso. Si el middleware va a utilizarse en cada petición HTTP, es decir, cada cambio de página, deberá registrarse globalmente de lo contrario se indicará en las rutas donde se requiera.

Para uso global, en cada petición de HTTP:

```php #
protected $middleware = [
        // \App\Http\Middleware\TrustHosts::class,
        \App\Http\Middleware\TrustProxies::class,
        \Illuminate\Http\Middleware\HandleCors::class,
        \App\Http\Middleware\PreventRequestsDuringMaintenance::class,
        \Illuminate\Foundation\Http\Middleware\ValidatePostSize::class,
        \App\Http\Middleware\TrimStrings::class,
        \Illuminate\Foundation\Http\Middleware\ConvertEmptyStringsToNull::class,
    ];
```

Para uso en las rutas deberá asignarse un nombre, como el del ejemplo _auth_:

```php #
protected $routeMiddleware = [
        'auth' => \App\Http\Middleware\Authenticate::class,
        'auth.basic' => \Illuminate\Auth\Middleware\AuthenticateWithBasicAuth::class,
        'cache.headers' => \Illuminate\Http\Middleware\SetCacheHeaders::class,
        'can' => \Illuminate\Auth\Middleware\Authorize::class,
        'guest' => \App\Http\Middleware\RedirectIfAuthenticated::class,
        'password.confirm' => \Illuminate\Auth\Middleware\RequirePassword::class,
        'signed' => \Illuminate\Routing\Middleware\ValidateSignature::class,
        'throttle' => \Illuminate\Routing\Middleware\ThrottleRequests::class,
        'tieneHabs'=> \App\Http\Middleware\TieneHabilidades::class,
        'verified' => \Illuminate\Auth\Middleware\EnsureEmailIsVerified::class,
    ];
```

En el caso de las rutas del archivo __routes\web__ se usa de la siguiente forma:

```php 
Route::get('habilidades/create',[HabilidadesController::class,'create'])
        ->middleware(['auth']);
```