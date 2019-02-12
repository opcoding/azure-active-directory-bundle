# knowledge-base
Knowledge Base Project

### Routing

Add the following code in the your `config/routes.yaml`

```yaml
opco_aad_bundle:
    resource: '@OpcoAADBundle/Resources/config/routes.yaml'
```

Edit the bundles.php file and add the following code : 
```php
<?php
return [
    KnpU\OAuth2ClientBundle\KnpUOAuth2ClientBundle::class => ['all' => true],
		\OpcodingAADBundle\OpcoAADBundle::class => ['all' => true]
];
```

Create a new yaml config with this : 
```yaml
knpu_oauth2_client:
    clients:
        azure:
            type: azure
            client_id: '%env(resolve:AZURE_CLIENT_ID)%'
            client_secret: '%env(resolve:AZURE_CLIENT_SECRET)%'
            redirect_route: connect_azure_check
            redirect_params: {}
            api_version: '1.6'
```


Then edit the `config/packages/security.yml` and add the following code : 

```yaml
providers:
        app:
            entity:
                class: OpcoAADBundle:User
                property: username
    firewalls:
        dev:
            pattern: ^/(_(profiler|wdt)|css|images|js)/
            security: false
        main:
            pattern: ^/
            anonymous: ~
            logout:
                path: app_logout
                target: /
            guard:
                authenticators:
                    - OpcodingAADBundle\Security\AzureAuthenticator
```

