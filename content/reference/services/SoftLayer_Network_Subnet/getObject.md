---
title: "getObject"
description: "getObject retrieves the SoftLayer_Network_Subnet object whose ID number corresponds to the ID number of the init paramet... "
layout: "method"
tags:
    - "method"
    - "sldn"
    - "Network"
classes:
    - "SoftLayer_Network_Subnet"
aliases:
    - "/reference/services/softlayer_network_subnet/getObject"
---
# [SoftLayer_Network_Subnet](/reference/services/SoftLayer_Network_Subnet)::getObject

Retrieve a SoftLayer_Network_Subnet record.


## Overview 
getObject retrieves the SoftLayer_Network_Subnet object whose ID number corresponds to the ID number of the init parameter passed to the SoftLayer_Network_Subnet service. You can only retrieve the subnet whose vlan is associated with the account that you portal user is assigned to. 

### Parameters 
|Name | Type | Description |
| --- | --- | --- |


### Required Headers
* SoftLayer_Network_SubnetInitParameters
* authenticate

### Optional Headers
* SoftLayer_Network_SubnetObjectMask
* SoftLayer_Network_SubnetObjectFilter
* SoftLayer_ObjectMask

### Return Values
<a href='/reference/datatypes/SoftLayer_Network_Subnet'>SoftLayer_Network_Subnet </a>

