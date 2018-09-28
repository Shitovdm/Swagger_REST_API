#Пример построения REST API на Swagger + Symfony
========================

## Установка

- Клонировать репозиторий:  
```git clone https://github.com/Shitovdm/Swagger_REST_API.git```

- Перейти в папку с проектом:  
```cd ./{PATH}```

- Установить [Composer](http://getcomposer.org/download)  

## Запуск

- Запустить локальный веб-сервер:  
```php bin/console server:run 127.0.0.1:80```  

## Использование

- HTTP WEB  

[http://127.0.0.1:80](http://127.0.0.1:80)  


Разработка  
========================

## Установка Symfony
Актуальная версия фреймворка на момент написания 3.4.

```composer create-project symfony/framework-standard-edition```

## Установка Swagger
Актуальная версия на момент написания 2.  
Возможна установка после добавления сгенерированного кода API.

**Требуется версия PHP >= 5.4.0**
 
- Установить [Composer](http://getcomposer.org/download) 
- Добавить зависимость в `composer.json`:   
```json
{
    // ...
    "repositories": [{
        "type": "path",
        "url": "src/Swagger/Server/"
    }]
    // ...
}
```

- Затем запустить:
```
composer require swagger/server-bundle:dev-master
```


## Добавление сгенерированного кода API

- Создать структуру `./scr/Swagger/Server/`
- Поместить туда файлы сренерированного бандла.
- Добавить бандл Swagger в AppKernel:
```php
<?php
// app/AppKernel.php
public function registerBundles()
{
    $bundles = array(
        // ...
        new Swagger\Server\SwaggerServerBundle(),
        // ...
    );
}
```

- Добавить роутинг Swagger:
```yaml
# app/config/routing.yml
swagger_server:
    resource: "@SwaggerServerBundle/Resources/config/routing.yml"
```

## Реализация интерфейсов API

- Создать новый бандл AppBundle (если не создан)

- Добавить в каталог нового бандла 

```yaml
# src/AppBundle/Resources/services.yml
services:
    # ...
    appbundle.api.service:
        class: AppBundle\Api\ServiceApi
        tags:
            - { name: "swagger_server.api", api: "service" }
    # ...
```

```php
<?php
// src/AppBundle/Api/ServiceApiInterface.php

namespace AppBundle\Api;

use Symfony\Component\HttpFoundation\File\UploadedFile;
use Swagger\Server\Model\ApiResponse;
use Swagger\Server\Model\Service;
use Swagger\Server\Api\ServiceApiInterface;

class ServiceApi implements ServiceApiInterface // An interface is autogenerated
{
    
    public function infoService(&$responseCode, array &$responseHeaders)
    {
        // Implement the operation ...
    }

    // ...
    
}
```

- Зарегистрировать привязку в `\app\config\config.yml`

```
imports:
   // ...
    - { resource: ../../src/AppBundle/Resources/config/services.yml }
```

## Внесение изменений и добавление новых маршрутов в API





