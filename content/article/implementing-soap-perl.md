---
title: "SOAP in Perl"
description: "Interacting with the SoftLayer API SOAP endpoint with Perl"
date: "2011-07-11"
tags:
    - "article"
    - "sldn"
    - "perl"
    - "soap"
---


<p><em>Since writing this guide we've created a <a href="/article/Perl">Perl</a> specific implementation and manual page. Check them out!</em><br />
Our Perl SOAP <a href="/article/The SoftLayer API">API</a> examples perform simple retrieval operations and output results to your Perl console. We use the <a href="http://sourceforge.net/project/showfiles.php?group_id=66000">SOAP::Lite</a> Perl module to handle generating our SOAP headers, generating our SOAP request, then interpreting the result from the method call. Each of these examples execute a single method call to retrieve its data. We'll start out with a simple direct call, implement an <a href="/article/object mask">object mask</a>, then combine the two. </p>
<p>In each of these examples you must set the <em>$PORTAL_USERNAME</em> and <em>$API_KEY</em> variables to <a href="/article/authenticate">authenticate</a> to the API.</p>
<h2>Example 1: Generating a Server List</h2>
<p>First we'll retrieve a list of servers on an account and display their id numbers, hostnames, public IP addresses, and private IP addresses in a simple console table. Hostname, domain name, public IP address, and private IP address are all local properties to the <a href="/reference/datatypes/SoftLayer_Hardware/">SoftLayer_Hardware</a> data type, so a single call to <a href="/reference/services/SoftLayer_Account/getHardware">getHardware</a> in the <a href="/reference/services/SoftLayer_Account/">SoftLayer_Account</a> service is all we need. This example will:</p>
<p>#Connect to the SoftLayer_Account SOAP endpoint via SOAP::Lite<br />
#Build the authenticate SOAP header<br />
#Call getHardware from the SoftLayer_Account service<br />
#Loop through the results of the method call and display table output</p>
<p>This is a very simple call and loop script. The next examples will get more in depth. The output from the code below looks akin to the following:</p>
<div class="geshifilter">
<pre class="text geshifilter-text" style="font-family:monospace;"> +------------+-------------------------------------------------------------+------------------+------------------+
 | HardwareId | Host Name                                                   | Public IP        | Private IP       |
 +------------+-------------------------------------------------------------+------------------+------------------+
 |       1001 | myhostname.example.org                                      |          x.y.z.w |        10.4.1.2  |
 |       1002 | another-server.example.org                                  |          a.b.c.d |        10.4.1.3  |
 |       1003 | and-another-server.example.org                              |          e.f.g.h |        10.4.1.4  |
 +------------+-------------------------------------------------------------+------------------+------------------+</pre></div>
<div class="geshifilter">
<pre class="perl geshifilter-perl" style="font-family:monospace;"><span style="color: #666666; font-style: italic;"># SoftLayer API Perl-SOAP Example #1 - A server list</span>
<span style="color: #666666; font-style: italic;">#</span>
<span style="color: #666666; font-style: italic;"># Retrieve and display a list of an account's servers id numbers, hostnames,</span>
<span style="color: #666666; font-style: italic;"># public IP addresses, and private IP addresses. Do all of this through a single</span>
<span style="color: #666666; font-style: italic;"># SOAP call to the SoftLayer API, in this case to the getHardware() method in</span>
<span style="color: #666666; font-style: italic;"># the SoftLayer_Account service</span>
<span style="color: #666666; font-style: italic;"># <http://sldn.softlayer.com/wiki/index.php/SoftLayer_Account::getHardware>.</span>
<span style="color: #666666; font-style: italic;">#</span>
<span style="color: #666666; font-style: italic;"># Sample Output:</span>
<span style="color: #666666; font-style: italic;"># +------------+-------------------------------------------------------------+------------------+------------------+</span>
<span style="color: #666666; font-style: italic;"># | HardwareId | Host Name                                                   | Public IP        | Private IP       |</span>
<span style="color: #666666; font-style: italic;"># +------------+-------------------------------------------------------------+------------------+------------------+</span>
<span style="color: #666666; font-style: italic;"># |       1001 | myhostname.example.org                                      |          x.y.z.w |        10.4.1.2  |</span>
<span style="color: #666666; font-style: italic;"># |       1002 | another-server.example.org                                  |          a.b.c.d |        10.4.1.3  |</span>
<span style="color: #666666; font-style: italic;"># |       1003 | and-another-server.example.org                              |          e.f.g.h |        10.4.1.4  |</span>
<span style="color: #666666; font-style: italic;"># +------------+-------------------------------------------------------------+------------------+------------------+</span>
<span style="color: #666666; font-style: italic;">#</span>
<span style="color: #666666; font-style: italic;"># The most up to date version of this code can be found at</span>
<span style="color: #666666; font-style: italic;"># <http://sldn.softlayer.com/wiki/index.php/Implementing_SOAP_in_Perl></span>
<span style="color: #666666; font-style: italic;">#</span>
<span style="color: #666666; font-style: italic;"># This code is licensed under the Creative Commons 3.0 Attribute Unported</span>
<span style="color: #666666; font-style: italic;"># License <http://creativecommons.org/licenses/by/3.0/>.</span>
&nbsp;
<span style="color: #000000; font-weight: bold;">use</span> SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Lite</span><span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># define variables</span>
<span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$SOAP_ENDPOINT</span> <span style="color: #339933;">=</span> <span style="color: #ff0000;">'http://api.service.softlayer.com/soap/v3/SoftLayer_Account'</span><span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># Your portal username</span>
<span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$PORTAL_USERNAME</span> <span style="color: #339933;">=</span> <span style="color: #ff0000;">"set me"</span><span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># Your API Access key as generated from the portal</span>
<span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$API_KEY</span> <span style="color: #339933;">=</span> <span style="color: #ff0000;">'set me'</span><span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># Create a SOAP object to make queries to the endpoint</span>
<span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$soap</span> <span style="color: #339933;">=</span> SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Lite</span>
    <span style="color: #339933;">-></span> <span style="color: #006600;">uri</span><span style="color: #009900;">&#40;</span><span style="color: #0000ff;">$SOAP_ENDPOINT</span><span style="color: #009900;">&#41;</span>
    <span style="color: #339933;">-></span> <span style="color: #006600;">proxy</span><span style="color: #009900;">&#40;</span><span style="color: #0000ff;">$SOAP_ENDPOINT</span><span style="color: #009900;">&#41;</span>
    <span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># Create the header object to be sent along with the request, this will</span>
