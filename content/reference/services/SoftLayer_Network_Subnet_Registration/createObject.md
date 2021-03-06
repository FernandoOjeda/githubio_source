---
title: "createObject"
description: "Create registration with a global registrar to associate an assigned subnet with the provided contact details. 

Contact... "
layout: "method"
tags:
    - "method"
    - "sldn"
    - "Network"
classes:
    - "SoftLayer_Network_Subnet_Registration"
aliases:
    - "/reference/services/softlayer_network_subnet_registration/createObject"
---
# [SoftLayer_Network_Subnet_Registration](/reference/services/SoftLayer_Network_Subnet_Registration)::createObject

Create a new subnet registration


## Overview 
Create registration with a global registrar to associate an assigned subnet with the provided contact details. 

Contact information is provided in the form of a [[SoftLayer_Account_Regional_Registry_Detail|person detail record]], which reference can be provided when the registration is created or afterwards. Registrations without an associated person detail will remain in the ``OPEN`` status. To specify a person detail when creating a registration, the ``detailReferences`` property should be populated with a list item providing a ``detailId`` value referencing the [[SoftLayer_Account_Regional_Registry_Detail|person detail record]]. 

The same applies to [[SoftLayer_Account_Regional_Registry_Detail|network detail records]], though these references need not be provided. The system will create a reference to the network described by the registration's subnet in the absence of a provided network detail reference. However, if a specific detail is referenced, it must describe the same subnet as the registration. 

A template containing the following properties will create a subnet registration: 


* networkIdentifier
* cidr
* detailReferences


``networkIdentifier`` is the base address of the public, SoftLayer owned subnet which is being registered. ``cidr`` must be an integer representing the CIDR of the subnet to be registered. The ``networkIdentifier``/``cidr`` must represent an assigned subnet. ``detailReferences`` tie the registration to SoftLayer_Account_Regional_Registry_Detail objects. 

### Parameters 
|Name | Type | Description |
| --- | --- | --- |
|templateObject| <a href='/reference/datatypes/SoftLayer_Network_Subnet_Registration'>SoftLayer_Network_Subnet_Registration </a>| The SoftLayer_Network_Subnet_Registration object that you wish to create.|


### Required Headers
* authenticate

### Optional Headers
* SoftLayer_Network_Subnet_RegistrationObjectMask
* SoftLayer_ObjectMask

### Return Values
<a href='/reference/datatypes/SoftLayer_Network_Subnet_Registration'>SoftLayer_Network_Subnet_Registration </a>


### associatedMethods

*  [SoftLayer_Network_Subnet_Registration::createObjects](/reference/services/SoftLayer_Network_Subnet_Registration/createObjects )
*  [SoftLayer_Account_Regional_Registry_Detail::createObject](/reference/services/SoftLayer_Account_Regional_Registry_Detail/createObject )

