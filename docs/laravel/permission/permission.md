---
sidebar_position: 1
---

# Gestion de la permission d'accès à la vue

## Créer la permission d'accès à la vue

Dans le controller `app/Http/Controllers/PermissionGestureController.php`, dans la fonction <code>createPermissions</code>, ajouter à la suite des permissions votre permission

```php
Permission::create(['name'=>'`nom_de_la_vue`]);
```

dzdz


````php
php artisan make:controller *Nom du controlleur*
```