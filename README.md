# Puzzle Admin Bundle

Symfony project.

### **Installation**


Open a command console, enter your project directory and execute the following command to download the latest stable version of this bundle:

```yaml
composer require webundle/puzzle-admin-bundle
```

### **Step 1: Enable bundles**
Enable admin bundle by adding it to the list of registered bundles in the `app/AppKernel.php` file of your project:

```php
<?php
// app/AppKernel.php

// ...
class AppKernel extends Kernel
{
    public function registerBundles()
    {
        $bundles = array(
            // ...
            new Symfony\Bundle\FrameworkBundle\FrameworkBundle(),
            new Symfony\Bundle\SecurityBundle\SecurityBundle(),
            new Symfony\Bundle\TwigBundle\TwigBundle(),
            new Symfony\Bundle\MonologBundle\MonologBundle(),
            new Symfony\Bundle\SwiftmailerBundle\SwiftmailerBundle(),
            new Doctrine\Bundle\DoctrineBundle\DoctrineBundle(),
            new Sensio\Bundle\FrameworkExtraBundle\SensioFrameworkExtraBundle(),
            new JMS\DiExtraBundle\JMSDiExtraBundle($this),
            new JMS\AopBundle\JMSAopBundle(),
            new JMS\SecurityExtraBundle\JMSSecurityExtraBundle(),
            new Knp\Bundle\PaginatorBundle\KnpPaginatorBundle(),
            new Knp\DoctrineBehaviors\Bundle\DoctrineBehaviorsBundle(),
            new Symfony\Bundle\AsseticBundle\AsseticBundle(),
            new FOS\JsRoutingBundle\FOSJsRoutingBundle(),
            new Puzzle\AdminBundle\AdminBundle(),
        );

        // ...
    }

    // ...
}
```

### **Step 2: Define hosts values**
Defined hosts values by adding it in the `app/config/parameters.yml` file of your project:

```yaml
paremeters:
	...
	host: <hostname>
	admin_host: 'admin.%host%' # or %host% if you don't use subdomain
	admin_prefix: '/' # or /admin/ if with you don't use subdomain
```


### **Step 3: Security**
Configure security by adding it in the `app/config/security.yml` file of your project:

```yaml
security:
	...
    role_hierarchy:
        ROLE_ADMIN: ROLE_USER
        ROLE_SUPER_ADMIN: [ROLE_ADMIN, ROLE_ALLOWED_TO_SWITCH]
	...
    access_control:
        - {path: ^/login, roles: IS_AUTHENTICATED_ANONYMOUSLY }
        - {path: ^%admin_prefix%, host: "%admin_host%", roles: ROLE_ADMIN }

```

### **Step 4: Register default routes**
Register default routes by adding it in the `app/config/routing.yml` file of your project:

```yaml
....
admin:
    resource: "@AdminBundle/Resources/config/routing.yml"
    host: '%admin_host%'
    options:
        expose: true
        
fos_js_routing:
    resource: "@FOSJsRoutingBundle/Resources/config/routing/routing.xml"
```

### **Step 5: Install bower components**
Open CLI terminal and generate AdminBundle into src folder. After that, go into bundle folder and install bower components by typing:

```yaml
....
bower install
```

### **Step 6: Default configurations**
Configure admin bundle by adding it in the `app/config/config.yml` file of your project:

```yaml
imports:
    - { resource: parameters.yml }
    - { resource: security.yml }

# Put parameters here that don't need to change on each machine where the app is deployed
# http://symfony.com/doc/current/best_practices/configuration.html#application-related-configuration
parameters:
    locale: fr

framework:
    #esi:             ~
    # default_locale: fr
    translator:
        fallbacks: ["%locale%"]

    secret:          "%secret%"
    router:
        resource: "%kernel.root_dir%/config/routing.yml"
        strict_requirements: ~
    form:            ~
    csrf_protection: ~
    validation:      { enable_annotations: true }
    #serializer:      { enable_annotations: true }
    templating:
        engines: ['twig']
    default_locale:  "%locale%"
    trusted_hosts:   ~
    trusted_proxies: ~
    session:
        # handler_id set to null will use default session handler from php.ini
        handler_id:  ~
    fragments:       ~
    http_method_override: true
    assets:
        version: 2
        version_format: '%%s?version=%%s'
        base_path: ~

# Twig Configuration
twig:
    debug:            "%kernel.debug%"
    strict_variables: "%kernel.debug%"
    paths:
        '%kernel.root_dir%/../web': template

# Assetic Configuration
assetic:
    debug:          "%kernel.debug%"
    use_controller: false
    java: /usr/bin/java
    filters:
        cssrewrite: ~
# Doctrine Configuration
doctrine:
    dbal:
        driver:   pdo_mysql
        host:     "%database_host%"
        port:     "%database_port%"
        dbname:   "%database_name%"
        user:     "%database_user%"
        password: "%database_password%"
        charset:  UTF8

    orm:
        auto_generate_proxy_classes: "%kernel.debug%"
        naming_strategy: doctrine.orm.naming_strategy.underscore
        auto_mapping: true
        
# Swiftmailer Configuration
swiftmailer:
    transport: "%mailer_transport%"
    host:      "%mailer_host%"
    username:  "%mailer_user%"
    password:  "%mailer_password%"
    spool:     { type: memory }

# Knp Paginator
knp_paginator:
    page_range: 5                      # default page range used in pagination control
    default_options:
        page_name: page                # page query parameter name
        sort_field_name: sort          # sort field query parameter name
        sort_direction_name: direction # sort direction query parameter name
        distinct: true                 # ensure distinct results, useful when ORM queries are using GROUP BY statements
        # filter_field_name: filterField  # filter field query parameter name
        # filter_value_name: filterValue  # filter value query parameter name
    template:
        pagination: AppBundle:Pagination:sliding.html.twig     # sliding pagination controls template
        sortable: AppBundle:Pagination:sortable_link.html.twig # sort link template
        filtration: AppBundle:Pagination:filtration.html.twig  # filters template

# Knp Doctrine Behaviors
knp_doctrine_behaviors:
    blameable:      true
    geocodable:     ~     # Here null is converted to false
    loggable:       ~
    timestampable:  true
    sluggable:      true
    soft_deletable: true
    # All others behaviors are disabled
    
# Liip
liip_imagine :
    ...
    filter_sets :
        ...
        # Logo naviagtion
        logo_navigation :
            quality : 100
            filters :
                thumbnail  : { size : [100, 100], mode : outbound }

# Admin Configuration
admin:
    website:
        title: 'Lorem ipsum dolor' # Customize with your own admin name
        description: 'Lorem ipsum dolor' # Customize with your own admin description
        email: 'johndoe@exemple.ci' # Customize with your own admin email
    time_format: "H:i" # customize time format see php.net time format
    date_format: "d-m-Y" # customize date format see php.net date format
    modules_available: 'admin'
    navigation:
        nodes:
            dashboard:
                label: 'admin.dashboard.navigation'
                translation_domain: 'admin'
                path: admin_homepage
                attr:
                    class: 'fa fa-home' # customize icon class
                parent: ~
                user_roles: ['ROLE_ADMIN']
```