<span style="color: #666666; font-style: italic;"># authenticate your request</span>
<span style="color: #666666; font-style: italic;"># See <http://sldn.softlayer.com/wiki/index.php/Authenticating_to_the_SoftLayer_API></span>
<span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$header</span> <span style="color: #339933;">=</span> SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">name</span><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"authenticate"</span> <span style="color: #339933;">=></span>
  \SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">value</span><span style="color: #009900;">&#40;</span>
   SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">name</span><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"username"</span> <span style="color: #339933;">=></span> <span style="color: #0000ff;">$PORTAL_USERNAME</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">,</span>
   SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">name</span><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"apiKey"</span> <span style="color: #339933;">=></span> <span style="color: #0000ff;">$API_KEY</span><span style="color: #009900;">&#41;</span>
   <span style="color: #009900;">&#41;</span>
<span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># Define the method you wish to call</span>
<span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$method</span> <span style="color: #339933;">=</span> SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Data</span><span style="color: #339933;">-></span><span style="color: #006600;">name</span><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">'getHardware'</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># Call the method and assign the response to the $response object</span>
<span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$response</span> <span style="color: #339933;">=</span> <span style="color: #0000ff;">$soap</span><span style="color: #339933;">-></span><span style="color: #006600;">call</span><span style="color: #009900;">&#40;</span><span style="color: #0000ff;">$method</span><span style="color: #339933;">,</span> <span style="color: #0000ff;">$header</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># Error-checking...</span>
<span style="color: #b1b100;">if</span> <span style="color: #009900;">&#40;</span><span style="color: #0000ff;">$response</span><span style="color: #339933;">-></span><span style="color: #006600;">fault</span><span style="color: #009900;">&#41;</span>
<span style="color: #009900;">&#123;</span>
    <a href="http://perldoc.perl.org/functions/print.html"><span style="color: #000066;">print</span></a> <span style="color: #0000ff;">$response</span><span style="color: #339933;">-></span><span style="color: #006600;">faultcode</span><span style="color: #339933;">,</span> <span style="color: #ff0000;">" "</span><span style="color: #339933;">,</span> <span style="color: #0000ff;">$response</span><span style="color: #339933;">-></span><span style="color: #006600;">faultstring</span><span style="color: #339933;">,</span> <span style="color: #ff0000;">"<span style="color: #000099; font-weight: bold;">\n</span>"</span><span style="color: #339933;">;</span>
    <a href="http://perldoc.perl.org/functions/die.html"><span style="color: #000066;">die</span></a> <span style="color: #ff0000;">'Terminating program'</span><span style="color: #339933;">;</span>
<span style="color: #009900;">&#125;</span>
<span style="color: #b1b100;">else</span>
<span style="color: #009900;">&#123;</span>
    <span style="color: #666666; font-style: italic;"># For this particular example, we have an array of objects returned, so</span>
    <span style="color: #666666; font-style: italic;"># break them into @hardwareObjects</span>
    <span style="color: #b1b100;">my</span> <span style="color: #0000ff;">@hardwareObjects</span> <span style="color: #339933;">=</span> <span style="color: #339933;">@</span><span style="color: #009900;">&#123;</span><span style="color: #0000ff;">$response</span><span style="color: #339933;">-></span><span style="color: #006600;">result</span><span style="color: #009900;">&#125;</span><span style="color: #339933;">;</span>
&nbsp;
&nbsp;
    <span style="color: #666666; font-style: italic;"># Print table headers</span>
    <a href="http://perldoc.perl.org/functions/print.html"><span style="color: #000066;">print</span></a> <span style="color: #ff0000;">"+------------+-------------------------------------------------------------+------------------+------------------+<span style="color: #000099; font-weight: bold;">\n</span>"</span><span style="color: #339933;">;</span>
    <a href="http://perldoc.perl.org/functions/print.html"><span style="color: #000066;">print</span></a> <span style="color: #ff0000;">"| HardwareId | Host Name                                                   | Public IP        | Private IP       |<span style="color: #000099; font-weight: bold;">\n</span>"</span><span style="color: #339933;">;</span>
    <a href="http://perldoc.perl.org/functions/print.html"><span style="color: #000066;">print</span></a> <span style="color: #ff0000;">"+------------+-------------------------------------------------------------+------------------+------------------+<span style="color: #000099; font-weight: bold;">\n</span>"</span><span style="color: #339933;">;</span>
