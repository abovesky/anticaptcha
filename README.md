# anticaptcha

A Simple wrapper for anti-captcha.com

# Thanks

[maxlen/anticaptcha](https://github.com/maxlen/anticaptcha)

## Installation:
```php
composer require abovesky/anticaptcha
```

### Use for captcha ImageToText:

```php
$resCaptcha = new CaptchaService(
    $apiKey, // your apiKey from anti-captcha.com
    CaptchaService::TYPE_IMAGE_TO_TEXT,
    [
        'imgPath' => '/path/example.png', // path to captcha image
        'isBase64' => false
    ]
);

echo $resCaptcha->check(); // text-result

// or

$resCaptcha->check();
echo $resCaptcha->hashResult; // text-result
```

### Use for reCaptcha:

```php
// $html - it's html-code from $url
// $html = file_get_contents($url) or something like that

if (Captcha::isCaptcha($html)) {
    $html = self::CaptchaProc($url, $html);
    die('CAPTCHA RESOLVED');
}

private function CaptchaProc($url, $html)
{
    $resCaptcha = new CaptchaService(
        $apiKey, // your apiKey from anti-captcha.com
        CaptchaService::TYPE_NO_CAPTCHA_PROXYLESS,
        [
            'webSiteUrl' => $url,
            'html' => $html,
        ]
    );

    // echo PHP_EOL . "getWebSiteKey: " . $resCaptcha->getWebSiteKey();
    // echo PHP_EOL . "getWebSiteSToken: " . $resCaptcha->getWebSiteSToken();
    // echo PHP_EOL;
    // var_dump($resCaptcha->getResolveGetParams());

    if (!$resCaptcha->check()) {
        return false;
    }

    $resolveUrl = $url . '&g-recaptcha-response=' . $resCaptcha->hashResult;
    return file_get_contents($resolveUrl); // curl or something like that
}
```
