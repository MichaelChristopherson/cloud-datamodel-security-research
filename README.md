Cloud Infrastructure Datamodel
==========

A Splunk datamodel for cloud infrastructure data (AWS / GCP / Azure)

*Join the conversation at* [![Slack Status](https://img.shields.io/badge/slack-@splunk/security-yellow.svg?logo=slack)](https://splunk-usergroups.slack.com/signup)

- Original Author: Rico Valdez
- Current maintainers:
- Sourcetype: aws:cloudtrail, aws:cloudwatchlogs:vpcflow, google:gcp:pubsub:message, mscs:azure:audit
- Has index-time ops: false



# Features
* Provides a datamodel suitable for normalizing some of the data coming from aws, gcp, and azure. 
	* Blocks for compute (VM Instances), storage, and network traffic
* Includes additional eventtypes, field aliases, tags, and calculations to help populate the model.
* Provides Datamodel Mapping for:
	* Basic VM activity (start, stop, create, terminate)
	* Basic bucket/object activity for storage use cases
	* Network traffic as provided by vpcflow logs, and gec_instance events for GCP


# Description

A Splunk data model is a type of knowledge object that applies an information structure to raw data at search time—regardless of the data's origin or format—and encodes the domain knowledge necessary to build a variety of specialized searches. Data models enhance the efficiency of your searches. You can increase their velocity with data-model acceleration, which creates summaries for the fields you report on, accelerating the dataset represented by those fields. 

The Cloud Infrastructure data model normalizes and combines your machine data from AWS, Azure, and/or GCP, so you can write analytics that work across all three.


# Pre-requisites

You should have the appropriate add-on for your cloud provider to bring data into splunk and perform the basic field extractions. These are the following

* AWS - Splunk Add-on for Amazon Web Services - https://splunkbase.splunk.com/app/1876/
* GCP - Splunk Add-on for Google Cloud Platform - https://splunkbase.splunk.com/app/3088/
* Azure - Splunk Add-on for Microsoft Cloud Services - https://splunkbase.splunk.com/app/3110/


# Installing the datamodel 

Method 1:

1. Download the .spl package and install as a normal app.
2. It may be necessary to move props.conf, tags.conf, and eventtypes.conf from the app default directory to the local directory (create it if it doesn't exist, at the same level as the default directory).


Method 2:

1. Clone or download the repo. 
2. Find the cloud_infrastructure.json file located in default/data/models
3. Navigate to Settings->Datamodels in your splunk instance
4. Click the 'upload' button in the top right of the page, and select the cloud_infrastrucutre.json file. You can install in whatever app context you like. 
5. Go back one level to the list of datamodels and click edit, then select "edit permissions"
6. Select 'all apps', and assign permissions so that everyone read and admins have write. Click save.
7. [optional] Click edit, then choose edit data model accelleration if you wish to accelerate the model. 
8. Drop the included props.conf, tags.conf, and eventtypes.conf in the <code>local</code> directory of the app context in which you installed, or create/update the associated files in the lcoal directory for the appropriate app. For example, create or modify <code>local/props.conf</code> in the app directory for the Amazon TA, to include the information under the aws stanzas in the included props.conf file.


# Testing the datamodel

The following search will show you how the datamodel is being populated with compute data -

<code>| datamodel Cloud_Infrastructure Compute search | table Compute*</code>

You can re-run the search, replacing both instances of "Compute" with "Storage" or "Traffic". You can also look at a specific provider by inserting a search, such as:

<code>| datamodel Cloud_Infrastructure Compute search | search sourcetype=aws:cloudtrail | table Compute*</code>


# Troubleshooting
If the data is not showing up, confirm that the provided conf files are properly deployed. They may need to be moved into an app <code>local</code> directory to take precedence over existing directives

Make sure the indexes containing your cloud_infrastructure data are searchable by default, or add the indexes to the definition for the eventtypes as necessary

If data is not properly mapped, consider making edits to the lines provided in props.conf

# To Do's
* Further testing/refinement of extractions and mappings


# Provided DM fields

<table here>
