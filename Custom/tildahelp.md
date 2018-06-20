---
title: Полезные методы Tilda
layout: default
---

При работе с тильдой иногда надо  спарсить  куки в читабельный вид , вот рабочая функция.
``` 
function parseCookie($string){
$array = explode(';',$string);
$func = function($one){
    parse_str(trim($one), $output);
    return $output;
};
$array = array_map($func,$array);
$array =call_user_func_array('array_merge', $array);
return $array;
}
```