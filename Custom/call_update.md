---
title: Обновление в истории звонка.
layout: default
---
```  
<?php
/**
 * Created by PhpStorm.
 * User: xaaalera
 * Date: 05.07.18
 * Time: 18:13
 */
//sleep(1);
//ini_set('error_reporting', E_ALL);
//ini_set('display_errors', 1);
//ini_set('display_startup_errors', 1);
//$data = json_decode(trim(file_get_contents('php://input')), true);
$data =$_REQUEST;
if (!isset($data['caller'])){
    echo "YOU SHALL NOT PASS!!!!";
    return;
}
require_once 'AmoCRM_Wrap/autoload.php';

try {
    $amo = new \DrillCoder\AmoCRM_Wrap\AmoCRM('', '', '');
    $phone = $data['caller'];
    $roistat = $data['roistat'];
    $email = '';
    $contact = $amo->searchContact($phone,$email);
    
    if (!empty($contact)) {
        /** @var Contact $contact */
        $contact = current($contact);
        $lastid = max($contact->getLeadsId());
    } else {
        return;
    }
//        $visit = empty($data['visit_id']) ? 'нет номера визита' : $data['visit_id'];
    $lead = new \DrillCoder\AmoCRM_Wrap\Lead($lastid);
    $lead->setCustomField(1970010,$roistat);
    $lead->setCustomField(1979092,'Mango');
//    $lead->setCustomField(356421,$city);
   
    $lead->save();
    
   
    $post = array(
        'filters' => array(array("caller", "=", $phone)),
        'extend' => array('visit'),
        'sort' => array('date', 'DESC'),
        'limit' => 100,
        'offset' => 0
    );
    
    
  $pos = curl('https://cloud.roistat.com/api/v1/project/calltracking/call/list?key=75675&project=567567',json_encode($post));
  echo "<pre>";
  $last = json_decode($pos,1);
  $last = current(current($last))['id'];
  $update = array('id' => $last,
                'order_id' =>(string)$lastid);

    $pos_update = curl('https://cloud.roistat.com/api/v1/project/calltracking/call/update?key=5654&project=78678678',json_encode($update));
  var_dump($pos_update);
  die();
    mail('xaaalera@mail.ru','xaaalera call',print_r($roistatData,1));
    file_put_contents('file_roistat.txt',json_encode($roistatData));
} catch (\DrillCoder\AmoCRM_Wrap\AmoWrapException $e) {
    var_dump($e->getMessage());
    file_put_contents('json_error.txt',$e->getMessage());
}

function curl($url,$data)
{
    $curl = curl_init();
    
    curl_setopt_array($curl, array(
        CURLOPT_URL            => $url,
        CURLOPT_RETURNTRANSFER => true,
        CURLOPT_ENCODING       => "",
        CURLOPT_MAXREDIRS      => 10,
        CURLOPT_TIMEOUT        => 30,
        CURLOPT_HTTP_VERSION   => CURL_HTTP_VERSION_1_1,
        CURLOPT_CUSTOMREQUEST  => "POST",
        CURLOPT_POSTFIELDS     => $data,
        CURLOPT_HTTPHEADER     => array(
            "content-type: application/json"
        ),
    ));
    
    $response = curl_exec($curl);
    $err = curl_error($curl);
    
    curl_close($curl);
    
    if ($err) {
        "cURL Error #:" . $err;
    } else {
        return $response;
    }
}
```