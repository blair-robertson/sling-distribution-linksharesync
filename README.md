Sling Distribution configuration package for publish farm replication for the Assets Link Share functionality.

Replication ``/var/dam/share``


**Building:**

	mvn clean install


Default Setup
-------------
```
Author    : 34622
Publish 1 : 34623
Publish 2 : 34625

Links
------

	http://author.local:34622/libs/granite/distribution/content/distribution.html
	http://pub1.local:34623/libs/granite/distribution/content/distribution.html
	http://pub2.local:34625/libs/granite/distribution/content/distribution.html

Install Service Users first
---------------------------

	cd ui.service-users
	mvn clean install -PautoInstallPackage -PautoInstallPackagePublish -PautoInstallPackagePublish2 -Daem.port=34622 -Daem.publish.port=34623 -Daem.publish2.port=34625

Perform for Author and each Publish:

Creating service user and permissions uses the Netcentric ACL Tool:
https://github.com/Netcentric/accesscontroltool

Download and install the 2 packages.

Edit the configuration for each (could be set by config, but already used in another project this was part of): 
Set the **Configuration storage path** to ``/apps/linksharesync/acls``

	http://author.local:34622/system/console/configMgr/biz.netcentric.cq.tools.actool.aceservice.impl.AceServiceImpl
	http://pub1.local:34623/system/console/configMgr/biz.netcentric.cq.tools.actool.aceservice.impl.AceServiceImpl
	http://pub2.local:34625/system/console/configMgr/biz.netcentric.cq.tools.actool.aceservice.impl.AceServiceImpl
 


Run ``execute()`` from JMX console:

	http://author.local:34622/system/console/jmx/biz.netcentric.cq.tools%3Atype%3DACTool
	http://pub1.local:34623/system/console/jmx/biz.netcentric.cq.tools%3Atype%3DACTool
	http://pub2.local:34625/system/console/jmx/biz.netcentric.cq.tools%3Atype%3DACTool



Create Encrypted Password Transport Secret Provider
---------------------------------------------------

### Replicate the Crypto Keys

[https://docs.adobe.com/docs/en/aem/6-2/deploy/communities.html#Replicate the Crypto Key]


### Encrypt a password

1. Create/generate a password
2. On author access the Web Console: http://author.local:34622/system/console/crypto
3. Enter the password, click 'Protect'
4. Copy the encrypted password

### Create the Secret Provider

1. On author access the Web Console: http://author.local:34622/system/console/configMgr/
2. locate **Adobe Granite Distribution - Encrypted Password Transport Secret Provider**
3. Create a new configuration and enter the following details:

	name: linksharesync-publishUser
	usename: linkshare-admin
	password: <encrypted password from above>
	

### Set password on each publish

1. On each publish go to the User Admin and set the password for the **linkshare-admin** user

	http://pub1.local:34623/libs/granite/security/content/useradmin.html
	http://pub2.local:34625/libs/granite/security/content/useradmin.html



Install the UI.Apps
-------------------

	cd ui.apps
	mvn clean install -PautoInstallPackage -PautoInstallPackagePublish -PautoInstallPackagePublish2 -Daem.port=34622 -Daem.publish.port=34623 -Daem.publish2.port=34625



