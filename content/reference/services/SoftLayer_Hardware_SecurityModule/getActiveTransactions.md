---
title: "getActiveTransactions"
description: "Retrieve any active transaction(s) that are currently running for the server (example: os reload)."
layout: "method"
tags:
    - "method"
    - "sldn"
    - "Hardware"
classes:
    - "SoftLayer_Hardware_SecurityModule"
---
# [SoftLayer_Hardware_SecurityModule](/reference/services/SoftLayer_Hardware_SecurityModule)::getActiveTransactions

Retrieve any active transaction(s) that are currently running for the server (example: os reload).


## Overview 
Retrieve any active transaction(s) that are currently running for the server (example: os reload).

### Parameters 
|Name | Type | Description |
| --- | --- | --- |


### Required Headers
* SoftLayer_Hardware_SecurityModuleInitParameters
* authenticate

### Optional Headers
* SoftLayer_Hardware_SecurityModuleObjectMask
* SoftLayer_Hardware_SecurityModuleObjectFilter
* resultLimit
* SoftLayer_ObjectMask

### Return Values
<a href='/reference/datatypes/SoftLayer_Provisioning_Version1_Transaction'>SoftLayer_Provisioning_Version1_Transaction[] </a>