&nbsp;
    <span style="color: #666666; font-style: italic;"># Loop through the hardware objects and print their data</span>
    <span style="color: #b1b100;">foreach</span> <span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$hardware</span> <span style="color: #009900;">&#40;</span><span style="color: #0000ff;">@hardwareObjects</span><span style="color: #009900;">&#41;</span> <span style="color: #009900;">&#123;</span>
        <a href="http://perldoc.perl.org/functions/printf.html"><span style="color: #000066;">printf</span></a><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"| %10s "</span><span style="color: #339933;">,</span> <span style="color: #0000ff;">$hardware</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>id<span style="color: #009900;">&#125;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
        <a href="http://perldoc.perl.org/functions/printf.html"><span style="color: #000066;">printf</span></a><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"| %-59s "</span><span style="color: #339933;">,</span> <span style="color: #0000ff;">$hardware</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>hostname<span style="color: #009900;">&#125;</span> . <span style="color: #ff0000;">"."</span> . <span style="color: #0000ff;">$hardware</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>domain<span style="color: #009900;">&#125;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
        <a href="http://perldoc.perl.org/functions/printf.html"><span style="color: #000066;">printf</span></a><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"| %16s "</span><span style="color: #339933;">,</span> <span style="color: #0000ff;">$hardware</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>primaryIpAddress<span style="color: #009900;">&#125;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
        <a href="http://perldoc.perl.org/functions/printf.html"><span style="color: #000066;">printf</span></a><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"| %16s |<span style="color: #000099; font-weight: bold;">\n</span>"</span><span style="color: #339933;">,</span> <span style="color: #0000ff;">$hardware</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>primaryBackendIpAddress<span style="color: #009900;">&#125;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
    <span style="color: #009900;">&#125;</span>
    <a href="http://perldoc.perl.org/functions/print.html"><span style="color: #000066;">print</span></a> <span style="color: #ff0000;">"+------------+-------------------------------------------------------------+------------------+------------------+<span style="color: #000099; font-weight: bold;">\n</span>"</span><span style="color: #339933;">;</span>
&nbsp;
<span style="color: #009900;">&#125;</span></pre></div>
<h2>Example 2: Getting a server, its operating system, and its datacenter</h2>
<p>Its easy to pull up an object and display its local properties. This next example deals with how to pull up an object and its relational properties with a single call using object masks. In this example we're going to retrieve a single server via its <a href="/article/initialization parameter">initialization parameter</a> and also pick up its operating system and datacenter. Without an object mask this would require three method calls. One to <a href="/reference/services/SoftLayer_Hardware/getObject">getObject</a> to retrieve the hardware, one to <a href="/reference/services/SoftLayer_Hardware/getOperatingSystem">getOperatingSystem</a> to get its operating system software object, and one to <a href="/reference/services/SoftLayer_Hardware/getDatacenter">getDatacenter</a> to get the hardware's datacenter location object. </p>
<p>Since operating system and datacenter are directly related to SoftLayer_Hardware objects we'll instead use an <a href="/article/object mask">object mask</a> to get these two properties when we get our hardware object. Our object mask will contain two items in the following structure:<br />
*operatingSystem<br />
*datacenter</p>
<p>The example code below builds the object mask as another SOAP header and sends it along with the authenticate header to our method call. It goes through the following process:</p>
<p>#Connect to the SoftLayer_Hardware SOAP endpoint via SOAP::Lite<br />
#Build the authenticate SOAP header<br />
#Build the SoftLayer_HardwareObjectMask SOAP header with the operatingSystem and datacenter properties<br />
#Call getObject from the SoftLayer_Hardware service<br />
#Display the results</p>
<p>Object masks make this process take much less time, but example three really shows off its power. Since example two deals with a single server you will need to know its ID number before proceeding. You can find a hardware object's ID number from the getHardware() method in the SoftLayer_Account service. Set the <em>$SERVER_ID</em> variable in this script to the hardware ID you want to fetch. It's output should look like this:</p>
<p> HardwareId:               1001<br />
 Host Name:                myserver.example.org<br />
 Operating System:         Redhat<br />
 Operating System Version: 4.6-32<br />
 Data Center:              Seattle</p>
