rewards-basic-tomcat
=============

rewards-basic for Tomcat 7.

Firstly, you need to setup datasource and transaction in Tomcat. Follow the instruction.

https://access.redhat.com/documentation/en-US/Red_Hat_JBoss_BPM_Suite/6.2/html-single/Installation_Guide/index.html#Setting_up_transaction_manager

Here are simplified explanation of the instruction
========
1. Copy following files to $TOMCAT/lib

btm-2.1.4.jar
btm-tomcat55-lifecycle-2.1.4.jar
jta-1.1.jar
slf4j-api-1.7.2.redhat-3.jar
slf4j-jdk14-1.7.2.redhat-3.jar
javax.security.jacc-api-1.5.jar
h2-1.3.168.redhat-4.jar
kie-tomcat-integration-6.3.0.Final-redhat-5.jar
jboss-jaxb-api_2.2_spec-1.0.4.Final-redhat-3.jar

2. Create btm-config.properties and resources.properties under $TOMCAT/conf

btm-config.properties
---
bitronix.tm.serverId=tomcat-btm-node0
bitronix.tm.journal.disk.logPart1Filename=${btm.root}/work/btm1.tlog
bitronix.tm.journal.disk.logPart2Filename=${btm.root}/work/btm2.tlog
bitronix.tm.resource.configuration=${btm.root}/conf/resources.properties
---

resources.properties
---
resource.ds1.className=bitronix.tm.resource.jdbc.lrc.LrcXADataSource
resource.ds1.uniqueName=jdbc/jbpm
resource.ds1.minPoolSize=10
resource.ds1.maxPoolSize=20
resource.ds1.driverProperties.driverClassName=org.h2.Driver
resource.ds1.driverProperties.url=jdbc:h2:file:~/jbpm
resource.ds1.driverProperties.user=sa
resource.ds1.driverProperties.password=
resource.ds1.allowLocalTransactions=true
---

3. Edit $TOMCAT_DIR/conf/server.xml

<Listener className="bitronix.tm.integration.tomcat55.BTMLifecycleListener" />

4. Create setenv.sh with the content (shortened)

CATALINA_OPTS="-Xmx512M -XX:MaxPermSize=512m -Dbtm.root=$CATALINA_HOME -Dbitronix.tm.configuration=$CATALINA_HOME/conf/btm-config.properties -Djbpm.tsr.jndi.lookup=java:comp/env/TransactionSynchronizationRegistry"


