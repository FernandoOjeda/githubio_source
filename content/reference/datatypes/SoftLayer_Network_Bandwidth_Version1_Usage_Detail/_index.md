---
title: "SoftLayer_Network_Bandwidth_Version1_Usage_Detail"
description: "The SoftLayer_Network_Bandwidth_Version1_Usage_Detail data type contains specific information relating to bandwidth util... "
layout: "datatype"
tags:
    - "datatype"
    - "sldn"
    - "Network"
classes:
    - "SoftLayer_Network_Bandwidth_Version1_Usage_Detail"
---

# SoftLayer_Network_Bandwidth_Version1_Usage_Detail
<div id='service-datatype'>
    <ul id='sldn-reference-tabs'>
        <li id='datatype'> <a href='/reference/datatypes/SoftLayer_Network_Bandwidth_Version1_Usage_Detail' >Datatype</a></li>
    </ul>
</div>

## Description 
The SoftLayer_Network_Bandwidth_Version1_Usage_Detail data type contains specific information relating to bandwidth utilization at a specific point in time on a given network interface. 





<!-- Service Filer BEGIN -->
<div class="view-filters">
        <div class="clearfix">
            <div class="search-input-box">
                <input placeholder="Method Filter" onkeyup="titleSearch(inputId='prop-input', divId='properties', elementClass='prop-row')" 
                    type="text" id="prop-input" value="" size="30" maxlength="128" class="form-text">
            </div>
        </div>
</div>
<!-- Service Filer END -->

<div id="properties" class="content">
    <div id="localProperties" class="prop-content" >
        <h2>Local</h2>
                <div class='prop-row views-row'>
            <span class='views-field-title'>
                <a href="#amountIn" name=amountIn>amountIn</a>
            </span>
            <div class='views-field-body'>Incoming bandwidth utilization . </div>
            <span class="type-label">Type:</span> 
            <div class='type-content'>
                <p>decimal</p>
            </div>
        </div>
                <div class='prop-row views-row'>
            <span class='views-field-title'>
                <a href="#amountOut" name=amountOut>amountOut</a>
            </span>
            <div class='views-field-body'>Outgoing bandwidth utilization . </div>
            <span class="type-label">Type:</span> 
            <div class='type-content'>
                <p>decimal</p>
            </div>
        </div>
                <div class='prop-row views-row'>
            <span class='views-field-title'>
                <a href="#day" name=day>day</a>
            </span>
            <div class='views-field-body'>Day and time this bandwidth utilization event was recorded. </div>
            <span class="type-label">Type:</span> 
            <div class='type-content'>
                <p>dateTime</p>
            </div>
        </div>
            </div>
        <div id="relationalProperties"  class="prop-content" >
        <h2>Relational</h2>
                <div class='prop-row views-row'>
            <span class='views-field-title'>
                <a href="#bandwidthUsage" name=bandwidthUsage>bandwidthUsage</a>
            </span>
            <div class='views-field-body'>In and out bandwidth utilization for a specified time stamp. </div>
            <span class="type-label">Type:</span> 
            <div class='type-content'>
                <p><a href='/reference/datatypes/SoftLayer_Network_Bandwidth_Version1_Usage'>SoftLayer_Network_Bandwidth_Version1_Usage </a></p>
            </div>
        </div>
                <div class='prop-row views-row'>
            <span class='views-field-title'>
                <a href="#bandwidthUsageDetailType" name=bandwidthUsageDetailType>bandwidthUsageDetailType</a>
            </span>
            <div class='views-field-body'>Describes this bandwidth utilization record as on the public or private network interface. </div>
            <span class="type-label">Type:</span> 
            <div class='type-content'>
                <p><a href='/reference/datatypes/SoftLayer_Network_Bandwidth_Version1_Usage_Detail_Type'>SoftLayer_Network_Bandwidth_Version1_Usage_Detail_Type </a></p>
            </div>
        </div>
                <h2>Count</h2>
            </div>
</div>


