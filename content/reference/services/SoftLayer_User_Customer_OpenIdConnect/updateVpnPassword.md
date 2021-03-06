---
title: "updateVpnPassword"
description: "Update a user's VPN password on the SoftLayer customer portal. As with portal passwords, VPN passwords must match the fo... "
layout: "method"
tags:
    - "method"
    - "sldn"
    - "User"
classes:
    - "SoftLayer_User_Customer_OpenIdConnect"
aliases:
    - "/reference/services/softlayer_user_customer_openidconnect/updateVpnPassword"
---
# [SoftLayer_User_Customer_OpenIdConnect](/reference/services/SoftLayer_User_Customer_OpenIdConnect)::updateVpnPassword

Update a user's VPN password


## Overview 
Update a user's VPN password on the SoftLayer customer portal. As with portal passwords, VPN passwords must match the following restrictions. VPN passwords must... 
* ...be over eight characters long.
* ...be under twenty characters long.
* ...contain at least one uppercase letter
* ...contain at least one lowercase letter
* ...contain at least one number
* ...contain one of the special characters _ - | @ . , ? / ! ~ # $ % ^ & * ( ) { } [ ] \ =
* ...not match your username
* ...not match your forum password
Finally, users can only update their own VPN password. An account's master user can update any of their account users' VPN passwords. 

### Parameters 
|Name | Type | Description |
| --- | --- | --- |
|password| string| Your new VPN password|


### Required Headers
* authenticate
* SoftLayer_User_Customer_OpenIdConnectInitParameters

### Optional Headers

### Return Values
boolean

### External Links


* [The SoftLayer Customer Portal](https://manage.softlayer.com)



### associatedMethods

*  [SoftLayer_User_Customer::updatePassword](/reference/services/SoftLayer_User_Customer/updatePassword )

