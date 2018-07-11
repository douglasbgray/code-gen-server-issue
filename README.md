# code-gen-server-issue

Demonstrates issue when generating code that contains variables in the server definition

When running `mvn clean compile`, the swagger code gen tries to validate the server URL in openapi.yaml:

```
servers:
  - url: 'http://localhost:{port}/sample'
    description: 'Development server running on localhost'
    variables:
      port:
        default: '8080'
        description: port number on which the server is running
```
        
It does not apply the variable substituion before doing so, and the usage of `{port}` as a variable causes a warning
 to be logged.
        
```
java.net.MalformedURLException: For input string: "{port}"
	at java.net.URL.<init>(URL.java:627)
	at java.net.URL.<init>(URL.java:490)
	at java.net.URL.<init>(URL.java:439)
	at io.swagger.codegen.utils.URLPathUtil.getServerURL(URLPathUtil.java:30)
	at io.swagger.codegen.DefaultGenerator.configureGeneratorProperties(DefaultGenerator.java:211)
	...
```
        
