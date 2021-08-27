# update-jenkins-service-account-user
Changing the Jenkins service account user


Solution
There is a two-fold process that must be run in order to resolve this issue. Changing the directory ownership of Jenkins to talenduser:talenduser is the first part, but you must also change the Jenkins user.

Note: You might check the /etc/init.d/jenkins script because this location contains two crucial variables: $JENKINS_CONFIG(=/etc/sysconfig/jenkins) and $JENKINS_USER. You might think that changing the JENKINS_USER variable would work, but this is actually not the correct way to rectify the issue.

To change the service, open the /etc/sysconfig/jenkins (in Debian [Ubuntu] this file is created in /etc/default) and change the JENKINS_USER to the user you want. In this example, it's talenduser because that is the user that owns the product suite on this machine. Make sure that the user you are changing to actually exists in the system (you can check the user in the /etc/passwd file if you are unsure).
$JENKINS_USER="talenduser"
 
Run the following commands to ensure you did completely change the ownership of the Jenkins home, Jenkins webroot, and logs:
chown -R talenduser:talenduser /var/lib/jenkins
chown -R talenduser:talenduser /var/cache/jenkins
chown -R talenduser:talenduser /var/log/jenkins
 
After changing ownership of the directories above, restart the Jenkins service and use a ps command to check if the user has changed:

/etc/init.d/jenkins restart or systemctl restart jenkins

ps -ef | grep jenkins
