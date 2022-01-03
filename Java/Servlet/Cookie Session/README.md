# Cookie Session

Servlet that demonstrate the use of cookie and session.

- [Files](#files)
  - [web.xml](#webxml)
  - [Main.java](#mainjava)
- [Output](#output)

## Files

### web.xml

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<web-app>

    <servlet>
        <servlet-name>Main</servlet-name>
        <servlet-class>Main</servlet-class>
        <init-param>
            <param-name>count</param-name>
            <param-value>0</param-value>
        </init-param>
    </servlet>
    
    <servlet-mapping>
        <servlet-name>Main</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

</web-app>
```

### Main.java

```Java
public class Main extends HttpServlet {
    private int sessionCount;
    private int cookieCount;
    private static String KEY = "count";

    @Override
    public void init() throws ServletException {
        super.init();
        sessionCount = Integer.parseInt(getServletConfig().getInitParameter(KEY));
    }

    public Cookie getCookieExists(HttpServletRequest req, String key) {
        if (req.getCookies() != null) {
            for (Cookie cookie : req.getCookies()) {
                if (cookie.getName().equals(key)) {
                    return cookie;
                }
            }
        }
        return null;
    }

    protected void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        Cookie cookie = getCookieExists(req, KEY);
        if (cookie != null) {
            cookieCount = Integer.parseInt(cookie.getValue());
        } else {
            Cookie _cookie = new Cookie(KEY, getServletConfig().getInitParameter(KEY));
            cookie = _cookie;
        }
        ++sessionCount;
        ++cookieCount;
        cookie.setValue(String.valueOf(cookieCount));
        res.addCookie(cookie);
        res.setContentType("text/html;charset=UTF-8");
        res.getWriter()
                .write("<h4>Total views in this session</h4> " + sessionCount + "<br><br><br>" + "<h4>Total views</h4>"
                        + cookieCount);
    }
}
```

## Output

![Output](./output.png)
