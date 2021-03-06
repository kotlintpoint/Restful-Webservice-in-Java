# Restful-Webservice-in-Java

## Restful Webservice Maven Dependency
 

    <!-- https://mvnrepository.com/artifact/javax.ws.rs/javax.ws.rs-api -->
    <dependency>
        <groupId>javax.ws.rs</groupId>
        <artifactId>javax.ws.rs-api</artifactId>
        <version>2.1.1</version>
    </dependency>
      <!-- https://mvnrepository.com/artifact/org.glassfish.jersey.containers/jersey-container-servlet -->
    <dependency>
        <groupId>org.glassfish.jersey.containers</groupId>
        <artifactId>jersey-container-servlet</artifactId>
        <version>2.30.1</version>
    </dependency>
      <dependency>
        <groupId>org.glassfish.jersey.inject</groupId>
        <artifactId>jersey-hk2</artifactId>
        <version>2.28</version>
    </dependency>

## web.xml configuration

 

    <servlet>  
        <servlet-name>Jersey REST Service</servlet-name>  
        <servlet-class>org.glassfish.jersey.servlet.ServletContainer</servlet-class>  
        <init-param>  
            <param-name>jersey.config.server.provider.packages</param-name>  
            <param-value>webservice.service</param-value>  
        </init-param>  
        <load-on-startup>1</load-on-startup>  
      </servlet>  
      <servlet-mapping>
          <servlet-name>Jersey REST Service</servlet-name>
          <url-pattern>/rest/*</url-pattern>
    </servlet-mapping>

## Summary of JAX-RS Annotations

| Annotation  |  Description |
| ------------ | ------------ |
| @Path  |  The @Path annotation’s value is a relative URI path indicating where the Java class will be hosted: for example, /helloworld. You can also embed variables in the URIs to make a URI path template. For example, you could ask for the name of a user and pass it to the application as a variable in the URI: /helloworld/{username}. |
| @GET  |  The @GET annotation is a request method designator and corresponds to the similarly named HTTP method. The Java method annotated with this request method designator will process HTTP GET requests. The behavior of a resource is determined by the HTTP method to which the resource is responding. |
| @POST  | The @POST annotation is a request method designator and corresponds to the similarly named HTTP method. The Java method annotated with this request method designator will process HTTP POST requests. The behavior of a resource is determined by the HTTP method to which the resource is responding.  |
| @PUT  |  The @PUT annotation is a request method designator and corresponds to the similarly named HTTP method. The Java method annotated with this request method designator will process HTTP PUT requests. The behavior of a resource is determined by the HTTP method to which the resource is responding. |
| @DELETE  |  The @DELETE annotation is a request method designator and corresponds to the similarly named HTTP method. The Java method annotated with this request method designator will process HTTP DELETE requests. The behavior of a resource is determined by the HTTP method to which the resource is responding. |
| @HEAD  | The @HEAD annotation is a request method designator and corresponds to the similarly named HTTP method. The Java method annotated with this request method designator will process HTTP HEAD requests. The behavior of a resource is determined by the HTTP method to which the resource is responding.  |
| @PathParam  | The @PathParam annotation is a type of parameter that you can extract for use in your resource class. URI path parameters are extracted from the request URI, and the parameter names correspond to the URI path template variable names specified in the @Path class-level annotation.  |
| @QueryParam  |  The @QueryParam annotation is a type of parameter that you can extract for use in your resource class. Query parameters are extracted from the request URI query parameters. |
| @Consumes  | The @Consumes annotation is used to specify the MIME media types of representations a resource can consume that were sent by the client.  |
| @Produces  |  The @Produces annotation is used to specify the MIME media types of representations a resource can produce and send back to the client: for example, "text/plain". |
| @Provider  | The @Provider annotation is used for anything that is of interest to the JAX-RS runtime, such as MessageBodyReader and MessageBodyWriter. For HTTP requests, the MessageBodyReader is used to map an HTTP request entity body to method parameters. On the response side, a return value is mapped to an HTTP response entity body by using a MessageBodyWriter. If the application needs to supply additional metadata, such as HTTP headers or a different status code, a method can return a Response that wraps the entity and that can be built using Response.ResponseBuilder.  |

### MyService.java

```java
@Path("/users")
public class MyService {
	
	private UserDao dao=new UserDao();
		
	@GET
	@Produces(MediaType.APPLICATION_JSON)
	public String getUser() {
		List<User> users = dao.getUsers();
		Gson gson=new Gson();
		String usersJson=gson.toJson(users);
		return usersJson;
	}
	
	@POST
	@Produces(MediaType.TEXT_HTML)
	@Consumes(MediaType.APPLICATION_JSON)
	public String saveUser(String userJson) {
		System.out.println(userJson);
		Gson gson=new Gson();
		User user=gson.fromJson(userJson, User.class);
		String result=dao.registerUser(user);
		return result;
	}
	
	@PUT
	@Path("/{id}")
	@Produces("text/plain")	
	public String updateUser(@PathParam("id") String id) {
		return "update user";
	}
	
	@DELETE
	@Path("/{id}")
	@Produces("text/plain")
	public String deleteUser(@PathParam("id") String id) {
		return "delete User";
	}
}
```


## Restful Webservice Consumer Maven Dependency

```markdown
 <!-- https://mvnrepository.com/artifact/com.google.code.gson/gson -->
<dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.8.6</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.jboss.resteasy/resteasy-client -->
<dependency>
    <groupId>org.jboss.resteasy</groupId>
    <artifactId>resteasy-client</artifactId>
    <version>4.5.1.Final</version>
</dependency>
```

## Post User Data To Web Service

```java
Client client=ClientBuilder.newClient();
WebTarget target=client.target("http://localhost:8080/webservice/rest/users");
User user=new User();
user.setFullName(request.getParameter("fullName"));
user.setEmail(request.getParameter("email"));
user.setPassword(request.getParameter("password"));
Gson gson =new Gson();
String userJson=gson.toJson(user);
Response resp=target.request().post(Entity.json(userJson));
String result=resp.readEntity(String.class);
response.getWriter().append(result);
```

## Get Data From Web Service

```java
Client client=ClientBuilder.newClient();
WebTarget target=client.target("http://localhost:8080/webservice/rest/users");
Response resp=target.request().get();
String result=resp.readEntity(String.class);
Gson gson=new Gson();
Type type = new TypeToken<List<User>>() {}.getType();
List<User> users = gson.fromJson(result, type);
String str="<table border='1px'>";		
for(User user: users) {
		str+="<tr>"
				+"<td>"+user.getId()+"</td>"
				+"<td>"+user.getFullName()+"</td>"
				+"<td>"+user.getEmail()+"</td>"
				+"</tr>";
}
str+="</table>";
response.setContentType("text/html");
response.getWriter().append(str);
```