<div class="geshifilter">
<pre class="perl geshifilter-perl" style="font-family:monospace;"><span style="color: #666666; font-style: italic;"># SoftLayer API Perl-SOAP Example #2 - Getting a server and its software</span>
<span style="color: #666666; font-style: italic;">#</span>
<span style="color: #666666; font-style: italic;"># Retrieve a single server and display its id number, hostname, operating </span>
<span style="color: #666666; font-style: italic;"># system, and datacenter. We're going to do this with one call to the </span>
<span style="color: #666666; font-style: italic;"># getObject() method in the SoftLayer_Hardware service </span>
<span style="color: #666666; font-style: italic;"># <http://sldn.softlayer.com/wiki/index.php/SoftLayer_Hardware::getObject>. </span>
<span style="color: #666666; font-style: italic;"># Since the operating system and datacenter are relational properties to </span>
<span style="color: #666666; font-style: italic;"># SoftLayer_Hardware we need to use an object mask to retrieve them when we get </span>
<span style="color: #666666; font-style: italic;"># our server record.</span>
<span style="color: #666666; font-style: italic;">#</span>
<span style="color: #666666; font-style: italic;"># Sample Output:</span>
<span style="color: #666666; font-style: italic;"># HardwareId:               1001</span>
<span style="color: #666666; font-style: italic;"># Host Name:                myserver.example.org</span>
<span style="color: #666666; font-style: italic;"># Operating System:         Redhat</span>
<span style="color: #666666; font-style: italic;"># Operating System Version: 4.6-32</span>
<span style="color: #666666; font-style: italic;"># Data Center:              Seattle</span>
<span style="color: #666666; font-style: italic;">#</span>
<span style="color: #666666; font-style: italic;"># The most up to date version of this code can be found at</span>
<span style="color: #666666; font-style: italic;"># <http://sldn.softlayer.com/wiki/index.php/Implementing_SOAP_in_Perl></span>
<span style="color: #666666; font-style: italic;">#</span>
<span style="color: #666666; font-style: italic;"># This code is licensed under the Creative Commons 3.0 Attribute Unported</span>
<span style="color: #666666; font-style: italic;"># License <http://creativecommons.org/licenses/by/3.0/>.</span>
&nbsp;
<span style="color: #000000; font-weight: bold;">use</span> SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Lite</span><span style="color: #339933;">;</span>
<span style="color: #000000; font-weight: bold;">use</span> Data<span style="color: #339933;">::</span><span style="color: #006600;">Dumper</span><span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># define variables</span>
<span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$SOAP_ENDPOINT</span> <span style="color: #339933;">=</span> <span style="color: #ff0000;">'http://api.service.softlayer.com/soap/v3/SoftLayer_Hardware'</span><span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># Your portal username</span>
<span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$PORTAL_USERNAME</span> <span style="color: #339933;">=</span> <span style="color: #ff0000;">"set me"</span><span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># Your API Access key as generated from the portal</span>
<span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$API_KEY</span> <span style="color: #339933;">=</span> <span style="color: #ff0000;">'set me'</span><span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># The ID of the server we wish to retrieve. You can get your server IDs by</span>
<span style="color: #666666; font-style: italic;"># calling the getHardware() method in the SoftLayer_Account service.</span>
<span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$SERVER_ID</span> <span style="color: #339933;">=</span> <span style="color: #ff0000;">'set me'</span><span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># Create a SOAP object to make queries to the endpoint</span>
<span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$soap</span> <span style="color: #339933;">=</span> SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Lite</span>
    <span style="color: #339933;">-></span> <span style="color: #006600;">uri</span><span style="color: #009900;">&#40;</span><span style="color: #0000ff;">$SOAP_ENDPOINT</span><span style="color: #009900;">&#41;</span>
    <span style="color: #339933;">-></span> <span style="color: #006600;">proxy</span><span style="color: #009900;">&#40;</span><span style="color: #0000ff;">$SOAP_ENDPOINT</span><span style="color: #009900;">&#41;</span>
    <span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># Create the header object to be sent along with the request, this will</span>
<span style="color: #666666; font-style: italic;"># authenticate your request</span>
<span style="color: #666666; font-style: italic;"># See <http://sldn.softlayer.com/wiki/index.php/Authenticating_to_the_SoftLayer_API></span>
<span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$header</span> <span style="color: #339933;">=</span> SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">name</span><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"authenticate"</span> <span style="color: #339933;">=></span>
  \SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">value</span><span style="color: #009900;">&#40;</span>
   SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">name</span><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"username"</span> <span style="color: #339933;">=></span> <span style="color: #0000ff;">$PORTAL_USERNAME</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">,</span>
   SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">name</span><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"apiKey"</span> <span style="color: #339933;">=></span> <span style="color: #0000ff;">$API_KEY</span><span style="color: #009900;">&#41;</span>
   <span style="color: #009900;">&#41;</span>
<span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># Create the init parameters to limit ourselves to a single object.</span>
<span style="color: #666666; font-style: italic;"># See <http://sldn.softlayer.com/wiki/index.php/Using_Initialization_Parameters_in_the_SoftLayer_API>.</span>
<span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$initParams</span> <span style="color: #339933;">=</span> SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">name</span><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"ns1:SoftLayer_HardwareInitParameters"</span> <span style="color: #339933;">=></span>
    \SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">value</span><span style="color: #009900;">&#40;</span>
        SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">name</span><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"id"</span> <span style="color: #339933;">=></span> <span style="color: #0000ff;">$SERVER_ID</span><span style="color: #009900;">&#41;</span>
    <span style="color: #009900;">&#41;</span>
<span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># Create the object mask to mask what you're interested in. In this case we want</span>
<span style="color: #666666; font-style: italic;"># the datacenter and operating system associated with our server.</span>
<span style="color: #666666; font-style: italic;"># see <http://sldn.softlayer.com/wiki/index.php/Using_Object_Masks_in_the_SoftLayer_API></span>
<span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$objectMask</span> <span style="color: #339933;">=</span> SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">name</span><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"ns1:SoftLayer_HardwareObjectMask"</span> <span style="color: #339933;">=></span>
    \SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">value</span><span style="color: #009900;">&#40;</span>
        SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">name</span><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"mask"</span> <span style="color: #339933;">=></span>
            \SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">value</span><span style="color: #009900;">&#40;</span>
                SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">name</span><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"datacenter"</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">,</span>
                SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">name</span><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"operatingSystem"</span><span style="color: #009900;">&#41;</span>
            <span style="color: #009900;">&#41;</span>
        <span style="color: #009900;">&#41;</span><span style="color: #339933;">,</span>
    <span style="color: #009900;">&#41;</span>
