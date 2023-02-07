# Accesors

Los _accesors_ de Laravel son una forma de dar _presentación_ a los datos provenientes de un modelo cuando estos están almacenados de una forma poco comprensible, familiar o poco usable para el usuario.

Se definen en los __modelos__ del proyecto. Pueden haber tantos como atributos o columnas de la tabla referenciada por el modelo. 

Debes agregar el __use__ del cast _Attribute_ al modelo en el que vayas a crearlos.

```php #  
use Illuminate\Database\Eloquent\Casts\Attribute;

```

La sintaxis es la de una función protegida asociada al atributo. El método __make__ es un buen indicador de si hay algún error en la sintaxis como resultado de la versión de laravel utilizada. Este ejemplo está basado en Laravel 9, en otras versiones no será reconocido. Verifica siempre la versión en la que trabajas para usar la sintaxis adecuada.

La palabra __get__ nos indica que es un accesor. Es el indicador de un _getter_, de la obtención de un dato de la base de datos y que será _modificado_ para su visualización. La función debe nombrarse como el atributo al que referencia. Debe usar también el formato _camel case_ en sustitución del _underlying_ usado en el atributo.

!!!
_camel case_ es una convención tipográfica en la que las palabras se juntan y la inicial de cada una se escribe en mayúscula para diferenciarlas, así _nombreCompleto_ es un ejemplo de camel case. 
!!!

!!!
El formato _underlying_ de Laravel es la separación por guión bajo en las palabras que conforman el nombre de un campo de tabla en la base de datos. el ejemplo de _user_id_ es underlying.
!!!

```php # 
protected function firstName(): Attribute
    {
        return Attribute::make(
            get: fn ($value) => ucfirst($value),
        );
    }
```

En este ejemplo se entiende que hay un atributo _first_name_ en la base de datos en la tabla _user_ y cuando el atributo sea utilizado en alguna vista la primera letra de la cadena almacenada se cambiará a mayúscula por el método _ucfirst()_.

El ejemplo en el modelo:

```php #
namespace App\Models;
 
use Illuminate\Database\Eloquent\Casts\Attribute;
use Illuminate\Database\Eloquent\Model;
 
class User extends Model
{
    /**
     * Get the user's first name.
     *
     * @return \Illuminate\Database\Eloquent\Casts\Attribute
     */
    protected function firstName(): Attribute
    {
        return Attribute::make(
            get: fn ($value) => ucfirst($value),
        );
    }
}
```