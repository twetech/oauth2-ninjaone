This package provides NinjaOne OAuth 2.0 support for the PHP League's [OAuth 2.0 Client](https://github.com/thephpleague/oauth2-client).

## Installation

To install, use composer:

```
composer require twetech/oauth2-ninjaone
```


## Usage

Usage is the same as The League's OAuth client, using `\League\OAuth2\Client\Provider\Ninjaone` as the provider.

### Authorization Code Flow

```php
$provider = new League\OAuth2\Client\Provider\Ninjaone([
    'clientId'          => '{ninjaone-client-id}',
    'clientSecret'      => '{ninjaone-client-secret}',
    'redirectUri'       => 'https://example.com/callback-url'
]);

if (!isset($_GET['code'])) {

    // If we don't have an authorization code then get one
    $authUrl = $provider->getAuthorizationUrl();
    $_SESSION['oauth2state'] = $provider->getState();
    header('Location: '.$authUrl);
    exit;

// Check given state against previously stored one to mitigate CSRF attack
} elseif (empty($_GET['state']) || ($_GET['state'] !== $_SESSION['oauth2state'])) {

    unset($_SESSION['oauth2state']);
    exit('Invalid state');

} else {

    // Try to get an access token (using the authorization code grant)
    $token = $provider->getAccessToken('authorization_code', [
        'code' => $_GET['code']
    ]);

    // Use this to interact with an API on the users behalf
    echo $token->getToken();
}
```
