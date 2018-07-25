---
title: СОздание сущности звонок в амо и обновление  записи с ройстат телефонии
layout: default
---

СОздание сущности звонок в амо и обновление  записи с ройстат телефонии
```
<?php
/**
 * Created by PhpStorm.
 * User: xaaalera
 * Date: 05.07.18
 * Time: 18:13
 */
ini_set('error_reporting', E_ALL);
ini_set('display_errors', 1);
ini_set('display_startup_errors', 1);
$data = json_decode(trim(file_get_contents('php://input')), true);
file_put_contents('json.txt', file_get_contents('php://input'));
//$data =$_REQUEST;
if (!isset($data['caller'])) {
    echo "YOU SHALL NOT PASS!!!!";
    return;
}
require_once 'AmoCRM_Wrap/autoload.php';

try {
    $amo = new \DrillCoder\AmoCRM_Wrap\AmoCRM('', '',
        'fda1511476a64fb9dc6d6162ab76ea2f');
    $email = isset ($data['email']) ? $data['email'] : '';
    $phone = isset ($data['caller']) ? $data['caller'] : '';
    $contact = $amo->searchContact($phone, $email);
    if (!empty($contact)) {
        /** @var Contact $contact */
        $contact = current($contact);
        $lastid = max($contact->getLeadsId());
    } else {
        return;
    }
    $lead = new \DrillCoder\AmoCRM_Wrap\Lead($lastid);


   
    $call = array();
    $call['add']= array(array(
                            'element_id'=>$contact->getId(),
                            'element_type'=>1,
                            'note_type'=>'10',
                            'params' => array('UNIQ'     => $data['id'],
                                              'LINK'     => $data['link'],
                                              'PHONE'    => $data['caller'],
                                              'DURATION' => $data['duration'],
                                              'SRC'      => 'roistat'
                        )));
    
//    $lead->addNote($params, '10');

 
    $request = DrillCoder\AmoCRM_Wrap\AmoCRM::cUrl("api/v2/notes", $call);
} catch (\DrillCoder\AmoCRM_Wrap\AmoWrapException $e) {
    file_put_contents('jsonerror.txt', $e->getMessage());
}
```