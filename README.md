# Dataporten

```bash
composer require socialiteproviders/dataporten
```

## Installation & Basic Usage

Please see the [Base Installation Guide](https://socialiteproviders.com/usage/), then follow the provider specific instructions below.

### Add configuration to `config/services.php`

```php
'dataporten' => [
  'client_id' => env('DATAPORTEN_CLIENT_ID'),
  'client_secret' => env('DATAPORTEN_CLIENT_SECRET'),
  'redirect' => env('DATAPORTEN_REDIRECT_URI')
],
```

### Add provider event listener

Configure the package's listener to listen for `SocialiteWasCalled` events.

Add the event to your `listen[]` array in `app/Providers/EventServiceProvider`. See the [Base Installation Guide](https://socialiteproviders.com/usage/) for detailed instructions.

```php
protected $listen = [
    \SocialiteProviders\Manager\SocialiteWasCalled::class => [
        // ... other providers
        \SocialiteProviders\Dataporten\DataportenExtendSocialite::class.'@handle',
    ],
];
```

### Usage

You should now be able to use the provider like you would regularly use Socialite (assuming you have the facade installed):

```php
return Socialite::driver('dataporten')->redirect();
```

End session for application AND dataporten example.

```php
$idToken = auth()->user()->dataporten_id_token;
$endpointUri = env('DATAPORTEN_ENDSESSION_ENDPOINT');
$redirectUri = env('DATAPORTEN_LOGOUT_REDIRECT_URI');
$logoutUri = Socialite::driver('dataporten')->getLogoutUrl($idToken, $endpointUri, $redirectUri);

Auth::guard('web')->logout();

$request->session()->invalidate();

$request->session()->regenerateToken();

if($idToken) {
    return redirect()->away($logoutUri);
}

return redirect('/');
```
