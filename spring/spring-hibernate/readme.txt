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


Specified the host and port for the Context

         <protocol type="Servlet 3.0">
            <property name="host">172.17.0.3</property>
            <property name="port">8080</property>
            <!--  contextRoot -->
         </protocol>
-------------------------------------------------------------------------

Changed to use the Chameleon Arquillian Connector which supports EAP7

testUpdate(com.acme.spring.hibernate.repository.impl.HibernateStockRepositoryTestCase)  Time elapsed: 0.072 sec  <<< ERROR!
java.lang.NoClassDefFoundError: org/springframework/web/context/WebApplicationContext
    at java.lang.Class.getDeclaredMethods0(Native Method)
    at java.lang.Class.privateGetDeclaredMethods(Class.java:2701)
    at java.lang.Class.getDeclaredMethods(Class.java:1975)
    at org.jboss.arquillian.core.impl.Reflections.getObserverMethods(Reflections.java:52)
    at org.jboss.arquillian.core.impl.ExtensionImpl.of(ExtensionImpl.java:55)
    at org.jboss.arquillian.core.impl.ManagerImpl.inject(ManagerImpl.java:211)
    at org.jboss.arquillian.core.impl.InjectorImpl.inject(InjectorImpl.java:58)
    at org.jboss.arquillian.core.impl.loadable.ServiceRegistryLoader.createServiceInstance(ServiceRegistryLoader.java:108)
    at org.jboss.arquillian.core.impl.loadable.ServiceRegistryLoader.all(ServiceRegistryLoader.java:55)
    at org.jboss.arquillian.spring.integration.container.SpringContainerApplicationContextProducer.initApplicationContext(SpringContainerApplicationContextProducer.java:72)
    at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
    at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
    at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
    at java.lang.reflect.Method.invoke(Method.java:498)
    at org.jboss.arquillian.core.impl.ObserverImpl.invoke(ObserverImpl.java:96)
    at org.jboss.arquillian.core.impl.EventContextImpl.invokeObservers(EventContextImpl.java:99)
    at org.jboss.arquillian.core.impl.EventContextImpl.proceed(EventContextImpl.java:81)
    at org.jboss.arquillian.test.impl.TestContextHandler.createClassContext(TestContextHandler.java:92)
    at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
    at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
    at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
    at java.lang.reflect.Method.invoke(Method.java:498)
    at org.jboss.arquillian.core.impl.ObserverImpl.invoke(ObserverImpl.java:96)
    at org.jboss.arquillian.core.impl.EventContextImpl.proceed(EventContextImpl.java:88)
    at org.jboss.arquillian.test.impl.TestContextHandler.createSuiteContext(TestContextHandler.java:73)
    at sun.reflect.GeneratedMethodAccessor38.invoke(Unknown Source)
    at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
    at java.lang.reflect.Method.invoke(Method.java:498)
    at org.jboss.arquillian.core.impl.ObserverImpl.invoke(ObserverImpl.java:96)
    at org.jboss.arquillian.core.impl.EventContextImpl.proceed(EventContextImpl.java:88)
    at org.jboss.arquillian.core.impl.ManagerImpl.fire(ManagerImpl.java:145)
    at org.jboss.arquillian.core.impl.ManagerImpl.fire(ManagerImpl.java:116)
    at org.jboss.arquillian.test.impl.EventTestRunnerAdaptor.beforeClass(EventTestRunnerAdaptor.java:87)
    at org.jboss.arquillian.junit.Arquillian$2.evaluate(Arquillian.java:202)
    at org.jboss.arquillian.junit.Arquillian.multiExecute(Arquillian.java:431)
    at org.jboss.arquillian.junit.Arquillian.access$200(Arquillian.java:55)
    at org.jboss.arquillian.junit.Arquillian$3.evaluate(Arquillian.java:219)
    at org.junit.runners.ParentRunner.run(ParentRunner.java:363)
    at org.jboss.arquillian.junit.Arquillian.run(Arquillian.java:167)
    at org.junit.runner.JUnitCore.run(JUnitCore.java:137)
    at org.junit.runner.JUnitCore.run(JUnitCore.java:115)
    at org.jboss.arquillian.junit.container.JUnitTestRunner.execute(JUnitTestRunner.java:66)
    at org.jboss.arquillian.protocol.servlet.runner.ServletTestRunner.executeTest(ServletTestRunner.java:170)
    at org.jboss.arquillian.protocol.servlet.runner.ServletTestRunner.execute(ServletTestRunner.java:135)
    at org.jboss.arquillian.protocol.servlet.runner.ServletTestRunner.doGet(ServletTestRunner.java:98)
    at javax.servlet.http.HttpServlet.service(HttpServlet.java:687)
    at javax.servlet.http.HttpServlet.service(HttpServlet.java:790)
    at io.undertow.servlet.handlers.ServletHandler.handleRequest(ServletHandler.java:85)
    at io.undertow.servlet.handlers.security.ServletSecurityRoleHandler.handleRequest(ServletSecurityRoleHandler.java:62)
    at io.undertow.servlet.handlers.ServletDispatchingHandler.handleRequest(ServletDispatchingHandler.java:36)
    at org.wildfly.extension.undertow.security.SecurityContextAssociationHandler.handleRequest(SecurityContextAssociationHandler.java:78)
    at io.undertow.server.handlers.PredicateHandler.handleRequest(PredicateHandler.java:43)
    at io.undertow.servlet.handlers.security.SSLInformationAssociationHandler.handleRequest(SSLInformationAssociationHandler.java:131)
    at io.undertow.servlet.handlers.security.ServletAuthenticationCallHandler.handleRequest(ServletAuthenticationCallHandler.java:57)
    at io.undertow.server.handlers.PredicateHandler.handleRequest(PredicateHandler.java:43)
    at io.undertow.security.handlers.AbstractConfidentialityHandler.handleRequest(AbstractConfidentialityHandler.java:46)
    at io.undertow.servlet.handlers.security.ServletConfidentialityConstraintHandler.handleRequest(ServletConfidentialityConstraintHandler.java:64)
    at io.undertow.security.handlers.AuthenticationMechanismsHandler.handleRequest(AuthenticationMechanismsHandler.java:60)
    at io.undertow.servlet.handlers.security.CachedAuthenticatedSessionHandler.handleRequest(CachedAuthenticatedSessionHandler.java:77)
    at io.undertow.security.handlers.NotificationReceiverHandler.handleRequest(NotificationReceiverHandler.java:50)
    at io.undertow.security.handlers.AbstractSecurityContextAssociationHandler.handleRequest(AbstractSecurityContextAssociationHandler.java:43)
    at io.undertow.server.handlers.PredicateHandler.handleRequest(PredicateHandler.java:43)
    at org.wildfly.extension.undertow.security.jacc.JACCContextIdHandler.handleRequest(JACCContextIdHandler.java:61)
    at io.undertow.server.handlers.PredicateHandler.handleRequest(PredicateHandler.java:43)
    at io.undertow.server.handlers.PredicateHandler.handleRequest(PredicateHandler.java:43)
    at io.undertow.servlet.handlers.ServletInitialHandler.handleFirstRequest(ServletInitialHandler.java:285)
    at io.undertow.servlet.handlers.ServletInitialHandler.dispatchRequest(ServletInitialHandler.java:264)
    at io.undertow.servlet.handlers.ServletInitialHandler.access$000(ServletInitialHandler.java:81)
    at io.undertow.servlet.handlers.ServletInitialHandler$1.handleRequest(ServletInitialHandler.java:175)
    at io.undertow.server.Connectors.executeRootHandler(Connectors.java:202)
    at io.undertow.server.HttpServerExchange$1.run(HttpServerExchange.java:792)
    at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142)
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617)
    at java.lang.Thread.run(Thread.java:745)
