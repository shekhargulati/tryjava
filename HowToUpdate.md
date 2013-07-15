Forking this Quickstart to use a newer Tomcat version
-----------------------------------------------------
<em>This page explains how to fork the Quickstart on GitHub in order to embed a different release version of Tomcat within it, see [the README](README.md) for regular usage instructions for this Quickstart. This step is <em>not</em> needed if the release version of Tomcat used by the standard Quickstart is fine for you.</em>

1.) <a href="https://help.github.com/articles/fork-a-repo">Fork</a> the <a href="https://github.com/openshift/openshift-tomcat-quickstart">OpenShift version</a> of the Quickstart and clone it to your local machine.  

2.) <a href="http://tomcat.apache.org/download-70.cgi">Download</a> the latest Tomcat 7.0.x release (.tar.gz or .zip version as appropriate
for your operating system), and check its 
<a href="http://www.apache.org/dev/release-signing.html#verifying-signature">PGP</a> and <a href="https://help.ubuntu.com/community/HowToMD5SUM">MD5</a> values to confirm your download is uncorrupted.

3.) Extract the compressed Tomcat file into the openshift-tomcat-quickstart/diy folder from Step #1.  You should now have two directories there: <code>tomcat</code> (for the older Tomcat version already in the Quickstart) and <code>apache-tomcat-7.0.xx</code> (the latest version).

4.) The Tomcat embedded in the Quickstart presently differs from the standard Tomcat download in just two files to accommodate OpenShift's deployment characteristics.  (Information below current as of Tomcat 7.0.42, future versions of Tomcat may introduce the need for additional changes.) The contents of the new <code>diy/apache-tomcat-7.0.xx</code> folder should be updated in the following manner:

In the tomcat/conf/server.xml file:

a.) Update the &lt;Server/> element from:

    <Server port="8005" shutdown="SHUTDOWN">
to:

    <Server address="OPENSHIFT_DIY_IP" port="15005" shutdown="SHUTDOWN">

b.) Update the &lt;Connector port="8080".../> element from:

    <Connector port="8080" redirectPort="8443" ...rest of values... />
to:

    <Connector port="8080" redirectPort="15443" address="OPENSHIFT_DIY_IP" ...rest of values... />

c.) Update the AJP connector from:

    <Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />
to:

    <Connector port="15009" address="OPENSHIFT_DIY_IP" protocol="AJP/1.3" redirectPort="8443" />

d.) Update the &lt;Engine /> Element from:

    <Engine name="Catalina" defaultHost="localhost">
to:

    <Engine name="Catalina" defaultHost="OPENSHIFT_DIY_IP">

e.) Update the &lt;Host /> element from:

    <Host name="localhost"  appBase="webapps"... />
to:

    <Host name="OPENSHIFT_APP_DNS"  appBase="webapps"...  />

In the tomcat/conf/tomcat-users.xml file, add these two elements:

    <role rolename="manager-gui"/>
    <user username="tomcat" password="openshift" roles="manager-gui"/>

IMPORTANT:  If you're forking this project to update the Tomcat version, *don't* change the username and password to something more secure *here*, as this file will be <a href="https://github.com/openshift/openshift-tomcat-quickstart/blob/master/diy/tomcat/conf/tomcat-users.xml">publicly available</a> for everyone to see.  It's only after you <code>git pull</code> this project into your secure OpenShift account following the <a href="https://github.com/openshift/openshift-tomcat-quickstart#get-tomcat-running">Get Tomcat Running section</a> in the README will you edit the username and password prior to doing a <code>git push</code> to your OpenShift account.

5.) After making the above changes move the old <code>diy/tomcat</code> folder out of the present project and rename the <code>apache-tomcat-xxx</code> folder to <code>tomcat</code>.  The <code>git status</code> and <code>git diff</code> commands will show that Git has detected the new files, which can be pushed to your public GitHub repository via the normal git add/commit/push commands.

