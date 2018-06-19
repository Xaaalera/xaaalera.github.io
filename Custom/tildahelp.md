---
title: Полезные методы Tilda
layout: default
---

При работе с тильдой иногда надо  спарсить  куки в читабельный вид , вот рабочая функция.
``` 
function parceCookie($string){
$array = explode(';',$string);
$array2=array();
$func = function($one){
    parse_str(trim($one), $output);
    return $output;
};
$array = array_map($func,$array);
foreach ($array as $values){
$array2[key($values)] = $values[key($values)];
}
retunr $array2;
}
```