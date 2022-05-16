---
sidebar_position: 1
---

# Gestion de la permission d'accès à la vue

## Créer la permission d'accès à la vue

Dans le controller `app/Http/Controllers/PermissionGestureController.php`, dans la fonction <code>createPermissions</code>, ajouter à la suite des permissions votre permission

<i>code générique</i>

```php
Permission::create(['name'=>'`nom_de_la_vue.laFonction`]);
```

Les différentes fonctions sont : 
<ul>
index
create
update
show
delete
</ul>

<i>Exemple :</i>

```php
Permission::create(['name'=>'`intervenant.index`]);
```


## Assigner la permission au rôle

Dans le controller `app/Http/Controllers/PermissionGestureController.php`, dans la fonction <code>createPermissions</code>, ajouter dans la partie `Assign permissions to roles`, la permission d'accéder à la vue

<i>code générique</i>

```php
  $ceo->givePermissionTo([..., 'nom_de_la_vue.laFonction']);
  $soignant->givePermissionTo([..., 'nom_de_la_vue.laFonction']);
  $patient->givePermissionTo([..., 'nom_de_la_vue.laFonction']);
```

En ajoutant la permission à un rôle il pourra accéder à la vue

<i>Exemple :</i>

```php
  $ceo->givePermissionTo([..., 'intervenant.index']);
  $soignant->givePermissionTo([..., 'intervenant.index']);
  $patient->givePermissionTo([..., 'intervenant.index']);
```


## Traitement de la vérification d'accès à la vue

Dans le controller correspondant à votre vue ajouter le constructeur permettant d'accéder à la vue

<i>Code générique :</i>

```php
  /**
   * Instantiate a new controller instance.
   *
   * @return void
   */
  public function __construct()
  {
    $this->middleware('`nom_de_la_vue.laFonction`')->only('laFonction');
  }
```

<i>Exemple :</i>

```php
  /**
   * Instantiate a new controller instance.
   *
   * @return void
   */
  public function __construct()
  {
    $this->middleware('intervenant.index')->only('index');
  }
```


## Ajouter la Route de la vue

Dans le fichier `routes/web.php` ajouter la `Route` permettant d'accéder à la vue

<i>Code générique :</i>

```php
Route::resource('<nom_de_la_vue>', ObjectEstateController::class)->middleware('auth');
```

<i>Exemple :</i>

```php
Route::resource('intervenant.index', ObjectEstateController::class)->middleware('auth');
```

## Récap

Nous venons de créer une `Permission`, de l'ajouter à des `Role`, Ajouter le traitement dans le `Controller` et l'ajout de la `Route` correspondant à la vue

## Tester

Il ne rste plus qu'à se rendre sur la vue pour tester si notre configuration est bonne.