# Пакет Laravel-Sms

Этот пакет предоставляет класс для отправки смс и предустановленные реализации популярных провайдеров.

## Установка

Подключите пакет командой:
```bash
composer require nutnet/laravel-sms ~0.1
```

После того как пакет был установлен добавьте его сервис-провайдер в config/app.php:
```php
// config/app.php
'providers' => [
    ...
    Nutnet\LaravelSms\ServiceProvider::class,
];
```

Теперь необходимо перенести конфигурацию пакета в Laravel:
``` bash
php artisan vendor:publish
```

## Настройка
Все настройки находятся в файле `config/nutnet-laravel-sms.php`.
Настройки по умолчанию указаны ниже (используется отправка смс в лог-файл).

```php
/**
 * название класса-провайдера
 * @see Nutnet\LaravelSms\Providers
 */
'provider' => env('NUTNET_SMS_PROVIDER', \Nutnet\LaravelSms\Providers\Log::class),

/**
 * настройки, специфичные для провайдера
 */
'provider_options' => [
    'login' => env('NUTNET_SMS_LOGIN'),
    'password' => env('NUTNET_SMS_PASSWORD'),
],
```

### Поддерживаемые провайдеры смс услуг

#### Log
Используется для локальной разработки. Смс-сообщения записываются в файл лога.

#### SMPP
Отправка соообщений через протокол SMPP. Требует для работы пакет `franzose/laravel-smpp`.

#### Sms.ru
Отправка сообщений через провайдера Sms.ru. Требует для работы пакет `zelenin/smsru`.
В настройках провайдера требуется указать логин и пароль:
```php
// config/nutnet-laravel-sms.php
'provider_options' => [
    'login' => env('NUTNET_SMS_LOGIN'),
    'password' => env('NUTNET_SMS_PASSWORD'),
],
```

#### Smsc.ru
Отправка сообщений через провайдера Smsc.ru. Требует для работы установленный `curl`.
В настройках провайдера требуется указать логин и пароль:
```php
// config/nutnet-laravel-sms.php
'provider_options' => [
    'login' => env('NUTNET_SMS_LOGIN'),
    'password' => env('NUTNET_SMS_PASSWORD'),
],
```

## Отправка сообщений

Для отправки сообщений используется класс `Nutnet\LaravelSms\SmsSender`.
Пример отправки:

```php
class IndexController extends Controller
{
    public function sendSms(Nutnet\LaravelSms\SmsSender $smsSender)
    {
        // отправка сообщения на 1 номер
        $smsSender->send('89193216754', 'Здесь текст сообщений');
        
        // отправка сообщения на несколько номеров
        $smsSender->sendBatch(['89193216754', '89228764523'], 'Здесь текст сообщений');
                
        // ...
    }
}
```