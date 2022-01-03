# Shopping Cart

Implement the shopping cart for users for the online shopping. Apply the concept of session.

- [Files](#files)
  - [web.xml](#webxml)
  - [index.html](#indexhtml)
  - [bill.html](#billhtml)
  - [shop.html](#shophtml)
  - [Auth.java](#authjava)
  - [Bill.java](#billjava)
  - [Cart.java](#cartjava)
- [Output](#output)

## Files

### web.xml

```xml
<web-app>
    <servlet>
        <servlet-name>Auth</servlet-name>
        <servlet-class>Auth</servlet-class>
    </servlet>

    <servlet>
        <servlet-name>Bill</servlet-name>
        <servlet-class>Bill</servlet-class>
    </servlet>

    <servlet>
        <servlet-name>Cart</servlet-name>
        <servlet-class>Cart</servlet-class>
    </servlet>

    <servlet-mapping>
        <servlet-name>Auth</servlet-name>
        <url-pattern>/Auth</url-pattern>
    </servlet-mapping>

    <servlet-mapping>
        <servlet-name>Bill</servlet-name>
        <url-pattern>/Bill</url-pattern>
    </servlet-mapping>

    <servlet-mapping>
        <servlet-name>Cart</servlet-name>
        <url-pattern>/Cart</url-pattern>
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
    <h1>Login</h1>
    <form action="Auth" method="post">
        Username: <input type="email" name="email" /><br /><br />
        Password: <input type="password" name="pwd" /><br /><br />
        <input type="submit" value="Login" />
    </form>
</body>

</html>
```

### bill.html

```html
<html>

<body>
    <h2>Hello 'name'</h2>
    <p>'items'</p>
    <p>'total'</p>
</body>

</html>
```

### shop.html

```html
<html>

<body>
    <h1>Shop</h1>
    <form action="Cart" method="post">
        <table>
            <tr>
                <th>Item</th>
                <th>Price</th>
                <th>Add to cart</th>
            </tr>
            <tr>
                <td>Mouse</td>
                <td>$4.99</td>
                <td><input type="checkbox" name="chkMouse" value="Mouse,4.99"></td>
            </tr>
            <tr>
                <td>Keyboard</td>
                <td>$7.99</td>
                <td><input type="checkbox" name="chkKeyboard" value="Keyboard,7.99"></td>
            </tr>
            <tr>
                <td>Laptop</td>
                <td>$599.99</td>
                <td><input type="checkbox" name="chkLaptop" value="Laptop,599.99"></td>
            </tr>
            <tr colspan=2>
                <td>
                    <input type="submit" value="Proceed">
                </td>
            </tr>
        </table>
    </form>
</body>

</html>
```

### Auth.java

```Java
public class Auth extends HttpServlet {

    protected void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        try {
            String email = req.getParameter("email");
            String pwd = req.getParameter("pwd");
            Class.forName("com.mysql.cj.jdbc.Driver");

            Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/scet", "root", "");
            PreparedStatement ps = con.prepareStatement("SELECT * FROM users WHERE email=? AND password=?");
            ps.setString(1, email);
            ps.setString(2, pwd);
            ResultSet rs = ps.executeQuery();

            if (rs.next()) {
                HttpSession session = req.getSession();
                session.setAttribute("name", rs.getString("name"));
                res.sendRedirect("shop.html");
            } else {
                res.setContentType("text/html");
                res.getWriter().write("<h3>The email id is not registed!</h3>");
            }
        } catch (Exception e) {
            res.getWriter().write(e.toString());

            e.printStackTrace();
        }
    }
}
```

### Bill.java

```Java
public class Bill extends HttpServlet {
    private void sendHTMLFile(HttpServletResponse res, String pathName, HashMap<String, String> replace)
            throws Exception {
        try {
            Path filePath = new File(this.getClass().getResource(pathName).getPath()).toPath();
            String content = Files.readString(filePath);

            if (replace != null) {
                Object[] keys = replace.keySet().toArray();
                Object[] vals = replace.values().toArray();
                for (int i = 0; i < replace.size(); ++i) {
                    content = content.replaceAll(keys[i].toString(), vals[i].toString());
                }
            }
            res.setContentType("text/html");
            res.getWriter().write(content);
        } catch (Exception e) {
            e.printStackTrace(res.getWriter());

        }
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        doGet(req, res);
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        try {
            HttpSession session = req.getSession();
            String name = session.getAttribute("name").toString();
            String items = session.getAttribute("items").toString();
            String total = session.getAttribute("total").toString();

            HashMap<String, String> replace = new HashMap<String, String>();
            replace.put("'name'", name);
            if (items == null) {
                items = "Your cart is empty!";
            } else {
                items = "Items in your cart: " + items;
            }
            total = "Total amount to pay: &dollar;" + total;
            replace.put("'items'", items);
            replace.put("'total'", total);
            sendHTMLFile(res, "../../bill.html", replace);
        } catch (Exception e) {
            e.printStackTrace(res.getWriter());
        }
    }
}
```

### Cart.java

```Java
public class Cart extends HttpServlet {
    private static String appendItemToCart(String cartItems, String item) {
        if (item != null) {
            String[] itemAndPrice = item.split(",");
            String itemName = itemAndPrice[0], price = itemAndPrice[1];
            if (cartItems == null) {
                cartItems = "";
            }
            cartItems += itemName + " (&dollar;" + price + "), ";
        }
        return cartItems;
    }

    private static float getCartTotal(String cartItems) {
        float total = 0;
        // The following regex will replace all characters in the string 'cartItems' with "" (blank character) except [0-9], ','(comma) and '.'(dot)
        cartItems = cartItems.replaceAll("[^0-9.,]", "");
        for (String price : cartItems.split(",")) {
            total += Float.parseFloat(price);
        }
        return total;
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        HttpSession session = req.getSession();
        String chkMouse = req.getParameter("chkMouse");
        String chkKeyboard = req.getParameter("chkKeyboard");
        String chkLaptop = req.getParameter("chkLaptop");
        String cartItems = null;
        cartItems = appendItemToCart(cartItems, chkMouse);
        cartItems = appendItemToCart(cartItems, chkKeyboard);
        cartItems = appendItemToCart(cartItems, chkLaptop);
        if (cartItems != null) {
            // The following regex will remove last comma(,) from the string 'cartItems'
            cartItems = cartItems.trim().replaceAll("\\,$", "");
        }
        float total = getCartTotal(cartItems);
        session.setAttribute("items", cartItems);
        session.setAttribute("total", total);
        res.sendRedirect("Bill");
    }
}
```

## Output

### Login With Valid Credentials

![index.html](./images/login.png)

![shop (cart.html)](./images/shop.png)

![bill.html](./images/bill.png)

### Login With Invalid Credentials

![Entering false credentials](./images/login-invalid.png)

![Invalid credentials error](./images/error.png)