<span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># Define the method you wish to call</span>
<span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$method</span> <span style="color: #339933;">=</span> SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Data</span><span style="color: #339933;">-></span><span style="color: #006600;">name</span><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">'getObject'</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># Call the method and assign the response to the $response object</span>
<span style="color: #666666; font-style: italic;"># Note here that we've combined all our header objects into an array. You can do</span>
<span style="color: #666666; font-style: italic;"># this as you go if you like.</span>
<span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$response</span> <span style="color: #339933;">=</span> <span style="color: #0000ff;">$soap</span><span style="color: #339933;">-></span><span style="color: #006600;">call</span><span style="color: #009900;">&#40;</span><span style="color: #0000ff;">$method</span><span style="color: #339933;">,</span> <span style="color: #009900;">&#40;</span><span style="color: #0000ff;">$header</span><span style="color: #339933;">,</span> <span style="color: #0000ff;">$initParams</span><span style="color: #339933;">,</span> <span style="color: #0000ff;">$objectMask</span><span style="color: #009900;">&#41;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># Error-checking...</span>
<span style="color: #b1b100;">if</span> <span style="color: #009900;">&#40;</span><span style="color: #0000ff;">$response</span><span style="color: #339933;">-></span><span style="color: #006600;">fault</span><span style="color: #009900;">&#41;</span>
<span style="color: #009900;">&#123;</span>
    <a href="http://perldoc.perl.org/functions/print.html"><span style="color: #000066;">print</span></a> <span style="color: #0000ff;">$response</span><span style="color: #339933;">-></span><span style="color: #006600;">faultcode</span><span style="color: #339933;">,</span> <span style="color: #ff0000;">" "</span><span style="color: #339933;">,</span> <span style="color: #0000ff;">$response</span><span style="color: #339933;">-></span><span style="color: #006600;">faultstring</span><span style="color: #339933;">,</span> <span style="color: #ff0000;">"<span style="color: #000099; font-weight: bold;">\n</span>"</span><span style="color: #339933;">;</span>
    <a href="http://perldoc.perl.org/functions/die.html"><span style="color: #000066;">die</span></a> <span style="color: #ff0000;">'Terminating program'</span><span style="color: #339933;">;</span>
<span style="color: #009900;">&#125;</span>
<span style="color: #b1b100;">else</span>
<span style="color: #009900;">&#123;</span>
    <span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$hardwareObject</span> <span style="color: #339933;">=</span> <span style="color: #0000ff;">$response</span><span style="color: #339933;">-></span><span style="color: #006600;">result</span><span style="color: #339933;">;</span>
&nbsp;
    <span style="color: #666666; font-style: italic;"># Echo the results of this action:</span>
    <a href="http://perldoc.perl.org/functions/printf.html"><span style="color: #000066;">printf</span></a><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"%-25s %s<span style="color: #000099; font-weight: bold;">\n</span>"</span><span style="color: #339933;">,</span> <span style="color: #ff0000;">"HardwareId:"</span><span style="color: #339933;">,</span> <span style="color: #0000ff;">$hardwareObject</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>id<span style="color: #009900;">&#125;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
    <a href="http://perldoc.perl.org/functions/printf.html"><span style="color: #000066;">printf</span></a><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"%-25s %s<span style="color: #000099; font-weight: bold;">\n</span>"</span><span style="color: #339933;">,</span> <span style="color: #ff0000;">"Host Name:"</span><span style="color: #339933;">,</span> <span style="color: #0000ff;">$hardwareObject</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>hostname<span style="color: #009900;">&#125;</span> . <span style="color: #ff0000;">"."</span> . <span style="color: #0000ff;">$hardwareObject</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>domain<span style="color: #009900;">&#125;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
    <a href="http://perldoc.perl.org/functions/printf.html"><span style="color: #000066;">printf</span></a><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"%-25s %s<span style="color: #000099; font-weight: bold;">\n</span>"</span><span style="color: #339933;">,</span> <span style="color: #ff0000;">"Operating System:"</span><span style="color: #339933;">,</span> <span style="color: #0000ff;">$hardwareObject</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>operatingSystem<span style="color: #009900;">&#125;</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>softwareLicense<span style="color: #009900;">&#125;</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>softwareDescription<span style="color: #009900;">&#125;</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>manufacturer<span style="color: #009900;">&#125;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
    <a href="http://perldoc.perl.org/functions/printf.html"><span style="color: #000066;">printf</span></a><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"%-25s %s<span style="color: #000099; font-weight: bold;">\n</span>"</span><span style="color: #339933;">,</span> <span style="color: #ff0000;">"Operating System Version:"</span><span style="color: #339933;">,</span> <span style="color: #0000ff;">$hardwareObject</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>operatingSystem<span style="color: #009900;">&#125;</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>softwareLicense<span style="color: #009900;">&#125;</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>softwareDescription<span style="color: #009900;">&#125;</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>version<span style="color: #009900;">&#125;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
    <a href="http://perldoc.perl.org/functions/printf.html"><span style="color: #000066;">printf</span></a><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"%-25s %s<span style="color: #000099; font-weight: bold;">\n</span>"</span><span style="color: #339933;">,</span> <span style="color: #ff0000;">"Data Center:"</span><span style="color: #339933;">,</span> <span style="color: #0000ff;">$hardwareObject</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>datacenter<span style="color: #009900;">&#125;</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>longName<span style="color: #009900;">&#125;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
