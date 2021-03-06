---
title: "getMonitoringClosedAlarms"
description: "Returns closed monitoring alarms for a given time period"
layout: "method"
tags:
    - "method"
    - "sldn"
    - "Hardware"
classes:
    - "SoftLayer_Hardware"
aliases:
    - "/reference/services/softlayer_hardware/getMonitoringClosedAlarms"
---
# [SoftLayer_Hardware](/reference/services/SoftLayer_Hardware)::getMonitoringClosedAlarms

Returns closed monitoring alarms for a given time period


## Overview 
Returns closed monitoring alarms for a given time period 

### Parameters 
|Name | Type | Description |
| --- | --- | --- |
|startDate| dateTime| |
|endDate| dateTime| |


### Required Headers
* authenticate
* SoftLayer_HardwareInitParameters

### Optional Headers

### Return Values
<a href='/reference/datatypes/SoftLayer_Container_Monitoring_Alarm_History'>SoftLayer_Container_Monitoring_Alarm_History[] </a>

