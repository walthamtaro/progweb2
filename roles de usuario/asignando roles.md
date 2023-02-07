# Asignando roles al user

Crear los roles y asignar los permisos son el principio. Hay que asignar al usuario (user) el rol que tiene o el permiso que se le concede. 

Primero el modelo _user_ debe usar el _trait_ llamado _HasRoles_. Note que va en dos lugares:

```php 
use Spatie\Permission\Traits\HasRoles;

class User extends Authenticatable
{
    use HasApiTokens, HasFactory, Notifiable, HasRoles;
```

Una ves asignado el _trait_ al modelo _user_ podrá asignar el rol o permiso en donde vea conveniente. En este ejemplo se asigna el rol _tutorado_ al momento del registro, entendiendo que solo se registran alumnos que serán tutorados. En otro caso se puede hacer un CRUD de usuarios a los que manualmente se les asigne el rol.

```php #
public function store(Request $request)
    {
        $request->validate([
            'name' => 'required|string|max:255',
            'email' => 'required|string|email|max:255|unique:users',
            'password' => ['required', 'confirmed', Rules\Password::defaults()],
        ]);

        $user = User::create([
            'name' => $request->name,
            'email' => $request->email,
            'password' => Hash::make($request->password),
        ]);

        event(new Registered($user));

        //asignar el rol
        $user->assignRole('tutorado');

        Auth::login($user);

        return redirect(RouteServiceProvider::HOME);
```

En la base de datos se verá el cambio en la tabla _model_has_roles_ ya que se asignó un rol. Si se asigna un permiso solamente estaría en la tabla _model_has_permissions_. 