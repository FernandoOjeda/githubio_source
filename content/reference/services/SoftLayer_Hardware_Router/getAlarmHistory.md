---
title: "getAlarmHistory"
description: "The '''getAlarmHistory''' method retrieves a detailed history for the monitoring alarm. When calling this method, a star... "
layout: "method"
tags:
    - "method"
    - "sldn"
    - "Hardware"
classes:
    - "SoftLayer_Hardware_Router"
---
# [SoftLayer_Hardware_Router](/reference/services/SoftLayer_Hardware_Router)::getAlarmHistory

Returns monitoring alarm detailed history


## Overview 
The '''getAlarmHistory''' method retrieves a detailed history for the monitoring alarm. When calling this method, a start and end date for the history to be retrieved must be entered. 

### Parameters 
|Name | Type | Description |
| --- | --- | --- |
|startDate| dateTime| Beginning date to pull the alarm history.|
|endDate| dateTime| Ending date to pull the alarm history.|
|alarmId| string| |


### Required Headers
* authenticate
* SoftLayer_Hardware_RouterInitParameters

### Optional Headers

### Return Values
<a href='/reference/datatypes/SoftLayer_Container_Monitoring_Alarm_History'>SoftLayer_Container_Monitoring_Alarm_History[] </a>
