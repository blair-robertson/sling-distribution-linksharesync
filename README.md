Sling Distribution configuration package for publish farm replication for the Assets Link Share functionality.

Replication ``/var/dam/share``


Building:
```
mvn clean install -PautoInstallPackage -PautoInstallPackagePublish -PautoInstallPackagePublish2 -Daem.port=34622 -Daem.publish.port=34623 -Daem.publish2.port=34625
```
