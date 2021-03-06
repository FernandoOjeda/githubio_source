---
title: "activatePublicPort"
description: "<b>Note:</b> All error handling and parameter documentation is referencing behavior available on January 4th, 2019. 

<h... "
layout: "method"
tags:
    - "method"
    - "sldn"
    - "Hardware"
classes:
    - "SoftLayer_Hardware_SecurityModule"
aliases:
    - "/reference/services/softlayer_hardware_securitymodule/activatePublicPort"
---
# [SoftLayer_Hardware_SecurityModule](/reference/services/SoftLayer_Hardware_SecurityModule)::activatePublicPort

Activate a server's public network interface.


## Overview 
<b>Note:</b> All error handling and parameter documentation is referencing behavior available on January 4th, 2019. 

<h3>Behavior deprecated on January 4th, 2019</h3> 

Activates the public network port 

<h3>NEW! Behavior on January 4th, 2019</h3> 

Activate a server's public network interface to the maximum available speed. This operation is an alias for [[SoftLayer_Hardware_Server/setPublicNetworkInterfaceSpeed]] with a $newSpeed of -1 and a $redundancy of "redundant". 

Receipt of a response does not indicate completion of the configuration change. Any subsequent attempts to request the interface change speed or state, while changes are pending, will result in a busy error. 

A response of true indicates a change was required to activate the interface; thus changes are pending. A response of false indicates the interface was already active, and thus no changes are pending. 

### Parameters 
|Name | Type | Description |
| --- | --- | --- |


### Required Headers
* authenticate
* SoftLayer_Hardware_SecurityModuleInitParameters

### Optional Headers

### Return Values
boolean


### associatedMethods

*  [SoftLayer_Hardware_Server::setPublicNetworkInterfaceSpeed](/reference/services/SoftLayer_Hardware_Server/setPublicNetworkInterfaceSpeed )
*  [SoftLayer_Hardware_Server::shutdownPublicPort](/reference/services/SoftLayer_Hardware_Server/shutdownPublicPort )
*  [SoftLayer_Hardware_Server::activatePrivatePort](/reference/services/SoftLayer_Hardware_Server/activatePrivatePort )