Caused by: java.lang.ClassNotFoundException: org.springframework.web.context.WebApplicationContext from [Module "deployment.spring-test.war:main" from Service Module Loader]
    at org.jboss.modules.ModuleClassLoader.findClass(ModuleClassLoader.java:198)
    at org.jboss.modules.ConcurrentClassLoader.performLoadClassUnchecked(ConcurrentClassLoader.java:363)
    at org.jboss.modules.ConcurrentClassLoader.performLoadClass(ConcurrentClassLoader.java:351)
    at org.jboss.modules.ConcurrentClassLoader.loadClass(ConcurrentClassLoader.java:93)


Added spring-web to the Deployable archive

-------------------------------------------------------------------------------------------------------------------


.enhanced.SequenceStyleGenerator]
17:49:38,325 DEBUG [org.hibernate.id.factory.DefaultIdentifierGeneratorFactory] (default task-39) Registering IdentifierGenerator strategy [enhanced-table] -> [class org.hibernate.id.enhanced.TableGenerator]
17:49:38,331 DEBUG [org.hibernate.cfg.AnnotationConfiguration] (default task-39) Execute first pass mapping processing
17:49:38,332 WARN  [org.jboss.modules] (default task-39) Failed to define class org.hibernate.cfg.ExtendedMappings in Module "deployment.spring-test.war:main" from Service Module Loader: java.lang.IncompatibleClassChangeError: Failed to link org/hibernate/cfg/ExtendedMappings (Module "deployment.spring-test.war:main" from Service Module Loader): class org.hibernate.cfg.ExtendedMappings has interface org.hibernate.cfg.Mappings as super class
    at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
    at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
    at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)


Caused by: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'sessionFactory' defined in class path resource [applicationContext-repository.xml]: Invocation of init method failed; nested exception is java.lang.IncompatibleClassChangeError: Failed to link org/hibernate/cfg/ExtendedMappings (Module "deployment.spring-test.war:main" from Service Module Loader): class org.hibernate.cfg.ExtendedMappings has interface org.hibernate.cfg.Mappings as super class
    at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1455)
    at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:519)
    at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:456)
    at org.springframework.beans.factory.support.AbstractBeanFactory$1.getObject(AbstractBeanFactory.java:294)
    at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:225)
    at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:291)
    at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:193)
    at org.springframework.beans.factory.support.DefaultListableBeanFactory.preInstantiateSingletons(DefaultListableBeanFactory.java:567)
    at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:913)
    at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:464)
    at org.springframework.context.support.ClassPathXmlApplicationContext.<init>(ClassPathXmlApplicationContext.java:139)
    at org.springframework.context.support.ClassPathXmlApplicationContext.<init>(ClassPathXmlApplicationContext.java:93)
    ... 74 more
Caused by: java.lang.IncompatibleClassChangeError: Failed to link org/hibernate/cfg/ExtendedMappings (Module "deployment.spring-test.war:main" from Service Module Loader): class org.hibernate.cfg.ExtendedMappings has interface org.hibernate.cfg.Mappings as super class
    at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
    at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
    at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
    at java.lang.reflect.Constructor.newInstance(Constructor.java:423)

-----------------------------------------------------------------------------------------------------------------------------

