歡迎使用 PWFPAY SDK for PHP 。

## 環境要求
1. PWFPAY SDK for PHP 需要 PHP 5.5 以上的開發環境。

2. 使用 PWFPAY SDK for  PHP 之前 ，您需要先前往 https://pwf.com 申請開通賬號並完成開發者接入的一些準備工作，包括創建應用、為應用設置接口相關配置等。

3. 準備工作完成後，注意保存如下信息，後續將作為使用SDK的輸入。

* 加簽模式為公私鑰證書模式

`AppID`、`應用的私鑰`、`PWF公鑰`


## 快速使用

1. Composer 安裝
```
composer require pwf/paysdk 
```

2. 示例代碼
```php
<?php

require 'vendor/autoload.php';

use Pwf\PaySDK\Base\ApiClient;
use Pwf\PaySDK\Base\Config;
    
    
//加载配置文件
ApiClient::setOptions(getOptions());

try {

     //订单支付請求接口
    $params = [
        "trade_name" => "trade_name",
        "fiat_currency" => "EUR",
        "fiat_amount" => 0.01,
        "out_trade_no" => "20200326235526001",
        "subject" => "eur_pay",
        "timestamp" => 1657895835,
        "notify_url"=> "https://www.notify.com/notify",
        "return_url" => "https://www.return.com/return",
        "collection_model" => 1,
        "merchant_no" => "2022072091622963",
    ];

    $result = ApiClient::wallet()->payAddress($params);
    print_r($result);
    
    
    //异步回调通知
    $json_string = '{"ret":1000,"msg":"\u8bf7\u6c42\u6210\u529f","data":"WDlwdnBoSkFDeS96bVdIYjg4WUNaaXVuV3NTQ......."}';
    $result = ApiClient::notify()->pay($json_string);
    print_r($result);

} catch (Exception $e) {
    echo "調用失敗，". $e->getMessage(). PHP_EOL;;
}

function getOptions()
{
    $options = new Config();

    $options->apiUrl = "<-- 請填寫平台分配的接口域名，例如：https://xxx.pwf.com/ -->";
    $options->appToken = "<-- 請填寫您的appToken，例如：377b26eb8c25bd... -->";
    $options->merchantNo = "<-- 請填寫您的商戶號，例如：202207...964 -->";

    //語系(參考文檔中最下方語系表，如:TC)
    $options->lang = "TC";
    
    $options->merchantPrivateCertPath = "<-- 請填寫您的應用私鑰路徑，例如：/foo/MyPrivateKey.pem -->";
    $options->pwfPublicCertPath = "<-- 請填寫PWF平台公鑰證書文件路徑，例如：/foo/PwfPublicKey.pem -->";

    return $options;
}

```
