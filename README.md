Laravel Navigation
==================

A Laravel 4 package for adding multiple, database driven, menus to a site

## Installation

Add the following to you composer.json file

    "fbf/laravel-navigation": "dev-master"

Run

    composer update

Add the following to app/config/app.php

    'Fbf\LaravelNavigation\LaravelNavigationServiceProvider'

Publish the config

    php artisan config:publish fbf/laravel-navigation

Run the migration

    php artisan migrate --package="fbf/laravel-navigation"

Ensure the navigation `types` are set correctly in the config file. See the config file for comprehensive examples

Run the seed (this will create root nodes for each of your navigation `types`)

	php artisan db:seed --class=Fbf\LaravelNavigation\NavItemsTableSeeder

Build your menus in the database, or if you are using FrozenNode's Laravel Administrator, see the info below

## Usage

The package comes with a View Composer which you can attach to any view in your app. E.g.

	// app/routes.php
	View::composer('layouts.master', 'Fbf\LaravelNavigation\NavigationComposer');

This is responsible for generating your menu data.

Now to render the menus, you just need to do the following in your view:

	{{ $MainNavigation }}

This will render the 'Main' menu. If you had configured another menu called 'Footer', you would render this by adding the following to your view:

	{{ $FooterNavigation }}

Basically, whatever `types` you set up in the config file, that type's menu is in a view variable called "< type >Navigation".

If you need to a output a menu in a view file but you've attached the composer to a layout, e.g. you want to render a sidebar
menu inside the pages.view view file, the $SidebarNavigation variable won't be available since the composer won't have executed
yet, it runs when the master layout is rendered, which happens after your view. In this case, just attach the composer to the
pages.view view as well as the layout. The view composer won't create them again. E.g.

```php
View::composer(array(
	'layouts.master',
	'laravel-pages::page',
), 'Fbf\LaravelNavigation\NavigationComposer');
```

## Administrator

You can use the excellent Laravel Administrator package by frozennode to administer your pages.

http://administrator.frozennode.com/docs/installation

A ready-to-use model config file for the NavItem model (navigation.php), including custom actions to reorder nodes in the hierarchy, is provided in the src/config/administrator directory of the package, which you can copy into the app/config/administrator directory (or whatever you set as the model_config_path in the administrator config file).