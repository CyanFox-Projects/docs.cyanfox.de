---
title: Laravel-Template V2 - Module Development
permalink: /docs/laravel-template/v2/module-development
previous: /docs/laravel-template/v2/development
previous_title: Development
---

# Module Development

- <a href="/docs/laravel-template/v2/" wire:navigate>Introduction</a>
- <a href="/docs/laravel-template/v2/installation" wire:navigate>Installation</a>
- <a href="/docs/laravel-template/v2/development" wire:navigate>Development</a>
- <a href="/docs/laravel-template/v2/module-development" wire:navigate>Module Development</a>

The Laravel-Template uses the `nwidart/laravel-modules` library for modules

*We already created some modules, you can find them [here](https://github.com/CyanFox-Projects/Laravel-Template-Modules)*

The Documentation for

- `nwidart/laravel-modules` can be found [here](https://laravelmodules.com/docs/v10/introduction).

## Create a new Module

To create a new module, you need to run the following command:

```bash
php artisan module:make <ModuleName>
```

To migrate the module, you need to run the following command:

```bash
php artisan module:migrate <ModuleName>
```

*If you want to use Livewire in the module, take a look at
the [Laravel Modules Livewire Documentation](https://laravelmodules.com/docs/v10/livewire).*

This will create a new module in the `Modules` directory.

## Integrate module views
To integrate your module views into some Laravel-Template views, add the following code to your module service provider:

```php
public function register(): void
{
    $this->app->register(RouteServiceProvider::class);

    app('integrate.views')->add([
        [
            'location' => 'home',
            'section' => 'home',
            'component' => 'yourmodule::some.view',
        ],
    ]);
}
```

You can use following sections and locations:
- Sidebar
  - section: `mobile.sidebar` with location: `sidebar`
  - section: `mobile.navbar.quickActions` with location: `sidebar`
  - section: `mobile.navbar.profileDropdown` with location: `sidebar`
  - section: `desktop.navbar.quickActions` with location: `sidebar`
  - section: `desktop.navbar.profileDropdown` with location: `sidebar`
  - section: `desktop.sidebar` with location: `sidebar`
- Admin Sidebar
    - section: `mobile.sidebar` with location: `admin.sidebar`
    - section: `mobile.navbar.quickActions` with location: `admin.sidebar`
    - section: `mobile.navbar.profileDropdown` with location: `admin.sidebar`
    - section: `desktop.navbar.quickActions` with location: `admin.sidebar`
    - section: `desktop.navbar.profileDropdown` with location: `admin.sidebar`
    - section: `desktop.sidebar` with location: `admin.sidebar`
- Admin
    - section: `dashboard` with location: `admin.dashboard`
- Home
    - section: `home` with location: `home`
- Profile
    - section: `overview` with location: `account.profile`

If you want to add a new section, you can add the following code to the Laravel-Template views:

```php
@forelse (app('integrate.views')->getAll() as $moduleComponent)
    @if($moduleComponent['section'] == 'overview' && $moduleComponent['location'] == 'account.profile')
        @component($moduleComponent['component'])
        @endcomponent
    @endif
@empty
@endforelse
```

## Custom Settings page

If your module needs a settings page, you can add a route with following name: `modules.<ModuleName>.settings`.
This route will be used to display the settings page in the Admin Module Management section.
