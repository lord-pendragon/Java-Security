# Java-Security
For Java Security


* [Custom Error Page](#Custom-Error)
* [Restrict File to Access](#forbidden)
* [Broken Session Managment](#Session-fix)






## Custom-Error
How to implement custom error page in Java website? Create web.xml and insert below code

```
<error-page>  
  <error-code>404</error-code>  
  <location>/error.jsp</location>  
  </error-page>

## error.jsp is location of custom error page
```

## forbidden
Create web.xml and insert below code
```

<error-code>403</error-code>  
  <location>forbid.jsp</location>  
  </error-page> 
  ## forbid.jsp is location of custom error page

```
 ## Session-fix

using session

```
request.getSession(false).invalidate();
					HttpSession session = request.getSession(true);
					
					session.setAttribute("uname", uname);
					session.setAttribute("pass", pass);
					
					response.sendRedirect("welcome.jsp");
```

on logout 
```
HttpSession session = request.getSession();
		 session.removeAttribute("uname");
		 session.invalidate();
		 response.sendRedirect("login.jsp");
```






# Spring Boot  + Spring security

* [Adding security headers](#Secure-headers)
* [Custom error page](#error-page)
* [Broken session Managment](#Session)
* [Cross site Scripting](#Xss-Fix)


## Secure-headers

 To implement security headers in application created in spring boot framework we have to add spring security dependencies  in pom.xml
 
 ```
 <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency> 

```
 
 ```
Next, create a class that extends the WebSecurityConfigureAdapter. Add the annotation @EnableWebSecurity to the class to tell spring that this class is a spring security configuration.

Override the two overloaded methods configure(HttpSecurity) and configure(AuthenticationManagerBuilder).

The configure(HttpSecurity) defines the mapping of secured URLs or paths that will determine if the user can access specific pages.
 
 
 @EnableWebSecurity
public class Security  extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
    	
    }
}

 ```

## error-page

```
go to application.properties and set 
server.error.whitelabel.enabled=false  and create a custom error page 


```

![alt text](https://github.com/effortlessdevsec/Java-Security/blob/main/error.png)



## Xss-Fix


add   org.owasp.esapi dependeny in pom.xml file 
```
<dependency>
    <groupId>org.owasp.esapi</groupId>
    <artifactId>esapi</artifactId>
    <version>2.2.2.0</version>
</dependency>

```

```
use the StringEscapeUtils 
${StringEscapeUtils.escapeHtml(obj.name)}

#### Refernce : https://www.java67.com/2012/10/how-to-escape-html-special-characters-JSP-Java-Example.html
```


## Session

```
	@Override
	protected void configure(HttpSecurity http) throws Exception {
		
		http
			
			.authorizeRequests().antMatchers("/login").permitAll()
			.anyRequest().authenticated()
			.and()
			.formLogin()
			.loginPage("/login").permitAll()
			.and()
			.logout().invalidateHttpSession(true)
			.clearAuthentication(true)
			.logoutRequestMatcher(new AntPathRequestMatcher("/logout"))
			.logoutSuccessUrl("/logout-success").permitAll();
		
	}
```


