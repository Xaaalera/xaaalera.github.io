---
title: Полезные методы roistat
layout: default
---
## Полезные методы roistat чтобы не ползать постоянно в доку ##

Проксилид
```
$roistatData = array(
    'roistat' => isset($_COOKIE['roistat_visit']) ? $_COOKIE['roistat_visit'] : null,
    'key'     => '', // Ключ для интеграции с CRM, указывается в настройках интеграции с CRM.
    'title'   => '', // Название сделки
    'comment' => '', 
    'name'    => '', 
    'email'   => '', 
    'phone'   => '', 
        'fields'  => array(
        
        ),
);
  
file_get_contents("https://cloud.roistat.com/api/proxy/1.0/leads/add?" . http_build_query($roistatData));
```

если не работает `file_get_contents`
```
     function SendRequest($url){
        $ch = curl_init();
        curl_setopt ($ch, CURLOPT_URL, $url);
        curl_setopt ($ch, CURLOPT_CONNECTTIMEOUT, 5);
        curl_setopt ($ch, CURLOPT_RETURNTRANSFER, true);
        $contents = curl_exec($ch);
        return $contents;
    }
```
ссылка для просмотра доп инфы <a href="http://help.roistat.com/pages/viewpage.action?pageId=5898354">Roistat</a>


#### События ####
js
``` 
   $(function () {
            $(document).on('mousedown','selector',function () {
               roistat.event.send('register', {'domain':location.hostname,'page':location.href});
            });
        })
```
если `on` не работает 
``` 
$( 'selector' ).bind( "mousedown", function(e) {
    roistat.event.send('event', {'domain':location.hostname,'page':location.href});
});
```
Jivosite
``` 
    var userOnMessageSent = window.jivo_onMessageSent;
    window.jivo_onMessageSent = function () {
        if (userOnMessageSent) {
            userOnMessageSent();
        }
        roistat.event.send('jivo_firstMessageSent', {'url': document.location.href ,'domain' : location.hostname});
    };
    var userOnIntroduction = window.jivo_onIntroduction;
    window.jivo_onIntroduction = function () {
        if (userOnIntroduction) {
            userOnIntroduction();
        }
        if (jivo_api.chatMode() === 'offline') {
            roistat.event.send('jivo_onOfflineMessageSent', {'url': document.location.href, 'domain' : location.hostname});
        }
    };
    var userOnCallStart = window.jivo_onCallStart;
    window.jivo_onCallStart = function () {
        if (userOnCallStart) {
            userOnCallStart();
        }
        roistat.event.send('jivo_onCallStart', {'url': document.location.href, 'domain' : location.hostname});
    };
```
ловец лидов
``` 
   $(function () {
        $(document).on('mousedown','.roistat-lh-pulsator-circle',function () {
            roistat.event.send('click_lead_hunter', {'domain':location.hostname,'page':location.href});
        });
        $(document).on('mousedown','.roistat-lh-submit',function () {
            roistat.event.send('send_lead_hunter', {'domain':location.hostname,'page':location.href});
        });
    });
```

php
``` 
$roistatv= isset($_COOKIE['roistat_visit']) ? $_COOKIE['roistat_visit'] : '';
file_get_contents("https://cloud.roistat.com/api/site/1.0/{PROJECT_ID}/event/register?event=cart_view&visit={$roistatv}&data[page]={$_SERVER['HTTP_REFERER']}&data[domain]={$_SERVER['SERVER_NAME']}");  
```
#### Битрикс ####
События для инит 

 Корзина
```
AddEventHandler('sale', 'OnOrderAdd', 'SendOrderToRoistat');
function SendOrderToRoistat($ID, $arFields)
{

}
```
Формы 
``` 
AddEventHandler('form', 'onBeforeResultAdd', 'SendFormSpecialistToRoistat');
function SendFormSpecialistToRoistat($WEB_FORM_ID, &$arFields, &$arrVALUES)
{
}
```
Ещё формы 
``` 
AddEventHandler('main', 'OnBeforeEventAdd', 'SendFormToRoistat');

function SendFormToRoistat(&$event, &$lid, &$arFields)
{
}
```

Обновление свойства заказа
``` 
AddEventHandler('sale', 'OnOrderAdd', 'SendOrderToRoistat');
function SendOrderToRoistat($ID, $arFields)
{
    AddOrderProperty(29,'Корзина',$orderID);
}
function AddOrderProperty($prop_id, $value, $order)
{
    if (!strlen($prop_id)) {
        return false;
    }
    if (CModule::IncludeModule('sale')) {
        if ($arOrderProps = CSaleOrderProps::GetByID($prop_id)) {
            $db_vals = CSaleOrderPropsValue::GetList(array(),
                array('ORDER_ID' => $order, 'ORDER_PROPS_ID' => $arOrderProps['ID']));
            if ($arVals = $db_vals->Fetch()) {
                return CSaleOrderPropsValue::Update($arVals['ID'], array(
                    'NAME'           => $arVals['NAME'],
                    'CODE'           => $arVals['CODE'],
                    'ORDER_PROPS_ID' => $arVals['ORDER_PROPS_ID'],
                    'ORDER_ID'       => $arVals['ORDER_ID'],
                    'VALUE'          => $value,
                ));
            } else {
                return CSaleOrderPropsValue::Add(array(
                    'NAME'           => $arOrderProps['NAME'],
                    'CODE'           => $arOrderProps['CODE'],
                    'ORDER_PROPS_ID' => $arOrderProps['ID'],
                    'ORDER_ID'       => $order,
                    'VALUE'          => $value,
                ));
            }
        }
    }
}
```
#### Тильда #### 

``` 
https://xaaalera.github.io/Custom/tildahelp
```