---
title: "addAttachedDedicatedHost"
description: "Attach the given Dedicated Host to a SoftLayer ticket. An attachment provides an easy way for SoftLayer's employees to q... "
layout: "method"
tags:
    - "method"
    - "sldn"
    - "Ticket"
classes:
    - "SoftLayer_Ticket"
aliases:
    - "/reference/services/softlayer_ticket/addAttachedDedicatedHost"
---
# [SoftLayer_Ticket](/reference/services/SoftLayer_Ticket)::addAttachedDedicatedHost

Attach a Dedicated Host to a ticket.


## Overview 
Attach the given Dedicated Host to a SoftLayer ticket. An attachment provides an easy way for SoftLayer's employees to quickly look up your records in the case of specific issues. 

### Parameters 
|Name | Type | Description |
| --- | --- | --- |
|dedicatedHostId| integer| The internal identifier of the dedicated host record to attach.|


### Required Headers
* authenticate
* SoftLayer_TicketInitParameters

### Optional Headers
* SoftLayer_TicketObjectMask
* SoftLayer_ObjectMask

### Return Values
<a href='/reference/datatypes/SoftLayer_Ticket_Attachment_Dedicated_Host'>SoftLayer_Ticket_Attachment_Dedicated_Host </a>


### associatedMethods

*  [SoftLayer_Ticket::removeAttachedDedicatedHost](/reference/services/SoftLayer_Ticket/removeAttachedDedicatedHost )

