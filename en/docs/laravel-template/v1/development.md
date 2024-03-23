---
title: Laravel-Template V1 - Development
permalink: /docs/laravel-template/v1/development
previous: /docs/laravel-template/v1/installation
previous_title: Installation
---

# Development

## <i class="icon-triangle-alert text-yellow-600"></i> The V1 of the Laravel-Template is no longer maintained. Please use the V2 instead. <i class="icon-triangle-alert text-yellow-600"></i>

- <a href="/docs/laravel-template/v1/" wire:navigate>Introduction</a>
- <a href="/docs/laravel-template/v1/installation" wire:navigate>Installation</a>
- <a href="/docs/laravel-template/v1/development" wire:navigate>Development</a>

The Laravel-Template uses Livewire 3.x and Alpine.js 3.x for the frontend.

The Documentation for
 - Livewire 3.x can be found [here](https://livewire.laravel.com).
 - Alpine.js 3.x can be found [here](https://alpinejs.dev).
 - Laravel can be found [here](https://laravel.com).

## Modals and Notifications

The Laravel-Template uses the `wire-elements/modal` library for modals and the `filament/notifications` library for Notifications.

The Documentation for
 - `wire-elements/modal` can be found [here](https://github.com/wire-elements/modal).
 - `filament/notifications` can be found [here](https://filamentphp.com/docs/3.x/notifications/installation).

### Modals
To create a modal, you need to create a new Livewire component and extend the `ModalComponent` class.

```php
<?php

namespace App\Livewire\Components\Modals\Admin;

use App\Http\Controllers\Auth\AuthController;
use App\Models\User;
use Exception;
use Filament\Notifications\Notification;
use LivewireUI\Modal\ModalComponent;

class UserDelete extends ModalComponent
{
    public $userId;

    public function deleteUser()
    {
        $user = User::find($this->userId);

        $user->delete();

        Notification::make()
            ->title('User deleted')
            ->success()
            ->send();

        activity('system')
            ->performedOn($user)
            ->causedBy(auth()->user())
            ->withProperty('name', $user->username . ' (' . $user->email . ')')
            ->withProperty('ip', request()->ip())
            ->log('user.deleted');

        return redirect()->route('admin-user-list');
    }

    public function render()
    {
        return view('livewire.components.modals.admin.user-delete');
    }
}
```

In the view, you can use the `x-modal` component to open the modal.

```html
<x-modal class="modal-bottom sm:modal-middle">
    <div class="text-center">
        <h2 class="text-2xl font-bold mb-4">Delete User</h2>
        <p class="mb-3">Are you sure you want to delete this user?</p>
    </div>
    <div class="flex justify-center modal-action">
        <form method="dialog">
            <button class="btn btn-neutral" wire:click="$dispatch('closeModal')">Cancel</button>
            <a role="button" wire:click="deleteUser"
               class="btn btn-error">Delete User</a>
        </form>
    </div>
</x-modal>
```

### Notifications
To create a notification, you need to use the `Notification` class.

```php
Notification::make()
    ->title('User deleted')
    ->success()
    ->send();
```

## Directory Structure
- `app/Livewire` - Contains all Livewire components.
- `resources/views/livewire` - Contains all Livewire views.
- `resources/views/components/layouts` - Contains all layouts.
- `app/Http/Controllers/API` - Contains all API controllers.
- `app/Http/Controllers/Auth` - Contains all Auth controllers.
- `lang` - Contains all language files.