<span style="color: #009900;">&#125;</span></pre></div>
<h2>Example 3: Combining it up. Getting a server list along with each server's operating system and datacenter</h2>
<p>Now that we know how to pull a list of hardware from a single method call and know how to pull relational properties through an object mask, let's combine the two techniques and get a list of our account's hardware, their operating systems, datacenters, and display the results in a console table. Since we need a list of hardware we're going to go to the getObject() method in the SoftLayer_Account service. This would normally retrieve your general account data. However by defining an object mask that grabs the <a href="/reference/datatypes/SoftLayer_Account/">SoftLayer_Account</a> data type's relational properties for hardware, then get the operating system and datacenter that relate to that hardware. Our object mask now contains three items with the following structure.<br />
*hardware<br />
**operatingSystem<br />
**datacenter</p>
<p>Without an object mask we would need to call getHardware as in the first example, but then loop through every hardware object and call getOperatingSystem and getDatacenter for every single hardware object under your account. This object mask saves you a lot of time as a developer, makes your code cleaner, and lighter to load on our backend systems. Our final example goes through the following process:</p>
<p>#Connect to the SoftLayer_Account SOAP endpoint via SOAP::Lite<br />
#Build the authenticate SOAP header<br />
#Build the SoftLayer_AccountObjectMask SOAP header with the hardware and hardware's operatingSystem and datacenter properties<br />
#Call getObject from the SoftLayer_Account service<br />
#Loop through the results of the method call and display table output</p>
<p>And it's output will look akin to this:</p>
<div class="geshifilter">
<pre class="text geshifilter-text" style="font-family:monospace;"> +------------+-------------------------------------------------------------+---------------------------+--------------+
 | HardwareId | Host Name                                                   | OS Version                | Data Center  |
 +------------+-------------------------------------------------------------+---------------------------+--------------+
 |       1001 | myhostname.example.org                                      | Redhat 5.0-32             |       Dallas |
 |       1002 | another-server.example.org                                  | Microsoft STD-SRV-SP2-32  |      Seattle |
 |       1003 | and-another-server.example.org                              | Microsoft ENT-SRV-SP2-32  |      Seattle |
 +------------+-------------------------------------------------------------+---------------------------+--------------+</pre></div>
<div class="geshifilter">
<pre class="perl geshifilter-perl" style="font-family:monospace;"><span style="color: #666666; font-style: italic;"># SoftLayer API Perl-SOAP Example #3 - A server list with operating system and datacenter</span>
<span style="color: #666666; font-style: italic;">#</span>
<span style="color: #666666; font-style: italic;"># Retrieve and display a list of an account's servers id numbers, hostnames,</span>
<span style="color: #666666; font-style: italic;"># operatign systems, and datacenters. Do all of this through a single</span>
<span style="color: #666666; font-style: italic;"># SOAP call to the SoftLayer API, in this case to the getObject() method in</span>
<span style="color: #666666; font-style: italic;"># the SoftLayer_Account service</span>
<span style="color: #666666; font-style: italic;"># <http://sldn.softlayer.com/wiki/index.php/SoftLayer_Account::getObject>. Since</span>
<span style="color: #666666; font-style: italic;"># hardware is a relational property to SoftLayer_Account and operating system</span>
<span style="color: #666666; font-style: italic;"># and datacenter are relational properties to SoftLayer_Hardware we have to</span>
<span style="color: #666666; font-style: italic;"># define an object mask to pull the account's associated hardware and each</span>
<span style="color: #666666; font-style: italic;"># hardware's associated operating system and datacenter.</span>
<span style="color: #666666; font-style: italic;">#</span>
<span style="color: #666666; font-style: italic;"># Sample Output:</span>
<span style="color: #666666; font-style: italic;"># +------------+-------------------------------------------------------------+---------------------------+--------------+</span>
<span style="color: #666666; font-style: italic;"># | HardwareId | Host Name                                                   | OS Version                | Data Center  |</span>
<span style="color: #666666; font-style: italic;"># +------------+-------------------------------------------------------------+---------------------------+--------------+</span>
<span style="color: #666666; font-style: italic;"># |       1001 | myhostname.example.org                                      | Redhat 5.0-32             |       Dallas |</span>
<span style="color: #666666; font-style: italic;"># |       1002 | another-server.example.org                                  | Microsoft STD-SRV-SP2-32  |      Seattle |</span>
<span style="color: #666666; font-style: italic;"># |       1003 | and-another-server.example.org                              | Microsoft ENT-SRV-SP2-32  |      Seattle |</span>
<span style="color: #666666; font-style: italic;"># +------------+-------------------------------------------------------------+---------------------------+--------------+</span>
<span style="color: #666666; font-style: italic;">#</span>
<span style="color: #666666; font-style: italic;"># The most up to date version of this code can be found at</span>
<span style="color: #666666; font-style: italic;"># <http://sldn.softlayer.com/wiki/index.php/Implementing_SOAP_in_Perl></span>
<span style="color: #666666; font-style: italic;">#</span>
<span style="color: #666666; font-style: italic;"># This code is licensed under the Creative Commons 3.0 Attribute Unported</span>
<span style="color: #666666; font-style: italic;"># License <http://creativecommons.org/licenses/by/3.0/>.</span>
&nbsp;
<span style="color: #000000; font-weight: bold;">use</span> SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Lite</span><span style="color: #339933;">;</span>
<span style="color: #000000; font-weight: bold;">use</span> Data<span style="color: #339933;">::</span><span style="color: #006600;">Dumper</span><span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># define variables</span>
<span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$SOAP_ENDPOINT</span> <span style="color: #339933;">=</span> <span style="color: #ff0000;">'http://api.service.softlayer.com/soap/v3/SoftLayer_Account'</span><span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># Your portal username</span>
<span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$PORTAL_USERNAME</span> <span style="color: #339933;">=</span> <span style="color: #ff0000;">"set me"</span><span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># Your API Access key as generated from the portal</span>
<span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$API_KEY</span> <span style="color: #339933;">=</span> <span style="color: #ff0000;">'set me'</span><span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># Create a SOAP object to make queries to the endpoint</span>
<span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$soap</span> <span style="color: #339933;">=</span> SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Lite</span>
    <span style="color: #339933;">-></span> <span style="color: #006600;">uri</span><span style="color: #009900;">&#40;</span><span style="color: #0000ff;">$SOAP_ENDPOINT</span><span style="color: #009900;">&#41;</span>
    <span style="color: #339933;">-></span> <span style="color: #006600;">proxy</span><span style="color: #009900;">&#40;</span><span style="color: #0000ff;">$SOAP_ENDPOINT</span><span style="color: #009900;">&#41;</span>
    <span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># Create the header object to be sent along with the request, this will</span>
