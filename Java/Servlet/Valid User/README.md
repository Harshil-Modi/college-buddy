# Valid User

Servlet for login process. The servlet should accept the user name and password form user and if match then it should display login successful then it should display one page Welcome username and if it is unsuccessful then it should display one page of login unsuccessful.

- [Files](#files)
  - [web.xml](#webxml)
  - [index.html](#indexhtml)
  - [home.html](#homehtml)
  - [401.html](#401html)
  - [Auth.java](#authjava)
- [Output](#output)

## Files

### web.xml

```xml
<web-app>

    <servlet>
        <servlet-name>Auth</servlet-name>
        <servlet-class>Auth</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>Auth</servlet-name>
        <url-pattern>/Auth</url-pattern>
    </servlet-mapping>

    <welcome-file-list>
        <welcome-file>index.html</welcome-file>
    </welcome-file-list>
</web-app>
```

### index.html

```html
<html>

<body>
    <form action="Auth" method="post">
        Username: <input type="text" name="uname" /><br /><br />
        Password: <input type="password" name="pwd" /><br /><br />
        <input type="submit" value="Login" />
    </form>
</body>

</html>
```

### home.html

```html
<html>

<body>
    <h1>Home</h1>
    <p>Logged in successfully</p>
</body>

</html>
```

### 401.html

```html
<html>

<body>
    <h1>401: Unauthorized</h1>
    <p>You are not authorized</p>
</body>

</html>
```

### Auth.java

```Java
public class Auth extends HttpServlet {
    protected void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        String uname = req.getParameter("uname");
        String pwd = req.getParameter("pwd");
        if (uname.equals("admin") && pwd.equals("admin")) {
            res.sendRedirect("home.html");
        } else {
            res.sendRedirect("401.html");
        }
    }
}
```

## Output

### Login With Valid Credentials

![index.html](./images/index.html.png)

![home.html](./images/home.html.png)

### Login With Invalid Credentials

![index.html](./images/index-invalid.html.png)

![401.html](./images/401.html.png)
