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
    
    
    firewalls:
        admin_login:
            pattern: ^%admi_prefix%login$
            anonymous: ~

        admin:
            entry_point: admin.security.authentication_entry_point
            pattern: '^%admin_prefix%'
            host: '%admin_host%'
            provider: chain_provider
            access_denied_handler: security.access_denied_handler
            form_login:
                check_path: login_check
                login_path: admin_login
                success_handler: security.authentication_success_handler
                failure_handler: security.authentication_failure_handler
                csrf_token_generator: security.csrf.token_manager
            logout:
                path: /logout
                target: admin_homepage
                delete_cookies:
                    REMEMBERME: { path: null, domain: null}
            remember_me:
                secret: "%secret%"
                lifetime: 84400
                path: admin_homepage
                domain: ~
                always_remember_me: true
	...
    access_control:
        - {path: ^%admin_prefix%login, roles: IS_AUTHENTICATED_ANONYMOUSLY }
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
Overwrite, bower install folder by creating `.bowerrc` file in project folder root and typing into it:

```yaml
....
{
    "directory": "vendor/webundle/puzzle-admin-bundle/Puzzle/AdminBundle/Resources/public/libs/"
}
```
Create `bower.json` file in project folder root and copy into it: 

```yaml
....
{
  "name": "webundle-puzzle-admin",
  "version": "1.0.0",
  "authors": [
    "tzd"
  ],
  "description": "Webundle project",
  "main": [
    "index.php",
    "assets/less/main.less",
    "assets/js/altair_admin_common.js"
  ],
  "keywords": [
    "admin",
    "panel",
    "material",
    "design",
    "dashboard"
  ],
  "license": "http://themeforest.net/licenses",
  "homepage": "http://webundle.com",
  "private": true,
  "ignore": [
    "**/.*",
    "node_modules",
    "bower_components",
    "test",
    "tests"
  ],
  "dependencies": {
    "autosize": "~3.0.8",
    "c3js-chart": "~0.4.10",
    "clndr": "~1.2.14",
    "codemirror": "~5.5.0",
    "countUp.js": "~1.5.3",
    "d3": "<=3.5.5",
    "datatables": "~1.10.7",
    "datatables-colvis": "~1.1.2",
    "datatables-tabletools": "~2.2.4",
    "dense": "*",
    "fastclick": "~1.0.6",
    "fitvids": "~1.1.0",
    "fullcalendar": "~2.3.2",
    "handlebars": "~3.0.3",
    "ionrangeslider": "~2.0.12",
    "jquery": "~2.1",
    "jquery-icheck": "~1.0.2",
    "jquery-mapael": "~1.0.1",
    "jquery-mousewheel": "~3.1.13",
    "jquery.dotdotdot": "~1.7.3",
    "jquery.inputmask": "~3.1.63",
    "jquery.easy-pie-chart": "~2.1.6",
    "jquery.scrollbar": "~0.2.7",
    "jquery-ui": "~1.11.4",
    "kendo-ui-core": "~2015.2.720",
    "magnific-popup": "~1.0.0",
    "maplace.js": "~0.1.33",
    "marked": "~0.3.3",
    "metrics-graphics": "~2.6.0",
    "modernizr": "~2.8.3",
    "moment": "~2.10.3",
    "parsleyjs": "~2.1.2",
    "peity": "~3.2.0",
    "prism": "gh-pages",
    "selectize": "~0.12.1",
    "switchery": "~0.8.1",
    "uikit": "~2.21.0",
    "velocity": "~1.2.1",
    "weather-icons": "~1.3.2",
    "jquery.actual": "~1.0.16",
    "jquery-bez": "~1.0.11",
    "waypoints": "~3.1.1",
    "materialize-tags": "^1.2.3",
    "fontawesome": "^4.0",
    "jquery-typeahead": "^2.10.6",
    "ckeditor": "^4.11.4"
  },
  "resolutions": {
    "jquery": "~2.1",
    "fastclick": "~1.0.6"
  }
}
```

In project folder root, install bower components by typing it in CLI:

```yaml
bower install
```

### **Step 6: Default configurations**
Configure admin bundle by adding it in the `app/config/config.yml` file of your project:

```yaml
imports:
    - { resource: parameters.yml }
    - { resource: security.yml }

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

### **Step 7: Launch admin install**
In project folder root, type:

```yaml
php bin/console puzzle:admin-install
```
