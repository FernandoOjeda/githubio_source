---
title: "SoftLayer_Location_Group_Pricing"
description: "A pricing location group relates a set of [[SoftLayer_Product_Item_Price|prices]] to only be available to a set of [[Sof... "
date: "2018-02-12"
layout: "service"
tags:
    - "service"
    - "sldn"
    - "Location"
classes:
    - "SoftLayer_Location_Group_Pricing"
---
# SoftLayer_Location_Group_Pricing
<div id='service-datatype'>
    <ul id='sldn-reference-tabs'>
    <li id='service'> <a href='/reference/services/SoftLayer_Location_Group_Pricing' >Service</a></li>    <li id='datatype'> <a href='/reference/datatypes/SoftLayer_Location_Group_Pricing' >Datatype</a></li>
    </ul>
</div>

## Description
A pricing location group relates a set of [[SoftLayer_Product_Item_Price|prices]] to only be available to a set of [[SoftLayer_Location|locations]] when used for [[SoftLayer_Product_Order|ordering]]. 



        
<div id="properties" class="content">
    <h2>Methods</h2>
    <div class="view-filters">
        <div class="clearfix">
            <div class="search-input-box">
                <input placeholder="Datatype Filter" onkeyup="titleSearch(inputId='edit-combine', divId='method-div', elementClass='method-row')" 
                    type="text" id="edit-combine" value="" size="30" maxlength="128" class="form-text">
            </div>
        </div>
    </div>
    <div id="method-div">
            <div class="method-row">
                        <span class='view-field-title'><a href='/reference/services/SoftLayer_Location_Group_Pricing/getAllObjects'> getAllObjects</a> </span>
            <div class='views-field-body'>Get all pricing location groups.</div>
        </div>
            <div class="method-row">
                        <span class='view-field-title'><a href='/reference/services/SoftLayer_Location_Group_Pricing/getLocationGroupType'> getLocationGroupType</a> </span>
            <div class='views-field-body'>Retrieve the type for this location group.</div>
        </div>
            <div class="method-row">
                        <span class='view-field-title'><a href='/reference/services/SoftLayer_Location_Group_Pricing/getLocations'> getLocations</a> </span>
            <div class='views-field-body'>Retrieve the locations that this pricing location group is applicable for. This limits the locations that the prices referenced by this pricing location group can be used with.</div>
        </div>
            <div class="method-row">
                        <span class='view-field-title'><a href='/reference/services/SoftLayer_Location_Group_Pricing/getObject'> getObject</a> </span>
            <div class='views-field-body'>Retrieve a SoftLayer_Location_Group_Pricing record.</div>
        </div>
            <div class="method-row">
                        <span class='view-field-title'><a href='/reference/services/SoftLayer_Location_Group_Pricing/getPrices'> getPrices</a> </span>
            <div class='views-field-body'>Retrieve the prices that this pricing location group limits. All of these prices will only be available in the locations defined by this pricing location group.</div>
        </div>
        </div>
</div>

