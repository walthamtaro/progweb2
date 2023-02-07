# Mutators

Los _mutators_ de Laravel son una forma de _alterar_ el dato que se va a almacenar en la base de datos. Esto posiblemente porque la cadena original puede optimizarse. Por ejemplo, guardar 0 o 1 en lugar de _activo_ o _inactivo_.

Se definen en los __modelos__ del proyecto. Pueden haber tantos como atributos o columnas de la tabla referenciada por el modelo. 

Debes agregar el __use__ del cast _Attribute_ al modelo en el que vayas a crearlos.

```php #  
use Illuminate\Database\Eloquent\Casts\Attribute;

```

La sintaxis es la de una función protegida asociada al atributo. El método __make__ es un buen indicador de si hay algún error en la sintaxis como resultado de la versión de laravel utilizada. Este ejemplo está basado en Laravel 9, en otras versiones no será reconocido. Verifica siempre la versión en la que trabajas para usar la sintaxis adecuada.

La palabra __set__ nos indica que es un mutator. Es el indicador de un _setter_, de la _modificación_ de un dato previo a su almacenamiento. La función debe nombrarse como el atributo al que referencia. Debe usar también el formato _camel case_ en sustitución del _underlying_ usado en el atributo.

!!!
_camel case_ es una convención tipográfica en la que las palabras se juntan y la inicial de cada una se escribe en mayúscula para diferenciarlas, así _nombreCompleto_ es un ejemplo de camel case. 
!!!

!!!
El formato _underlying_ de Laravel es la separación por guión bajo en las palabras que conforman el nombre de un campo de tabla en la base de datos. el ejemplo de _user_id_ es underlying.
!!!

__El mutator está descrito en la línea 5.__

```php #
protected function firstName(): Attribute
    {
        return Attribute::make(
            get: fn ($value) => ucfirst($value),
            set: fn ($value) => strtolower($value),
        );
    }
```

En este ejemplo se entiende que hay un atributo _first_name_ en la base de datos en la tabla _user_ y cuando el atributo vaya a almacenarse por resultado de una _inserción_ o _actualización_ se aplicará un cambio a minúsculas por el método _strtolower()_.

!!!
Es de notarse que el _get_ y _set_ de un atributo se definen en la misma función protegida. Pueden estar ambos o solo alguno de ellos, según el diseño de su proyecto.
!!!