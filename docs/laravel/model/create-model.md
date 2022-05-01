---
sidebar_position: 1
---

# Création d'une vue avec PHPArtisan
````php
php artisan make:model *Nom du model*
````

# Guide | Bloc-notes pour nico

## Créer un model
Dans le model supprimer tous les usage et juste ajouter :
```php
use Jenssegers\Mongodb\Eloquent\Model; 
```

Ensuite dans le model supprimer le "HasFactory" et ajouter
```php
protected $connection = 'mongodb';
```

Mettre dans API comme dans Web les ressource comme ceci :
```php
Route::resource('tests', TestController::class)->only(['destroy', 'show', 'store', 'update']);
```

## Artisan 
Afin de pouvoir créer une entité avec ses dépendances

```php
php artisan make:model *Nom du model* -a -r 
```

Le -a est pour générer toutes les foncitonnalités avec le model (Controller, Migration, Factory, Request, Seeders, etc...);
Le -r est pour générer les méthodes Resources dans le controller (show, store, create, update, destroy);

## Factories 
Les factories sont incompatibles avec le package Jenssegers MongoDB.
De ce fais, l'alimentation en données doivent être fais à la main.

## Seeders 
Étant donné que les factories ne sont pas compatible, les seeders n'ont aucune utilitée.

## Authentification 
Système Auth Laravel Fortify.
Dans les vues comme le code utiliser 
```php
    auth()->user()->*Donnée cherché ou méthode nécessaire*
```
afin de récupérer des données propres à l'utilisateur ou utiliser les méthodes de "relation" afin de chercher des données d'une relation.


## Les différentes fonctions d'un controller classique
    - Index() => Permet de retourner la page d'accueil " Pour exemple, La fonction index du model article retournera tous les articles vers une vue
    - Show() => L'id de l'article est passé en argument, cette fonction permet donc d'afficher les détails d'un article
    - Create() => Permet de retourner la vue où se trouve le formulaire de création de l'article  (Esthetique).
    - Store() => Permet de traiter et sauvegarder les données provenant du create()
    - update() => Comme son nom l'indique, permet de mettre à jour un article.
    - destroy() => Permet de détruire toutes traces d'un article
## Sauvegarder une donnée dans la base de donnée
Afin de sauvegarder une donnée il faut avant tout créer un formulaire dans une vue et configurer la fonction create() du controlleur. (Ne pas oublier de faire le routage aussi).

Ensuite si pour exemple notre formulaire dans la vue redirigé par la fonction create() ressemble à ceci :
```html
<form action="{{ route('example.store') }}" method="post">
    @csrf
    <input type="text" name="name" id="name">
    <button type="submit">Envoyer</button>
</form>
```
** Attention avec laravel les {{ php }} servent à délimiter du php aux html. Et ne surtout pas oublié le csrf **

Dans notre controlleur, fonction store. 
Notre code ressemblerais à ceci :
```php
public function store(Request $request)
    {
        $example = Example::create([
            'name' => $request->name
        ]);

        return redirect()->route('accueil');
    }
```
Afin d'éviter tous soucis, il faut que dans le modèle il faut déclarer des données fillable pour faire de l'instanciation de masse. Ajoutez cette variable dans votre modele comme ceci :
```php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Jenssegers\Mongodb\Eloquent\Model;


class Example extends Model
{
    use HasFactory;

    protected $fillable = [
        'name',
    ];
}

```
### Résolution de soucis
Si lors du chargement de la page vous avez une erreur 403 et que votre fonction store ressemble à ceci :
```php
public function store(StoreExampleRequest $request)
    {
        $example = Example::create([
            'name' => $request->name
        ]);

        return redirect()->route('accueil');
    }
```

Il faut donc se rendre dans la classe ExampleRequest et autorisé sont utilisation comme ceci : 

```php
namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class StoreExampleRequest extends FormRequest
{
    public function authorize()
    {
        return true; // <= Par défaut est sur false. Doit être mis sur true
    }

    public function rules()
    {
        return [
            //
        ];
    }
}

```

En cas de soucis, se renseigner sur la documentation laravel disponible en fin de page.

Pour plus d'informations :
https://laravel.com/docs/9.x/fortify


Noco,
