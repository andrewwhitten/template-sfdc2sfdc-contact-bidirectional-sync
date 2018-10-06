
# Anypoint Template: Salesforce to Salesforce Contact Bidirectional Synchronization

+ [License Agreement](#licenseagreement)
+ [Use Case](#usecase)
+ [Considerations](#considerations)
	* [Salesforce Considerations](#salesforceconsiderations)
+ [Run it!](#runit)
	* [Running on premise](#runonopremise)
	* [Running on Studio](#runonstudio)
	* [Running on Mule ESB stand alone](#runonmuleesbstandalone)
	* [Running on CloudHub](#runoncloudhub)
	* [Deploying your Anypoint Template on CloudHub](#deployingyouranypointtemplateoncloudhub)
	* [Properties to be configured (With examples)](#propertiestobeconfigured)
+ [API Calls](#apicalls)
+ [Customize It!](#customizeit)
	* [config.xml](#configxml)
	* [businessLogic.xml](#businesslogicxml)
	* [endpoints.xml](#endpointsxml)
	* [errorHandling.xml](#errorhandlingxml)


# License Agreement <a name="licenseagreement"/>
Note that using this template is subject to the conditions of this [License Agreement](AnypointTemplateLicense.pdf).
Please review the terms of the license before downloading and using this template. In short, you are allowed to use the template for free with Mule ESB Enterprise Edition, CloudHub, or as a trial in Anypoint Studio.

# Use Case <a name="usecase"/>
As a Salesforce administrator I want to have my contacts synchronized between two different Salesforce organizations.

This template serves as a foundation for setting an online bidirectional sync of contacts between 
two Salesforce instances, and being able to specify filtering criteria. 

The  main behavior for the integration is polling for changes in new contacts or modified ones using the scheduler 
component. The polled changes are for those that have occurred in any of the Salesforce instances during a  
defined period of time. For those contacts that have not been updated yet, the integration triggers an 
upsert (update or create depending the case) taking the last modification as the one that should be applied.

Requirements have been set not only to be used as examples, but also to establish starting points 
to adapt the integration to any given requirements.

# Considerations <a name="considerations"/>

To make this template run, there are certain preconditions that must be considered. All of 
them deal with the preparations in both, that must be made for all to run smoothly. Failing 
to do so can lead to unexpected behavior of the template.



## Salesforce Considerations <a name="salesforceconsiderations"/>

There may be a few things that you need to know regarding Salesforce, in order for this template to work.

In order to have this template working as expected, you should be aware of your own Salesforce field configuration.

### FAQ

 - Where can I check that the field configuration for my Salesforce instance is the right one?

    [Salesforce: Checking Field Accessibility for a Particular Field][1]

- Can I modify the Field Access Settings? How?

    [Salesforce: Modifying Field Access Settings][2]


[1]: https://help.salesforce.com/HTViewHelpDoc?id=checking_field_accessibility_for_a_particular_field.htm&language=en_US
[2]: https://help.salesforce.com/HTViewHelpDoc?id=modifying_field_access_settings.htm&language=en_US

### As source of data

If the user configured in the template for the source system does not have at least *read only* permissions for the fields that are fetched, then a *InvalidFieldFault* API fault will show up.

```
java.lang.RuntimeException: [InvalidFieldFault [ApiQueryFault [ApiFault  exceptionCode='INVALID_FIELD'
exceptionMessage='
Account.Phone, Account.Rating, Account.RecordTypeId, Account.ShippingCity
^
ERROR at Row:1:Column:486
No such column 'RecordTypeId' on entity 'Account'. If you are attempting to use a custom field, be sure to append the '__c' after the custom field name. Please reference your WSDL or the describe call for the appropriate names.'
]
row='1'
column='486'
]
]
```

### As destination of data

There are no particular considerations for this Anypoint Template regarding Salesforce as data destination.









# Run it! <a name="runit"/>
Simple steps to get Salesforce to Salesforce Contact Bidirectional Synchronization running.
To have your application up and running:

1. Configure the application properties.
2. Run it on premises or in CloudHub (Runtime Manager).

## Running on premise <a name="runonopremise"/>
In this section we detail the way you should run your Anypoint Template on your computer.


### Where to Download Mule Studio and Mule ESB
First thing to know if you are a newcomer to Mule is where to get the tools.

+ You can download Mule Studio from this [Location](http://www.mulesoft.com/platform/mule-studio)
+ You can download Mule ESB from this [Location](http://www.mulesoft.com/platform/soa/mule-esb-open-source-esb)


### Importing an Anypoint Template into Studio
Mule Studio offers several ways to import a project into the workspace, for instance: 

+ Anypoint Studio Project from File System
+ Packaged mule application (.jar)

You can find a detailed description on how to do so in this [Documentation Page](http://www.mulesoft.org/documentation/display/current/Importing+and+Exporting+in+Studio).


### Running on Studio <a name="runonstudio"/>
Once you have imported you Anypoint Template into Anypoint Studio you need to follow these steps to run it:

+ Locate the properties file `mule.dev.properties`, in src/main/resources
+ Complete all the properties required as per the examples in the section [Properties to be configured](#propertiestobeconfigured)
+ Once that is done, right click on you Anypoint Template project folder 
+ Hover you mouse over `"Run as"`
+ Click on  `"Mule Application (configure)"`
+ Inside the dialog, select Environment and set the variable `"mule.env"` to the value `"dev"`
+ Click `"Run"`


### Running on Mule ESB stand alone <a name="runonmuleesbstandalone"/>
Fill in the properties in one of the property files, for example in 
mule.dev.properties, and run your app 
with the corresponding environment variable to use it. To follow the example, use the `mule.env=dev` value. 


## Running on CloudHub <a name="runoncloudhub"/>
While [creating your application on CloudHub](http://www.mulesoft.org/documentation/display/current/Hello+World+on+CloudHub) (Or you can do it later as a next step), you need to go to Deployment > Advanced to set all environment variables detailed in **Properties to be configured** as well as the **mule.env**.
To create your application on CloudHub, go to Anypoint Platform > Runtime Manager > Deployment > Advanced 
to set the environment variables listed in "Properties to Configure" as well as in the **mule.env** file.

### Deploying your Anypoint Template on CloudHub <a name="deployingyouranypointtemplateoncloudhub"/>
Mule Studio provides you with really easy way to deploy your Template directly to CloudHub, for the specific steps to do so please check this [link](http://www.mulesoft.org/documentation/display/current/Deploying+Mule+Applications#DeployingMuleApplications-DeploytoCloudHub)


## Properties to be configured (With examples) <a name="propertiestobeconfigured"/>
In order to use this Mule Anypoint Template you need to configure properties (Credentials, configurations, etc.) either in properties file or in CloudHub as Environment Variables. Detail list with examples:
### Application configuration
#### Application Configuration

+ scheduler.frequency `10000`  
The milliseconds between two different checks for updates in either Salesforce instance.

+ scheduler.startDelay `0`

+ watermark.default.expression `2018-02-25T11:00:00.000Z`  
This property is important, as it configures the starting point of the synchronization. If the 
use case includes synchronization for every contact created from the beginning of the time, you should use a 
date previous to any contact creation, such as `1900-01-01T08:00:00.000Z`). 
If you want to synchronize the contacts created from now on, then use a default value according to 
that requirement, for example, if today is April 21, 2018 and eleven o'clock in London, 
use the `2018-04-21T11:00:00.000Z` value.

+ page.size `1000`

+ account.sync.policy `syncAccount`
**Note:** the property **account.sync.policy** can take any of the two following values: 
+ **empty_value**: If the property has no value assigned to it then application will do nothing in what respect to the account and it'll just move the contact over.
+ **syncAccount**: It tries to create the contact's account when it is not pressented in the Salesforce instance B.

#### SalesForce Connector Configuration for Company A

+ sfdc.a.username `aunt.eater@mail.com`
+ sfdc.a.password `G0ttaF1ndTh3m!!`
+ sfdc.a.securityToken `avsfwCUl7apQs56Xq2AKi3X`
+ sfdc.a.url `https://login.salesforce.com/services/Soap/u/42.0`
+ sfdc.a.integration.user.id `A0ed000BO9T`

**Note:** To find the correct *sfdc.a.integration.user.id* value, refer to the 
example project "Salesforce Data Retrieval" in Anypoint Exchange.

#### SalesForce Connector Configuration for Company B

+ sfdc.b.username `polly.hedra@example.com`
+ sfdc.b.password `WootWoot99!!`
+ sfdc.b.securityToken `ces56arl7apQs56XTddf34X`
+ sfdc.b.url `https://login.salesforce.com/services/Soap/u/42.0`
+ sfdc.b.integration.user.id `B0ed000BO9T`

**Note:** To find the correct *sfdc.b.integration.user.id* value, refer to the example 
project "Salesforce Data Retrieval" in Anypoint Exchange.

# API Calls <a name="apicalls"/>
Not relevant for this use case.


# Customize It!<a name="customizeit"/>
This brief guide intends to give a high level idea of how this Anypoint Template is built and how you can change it according to your needs.
As mule applications are based on XML files, this page will be organized by describing all the XML that conform the Anypoint Template.
Of course more files will be found such as Test Classes and [Mule Application Files](http://www.mulesoft.org/documentation/display/current/Application+Format), but to keep it simple we will focus on the XMLs.

Here is a list of the main XML files you'll find in this application:

* [config.xml](#configxml)
* [endpoints.xml](#endpointsxml)
* [businessLogic.xml](#businesslogicxml)
* [errorHandling.xml](#errorhandlingxml)


## config.xml<a name="configxml"/>
Configuration for Connectors and [Configuration Properties](http://www.mulesoft.org/documentation/display/current/Configuring+Properties) are set in this file. **Even you can change the configuration here, all parameters that can be modified here are in properties file, and this is the recommended place to do it so.** Of course if you want to do core changes to the logic you will probably need to modify this file.

In the visual editor they can be found on the *Global Element* tab.


## businessLogic.xml<a name="businesslogicxml"/>
This file holds the functional aspect of the template. Its main component is a Mule batch job, and 
it includes steps for both executing the synchronization from Salesforce A to Salesforce B, and the other way around.



## endpoints.xml<a name="endpointsxml"/>
Use this file to contain each inbound and outbound endpoint for your integration app. 
In this template, this file contains a scheduler endpoint that queries Salesforce A and Salesforce B 
for updates using watermark.



## errorHandling.xml<a name="errorhandlingxml"/>
Use this file for how your integration reacts depending on the different exceptions. This file 
holds an error handler that is referenced by the scheduler flow in the endpoints XML file.



