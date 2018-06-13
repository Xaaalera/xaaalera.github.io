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

php
``` 
$roistatv= isset($_COOKIE['roistat_visit']) ? $_COOKIE['roistat_visit'] : '';
file_get_contents("https://cloud.roistat.com/api/site/1.0/{PROJECT_ID}/event/register?event=cart_view&visit={$roistatv}&data[page]={$_SERVER['HTTP_REFERER']}&data[domain]={$_SERVER['SERVER_NAME']}");  
```
