# copia del atributo

Es una modificación del tipo de dato que tiene asociado un atributo. Por ejemplo, un campo que guarda 0 y 1, quizás como tipo de dato entero o varchar, puede ser modificado por el _cast_ para que se interprete como dato booleano y así utilizarlo directamente en condicionales.

La sintaxis de un cast

```php #
protected $casts = [
        'is_admin' => 'boolean',
    ];
```

En este ejemplo se entiende que hay una columna llamada _is_admin_ en la tabla que referencia este modelo y a la cuál se le cambia el tipo de dato como booleano, es decir, verdadero o falso. 

!!!
El tipo de dato se modifica durante su lectura y uso no en la base de datos. Este cambio no es permanente
!!!