<span style="color: #666666; font-style: italic;"># authenticate your request</span>
<span style="color: #666666; font-style: italic;"># See <http://sldn.softlayer.com/wiki/index.php/Authenticating_to_the_SoftLayer_API></span>
<span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$header</span> <span style="color: #339933;">=</span> SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">name</span><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"authenticate"</span> <span style="color: #339933;">=></span>
  \SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">value</span><span style="color: #009900;">&#40;</span>
   SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">name</span><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"username"</span> <span style="color: #339933;">=></span> <span style="color: #0000ff;">$PORTAL_USERNAME</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">,</span>
   SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">name</span><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"apiKey"</span> <span style="color: #339933;">=></span> <span style="color: #0000ff;">$API_KEY</span><span style="color: #009900;">&#41;</span>
   <span style="color: #009900;">&#41;</span>
<span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># Create the object mask to mask what you're interested in. In this case we want</span>
<span style="color: #666666; font-style: italic;"># the datacenter and operating system associated with the hardware that's</span>
<span style="color: #666666; font-style: italic;"># asscoiated with our account.</span>
<span style="color: #666666; font-style: italic;"># see <http://sldn.softlayer.com/wiki/index.php/Using_Object_Masks_in_the_SoftLayer_API></span>
<span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$objectMask</span> <span style="color: #339933;">=</span> SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">name</span><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"ns1:SoftLayer_AccountObjectMask"</span> <span style="color: #339933;">=></span>
    \SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">value</span><span style="color: #009900;">&#40;</span>
        SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">name</span><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"mask"</span> <span style="color: #339933;">=></span>
            \SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">value</span><span style="color: #009900;">&#40;</span>
                SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">name</span><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"hardware"</span> <span style="color: #339933;">=></span>
                    \SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">value</span><span style="color: #009900;">&#40;</span>
                        SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">name</span><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"datacenter"</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">,</span>
                        SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Header</span><span style="color: #339933;">-></span><span style="color: #006600;">name</span><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"operatingSystem"</span><span style="color: #009900;">&#41;</span>
                    <span style="color: #009900;">&#41;</span>
                <span style="color: #009900;">&#41;</span>
            <span style="color: #009900;">&#41;</span>
        <span style="color: #009900;">&#41;</span><span style="color: #339933;">,</span>
    <span style="color: #009900;">&#41;</span>
<span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># Define the method you wish to call</span>
<span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$method</span> <span style="color: #339933;">=</span> SOAP<span style="color: #339933;">::</span><span style="color: #006600;">Data</span><span style="color: #339933;">-></span><span style="color: #006600;">name</span><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">'getObject'</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># Call the method and assign the response to the $response object</span>
<span style="color: #666666; font-style: italic;"># Note here that we've combined all our header objects into an array. You can do</span>
<span style="color: #666666; font-style: italic;"># this as you go if you like.</span>
<span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$response</span> <span style="color: #339933;">=</span> <span style="color: #0000ff;">$soap</span><span style="color: #339933;">-></span><span style="color: #006600;">call</span><span style="color: #009900;">&#40;</span><span style="color: #0000ff;">$method</span><span style="color: #339933;">,</span> <span style="color: #009900;">&#40;</span><span style="color: #0000ff;">$header</span><span style="color: #339933;">,</span> <span style="color: #0000ff;">$initParams</span><span style="color: #339933;">,</span> <span style="color: #0000ff;">$objectMask</span><span style="color: #009900;">&#41;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
&nbsp;
<span style="color: #666666; font-style: italic;"># Error-checking...</span>
<span style="color: #b1b100;">if</span> <span style="color: #009900;">&#40;</span><span style="color: #0000ff;">$response</span><span style="color: #339933;">-></span><span style="color: #006600;">fault</span><span style="color: #009900;">&#41;</span>
<span style="color: #009900;">&#123;</span>
    <a href="http://perldoc.perl.org/functions/print.html"><span style="color: #000066;">print</span></a> <span style="color: #0000ff;">$response</span><span style="color: #339933;">-></span><span style="color: #006600;">faultcode</span><span style="color: #339933;">,</span> <span style="color: #ff0000;">" "</span><span style="color: #339933;">,</span> <span style="color: #0000ff;">$response</span><span style="color: #339933;">-></span><span style="color: #006600;">faultstring</span><span style="color: #339933;">,</span> <span style="color: #ff0000;">"<span style="color: #000099; font-weight: bold;">\n</span>"</span><span style="color: #339933;">;</span>
    <a href="http://perldoc.perl.org/functions/die.html"><span style="color: #000066;">die</span></a> <span style="color: #ff0000;">'Terminating program'</span><span style="color: #339933;">;</span>
<span style="color: #009900;">&#125;</span>
<span style="color: #b1b100;">else</span>
<span style="color: #009900;">&#123;</span>
    <span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$account</span> <span style="color: #339933;">=</span> <span style="color: #0000ff;">$response</span><span style="color: #339933;">-></span><span style="color: #006600;">result</span><span style="color: #339933;">;</span>
    <span style="color: #b1b100;">my</span> <span style="color: #0000ff;">@hardwareObjects</span> <span style="color: #339933;">=</span> <span style="color: #339933;">@</span><span style="color: #009900;">&#123;</span><span style="color: #0000ff;">$account</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>hardware<span style="color: #009900;">&#125;</span><span style="color: #009900;">&#125;</span><span style="color: #339933;">;</span>
