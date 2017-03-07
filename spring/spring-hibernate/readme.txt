-------------------------------------------------------
Setup docker images

-------------------------------------------------------
Open management port

-------------------------------------------------------
Get await strategy working

-------------------------------------------------------
Investigate JDK /overlay2 bug

There seems to an overlayfs or Java  bug which is causing deployment exceptions in EAP7

https://issues.jboss.org/browse/WFCORE-2301

https://bugs.openjdk.java.net/browse/JDK-8165852
"If the sub-volume is mounted via `mount` it shows up in /proc/mounts and the error does not occur"

There is a work around in Wildfly Core here ->
https://github.com/wildfly/wildfly-core/pull/2188

ConfigurationFilePersistenceResource
Wildfly controller does deployment
./eap7/modules/system/layers/base/org/jboss/as/controller/main/wildfly-controller-2.1.2.Final-redhat-1.jar

After remote debugging EAP7, the error shown in the log is not critical
-------------------------------------------------------
JUNIT runrule

testUpdate(com.acme.spring.hibernate.repository.impl.HibernateStockRepositoryTestCase)  Time elapsed: 0.002 sec  <<< ERROR!
java.lang.NoClassDefFoundError: org/junit/rules/RunRules
        at org.jboss.arquillian.junit.RulesEnricher.enrichStatement(RulesEnricher.java:76)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorIm

------------------------------------------------------------


testSave(com.acme.spring.hibernate.repository.impl.HibernateStockRepositoryTestCase)  Time elapsed: 0.018 sec  <<< ERROR!
java.lang.IllegalArgumentException: ArquillianServletRunner not found. Could not determine ContextRoot from ProtocolMetadata, please contact DeployableContainer developer.
        at org.jboss.arquillian.protocol.servlet.ServletUtil.determineBaseURI(ServletUtil.java:69)
        at org.jboss.arquillian.protocol.servlet.ServletURIHandler.locateTestServlet(ServletURIHandler.java:60)
        at org.jboss.arquillian.protocol.servlet.ServletMethodExecutor.invoke(ServletMethodExecutor.java:84)
        at org.jboss.arquillian.container.test.impl.execution.RemoteTestExecuter.execute(RemoteTestExecuter.java:109)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at org.jboss.arquillian.core.impl.ObserverImpl.invoke(ObserverImpl.java:96)



-------------------------------------------------------------------------