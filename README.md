maven-dependency-conflict-demo
==============================

This project is for demonstrating conflicts due to Maven's dependency resolution know as "nears win strategy"

resolve-web -> project-A -> project-common:1.0
resolve-web -> project-B -> project-C -> project-common:2.0

(1)go to project-common, run "mvn clean install". Then change project version from 1.0 to 2.0, and add a sayGoodBye() method in HelloWorld.java.

    public String sayGoodBye(){
        return "goodbye world";
    }

run "mvn clean install" again. Now local Maven repository should have both V1.0 and V2.0 for project-common.

(2)go to project-A, run "mvn clean install"

(3)go to project-C, run "mvn clean install"

(4)go to project-B, run "mvn clean install"

(5)go to resolve-web, run "mvn clean verify org.codehaus.cargo:cargo-maven2-plugin:run"

(6)open "http://localhost:8080/resolve-web/hello", you should see "hello world".

(7)open "http://localhost:8080/resolve-web/goodbye", you should see "java.lang.NoSuchMethodError: projectcommon.HelloWorld.sayGoodBye()Ljava/lang/String;", but we expect to see "goodbye world".


错误信息:
HTTP ERROR 500
Problem accessing /resolve-web/goodbye. Reason:

    Server Error
Caused by:
java.lang.NoSuchMethodError: projectcommon.HelloWorld.sayGoodBye()Ljava/lang/String;
	at projectC.GoodByeAdapter.sayGoodByeAdaptToHelloWorld(GoodByeAdapter.java:20)
	at projectB.GoodByeService.sayGoodBye(GoodByeService.java:20)
	at goodbye.GoodByeServlet.doGet(GoodByeServlet.java:28)
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:735)
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:848)
	at org.eclipse.jetty.servlet.ServletHolder.handle(ServletHolder.java:684)
	at org.eclipse.jetty.servlet.ServletHandler.doHandle(ServletHandler.java:501)
	at org.eclipse.jetty.server.handler.ScopedHandler.handle(ScopedHandler.java:137)
	at org.eclipse.jetty.security.SecurityHandler.handle(SecurityHandler.java:533)
	at org.eclipse.jetty.server.session.SessionHandler.doHandle(SessionHandler.java:231)
	at org.eclipse.jetty.server.handler.ContextHandler.doHandle(ContextHandler.java:1086)
	at org.eclipse.jetty.servlet.ServletHandler.doScope(ServletHandler.java:427)
	at org.eclipse.jetty.server.session.SessionHandler.doScope(SessionHandler.java:193)
	at org.eclipse.jetty.server.handler.ContextHandler.doScope(ContextHandler.java:1020)
	at org.eclipse.jetty.server.handler.ScopedHandler.handle(ScopedHandler.java:135)
	at org.eclipse.jetty.server.handler.ContextHandlerCollection.handle(ContextHandlerCollection.java:255)
	at org.eclipse.jetty.server.handler.HandlerCollection.handle(HandlerCollection.java:154)
	at org.eclipse.jetty.server.handler.HandlerWrapper.handle(HandlerWrapper.java:116)
	at org.eclipse.jetty.server.Server.handle(Server.java:370)
	at org.eclipse.jetty.server.AbstractHttpConnection.handleRequest(AbstractHttpConnection.java:494)
	at org.eclipse.jetty.server.AbstractHttpConnection.headerComplete(AbstractHttpConnection.java:973)
	at org.eclipse.jetty.server.AbstractHttpConnection$RequestHandler.headerComplete(AbstractHttpConnection.java:1035)
	at org.eclipse.jetty.http.HttpParser.parseNext(HttpParser.java:641)
	at org.eclipse.jetty.http.HttpParser.parseAvailable(HttpParser.java:231)
	at org.eclipse.jetty.server.AsyncHttpConnection.handle(AsyncHttpConnection.java:82)
	at org.eclipse.jetty.io.nio.SelectChannelEndPoint.handle(SelectChannelEndPoint.java:696)
	at org.eclipse.jetty.io.nio.SelectChannelEndPoint$1.run(SelectChannelEndPoint.java:53)
	at org.eclipse.jetty.util.thread.QueuedThreadPool.runJob(QueuedThreadPool.java:608)
	at org.eclipse.jetty.util.thread.QueuedThreadPool$3.run(QueuedThreadPool.java:543)
	at java.lang.Thread.run(Thread.java:748)
Powered by Jetty://