&nbsp;
    <span style="color: #666666; font-style: italic;"># Print table headers</span>
    <a href="http://perldoc.perl.org/functions/print.html"><span style="color: #000066;">print</span></a> <span style="color: #ff0000;">"+------------+-------------------------------------------------------------+---------------------------+--------------+<span style="color: #000099; font-weight: bold;">\n</span>"</span><span style="color: #339933;">;</span>
    <a href="http://perldoc.perl.org/functions/print.html"><span style="color: #000066;">print</span></a> <span style="color: #ff0000;">"| HardwareId | Host Name                                                   | OS Version                | Data Center  |<span style="color: #000099; font-weight: bold;">\n</span>"</span><span style="color: #339933;">;</span>
    <a href="http://perldoc.perl.org/functions/print.html"><span style="color: #000066;">print</span></a> <span style="color: #ff0000;">"+------------+-------------------------------------------------------------+---------------------------+--------------+<span style="color: #000099; font-weight: bold;">\n</span>"</span><span style="color: #339933;">;</span>
&nbsp;
    <span style="color: #666666; font-style: italic;"># Loop through the hardware objects and print their data</span>
    <span style="color: #b1b100;">foreach</span> <span style="color: #b1b100;">my</span> <span style="color: #0000ff;">$hardware</span> <span style="color: #009900;">&#40;</span><span style="color: #0000ff;">@hardwareObjects</span><span style="color: #009900;">&#41;</span> <span style="color: #009900;">&#123;</span>
        <a href="http://perldoc.perl.org/functions/printf.html"><span style="color: #000066;">printf</span></a><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"| %10s "</span><span style="color: #339933;">,</span> <span style="color: #0000ff;">$hardware</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>id<span style="color: #009900;">&#125;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
        <a href="http://perldoc.perl.org/functions/printf.html"><span style="color: #000066;">printf</span></a><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"| %-59s "</span><span style="color: #339933;">,</span> <span style="color: #0000ff;">$hardware</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>hostname<span style="color: #009900;">&#125;</span> . <span style="color: #ff0000;">"."</span> . <span style="color: #0000ff;">$hardware</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>domain<span style="color: #009900;">&#125;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
        <a href="http://perldoc.perl.org/functions/printf.html"><span style="color: #000066;">printf</span></a><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"| %-25s "</span><span style="color: #339933;">,</span> <span style="color: #0000ff;">$hardware</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>operatingSystem<span style="color: #009900;">&#125;</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>softwareLicense<span style="color: #009900;">&#125;</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>softwareDescription<span style="color: #009900;">&#125;</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>manufacturer<span style="color: #009900;">&#125;</span> . <span style="color: #ff0000;">" "</span> . <span style="color: #0000ff;">$hardware</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>operatingSystem<span style="color: #009900;">&#125;</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>softwareLicense<span style="color: #009900;">&#125;</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>softwareDescription<span style="color: #009900;">&#125;</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>version<span style="color: #009900;">&#125;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
        <a href="http://perldoc.perl.org/functions/printf.html"><span style="color: #000066;">printf</span></a><span style="color: #009900;">&#40;</span><span style="color: #ff0000;">"| %12s |<span style="color: #000099; font-weight: bold;">\n</span>"</span><span style="color: #339933;">,</span> <span style="color: #0000ff;">$hardware</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>datacenter<span style="color: #009900;">&#125;</span><span style="color: #339933;">-></span><span style="color: #009900;">&#123;</span>longName<span style="color: #009900;">&#125;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
    <span style="color: #009900;">&#125;</span>
&nbsp;
    <span style="color: #666666; font-style: italic;"># Print the table footer</span>
    <a href="http://perldoc.perl.org/functions/print.html"><span style="color: #000066;">print</span></a> <span style="color: #ff0000;">"+------------+-------------------------------------------------------------+---------------------------+--------------+<span style="color: #000099; font-weight: bold;">\n</span>"</span><span style="color: #339933;">;</span>
<span style="color: #009900;">&#125;</span></pre></div>
<h2>See Also</h2>
<p>*<a href="/reference/services/SoftLayer_Account/">SoftLayer_Account</a><br />
*<a href="/reference/services/SoftLayer_Hardware/">SoftLayer_Hardware</a><br />
*<a href="/article/Authenticating to the SoftLayer API">Authenticating to the SoftLayer API</a><br />
*<a href="/article/Using Initialization Parameters in the SoftLayer API">Using Initialization Parameters in the SoftLayer API</a><br />
*<a href="/article/Using Object Masks in the SoftLayer API">Using Object Masks in the SoftLayer API</a></p>
<h2>Associated Methods</h2>
<p>*<a href="/reference/services/SoftLayer_Account/getHardware">SoftLayer_Account::getHardware</a><br />
*<a href="/reference/services/SoftLayer_Account/getObject">SoftLayer_Account::getObject</a><br />
*<a href="/reference/services/SoftLayer_Hardware/getObject">SoftLayer_Hardware::getObject</a></p>
<h2>External Links</h2>
<p>*<a href="http://www.perl.org/">The Perl Directory at perl.org</a><br />
*<a href="http://sourceforge.net/project/showfiles.php?group_id=66000">SOAP::Lite</a> at <a href="http://sourceforge.net/">SourceForge</a></p>