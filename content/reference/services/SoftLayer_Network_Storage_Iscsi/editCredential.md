---
title: "editCredential"
description: "This method will change the password of a credential created using the 'addNewCredential' method. If the credential exis... "
layout: "method"
tags:
    - "method"
    - "sldn"
    - "Network"
classes:
    - "SoftLayer_Network_Storage_Iscsi"
aliases:
    - "/reference/services/softlayer_network_storage_iscsi/editCredential"
---
# [SoftLayer_Network_Storage_Iscsi](/reference/services/SoftLayer_Network_Storage_Iscsi)::editCredential

This method will change the password of a credential created using the 'addNewCredential' method.


## Overview 
This method will change the password of a credential created using the 'addNewCredential' method. If the credential exists on multiple storage volumes it will change for those volumes as well. 

### Parameters 
|Name | Type | Description |
| --- | --- | --- |
|username| string| A username assigned to the current volume|
|newPassword| string| The new password you would like the username to have.|


### Required Headers
* authenticate
* SoftLayer_Network_Storage_IscsiInitParameters

### Optional Headers

### Return Values
boolean

