# jenkins

## jenkins backup   for recovery
use below url to take jenkins backup
using EBS snapshot create a disk and attched to a mount point ---> /jenkins_backup
### backup
dest ---> /jenkins_backup
source ---> /var/lib/jenkins of old server

### Restore
 dest ---> /var/lib/jenkins of new server
 source ---> /jenkins_backup

https://devopscube.com/jenkins-backup-data-configurations/


---------------------------------------------
