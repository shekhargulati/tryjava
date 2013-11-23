Run your Tomcat on OpenShift
============================
This git repository helps you get up and running quickly with a Tomcat 7.0.42 installation on OpenShift.

Create a Do It Yourself (DIY) app on OpenShift
----------------------------------------------
<a href="http://openshift.redhat.com/">Create an account</a> and install the <a href="https://www.openshift.com/get-started">command-line client tools</a>.

Create a DIY application:
    rhc app create -a tomcat -t diy-0.1

Get Tomcat running
----------------------------
Grab this quickstart project and make it work for you!

    cd tomcat
    git remote add upstream -m master git://github.com/openshift/openshift-tomcat-quickstart.git
    git pull -s recursive -X theirs upstream master
    git push

That's it, you can now checkout your tomcat at:
    http://tomcat-$yournamespace.rhcloud.com

By placing WARs (either WAR archives or exploded WARs) in the diy/tomcat/webapps folder,
those applications will be deployed and redeployed upon each <code>git push</code>.  Likewise,
applications (including the sample applications provided by Tomcat) can be deleted from that
folder so they will no longer be deployed.

The default managing account is tomcat/openshift; this can be changed by altering the 
diy/tomcat/conf/tomcat-users.xml file, committing the change within your secure OpenShift
Git account and issuing another <code>git push</code>.

Forking this Quickstart to use a newer Tomcat version
-----------------------------------------------------
See [here](HowToUpdate.md) for information.

License
-------
This code is dedicated to the public domain to the maximum extent
permitted by applicable law, pursuant to CC0
http://creativecommons.org/publicdomain/zero/1.0/

