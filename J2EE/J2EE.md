# J2EE

J2EE-->Java，Java Enterprise Edition

J2ME-->JavaME，Java Micro Edition

J2SE-->JavaSE，Java Standard Edition

java.lang java.io java.util java.net

JavaEE

javax

Servlet

javax.servlet

Java Application

Java Web Application

Servlet, Filter, Listener, JSP, Tag, Struts, Spring

HttpSession

Cookie

http协议基于请求应答的通信方式

会话实现有困难

session

servlet，ServletConfig，ServletContext

RequestDispathcer，

Filter，FilterConfig，Listener，

setAttribute，getAttribute，removeAttribute，

预处理(之前)

chain.doFilter(request,response)

后处理(之后)

## Servlet 核心概念

### 一、Servlet 核心概念

**Servlet** 是运行在 Web 服务器或应用服务器上的 Java 程序，它充当了 **客户端请求（Request）和服务器响应（Response）之间的中间层** 。Servlet 接收来自客户端（通常是 Web 浏览器）的请求，处理这些请求，然后返回响应。

### Servlet 的主要特点：

1. **服务器端技术** ：在 Web 服务器中执行
2. **平台无关** ：基于 Java，可跨平台运行
3. **高性能** ：通常采用多线程处理请求，效率高
4. **健壮安全** ：受益于 Java 语言的安全特性

---

### 二、Servlet 核心接口和类

1. `javax.servlet.Servlet` 接口（根接口）

所有 Servlet 必须直接或间接实现的根接口。

| 方法                                                      | 说明                  |
| --------------------------------------------------------- | --------------------- |
| `void init(ServletConfig config)`                       | Servlet 初始化时调用  |
| `ServletConfig getServletConfig()`                      | 获取 Servlet 配置信息 |
| `void service(ServletRequest req, ServletResponse res)` | 处理客户端请求        |
| `String getServletInfo()`                               | 返回 Servlet 信息     |
| `void destroy()`                                        | Servlet 销毁时调用    |

### 2. `javax.servlet.GenericServlet` 类（适配器类）

实现了 `Servlet` 接口的抽象类，提供了默认实现。

| 方法                                     | 说明                       |
| ---------------------------------------- | -------------------------- |
| `void init()`                          | 无参的 init 方法，方便重写 |
| `void log(String msg)`                 | 写入日志信息               |
| `String getInitParameter(String name)` | 获取初始化参数             |

### 3. `javax.servlet.http.HttpServlet` 类（最常用）

继承 `GenericServlet`，专门用于处理 HTTP 请求。

| 方法                                                                 | 说明                                | 触发条件               |
| -------------------------------------------------------------------- | ----------------------------------- | ---------------------- |
| `void doGet(HttpServletRequest req, HttpServletResponse resp)`     | 处理 GET 请求                       | 浏览器直接访问、超链接 |
| `void doPost(HttpServletRequest req, HttpServletResponse resp)`    | 处理 POST 请求                      | 表单提交、AJAX         |
| `void doPut(HttpServletRequest req, HttpServletResponse resp)`     | 处理 PUT 请求                       | RESTful API            |
| `void doDelete(HttpServletRequest req, HttpServletResponse resp)`  | 处理 DELETE 请求                    | RESTful API            |
| `void doHead(HttpServletRequest req, HttpServletResponse resp)`    | 处理 HEAD 请求                      | 获取头部信息           |
| `void doOptions(HttpServletRequest req, HttpServletResponse resp)` | 处理 OPTIONS 请求                   | 预检请求               |
| `void doTrace(HttpServletRequest req, HttpServletResponse resp)`   | 处理 TRACE 请求                     | 回显请求               |
| `void service(HttpServletRequest req, HttpServletResponse resp)`   | 根据请求方法分发到对应的 doXxx 方法 |                        |

---

### 三、请求和响应接口

1. `javax.servlet.http.HttpServletRequest` - 请求对象

**获取请求信息：**

**java**

```
String getParameter(String name) // 获取请求参数
String[] getParameterValues(String name) // 获取多个同名参数
Enumeration<String> getParameterNames() // 获取所有参数名
Map<String, String[]> getParameterMap() // 获取参数Map

String getMethod() // 获取请求方法（GET/POST等）
String getRequestURI() // 获取请求URI
String getQueryString() // 获取查询字符串
String getRemoteAddr() // 获取客户端IP
String getHeader(String name) // 获取请求头
Cookie[] getCookies() // 获取所有Cookie
HttpSession getSession() // 获取或创建Session
HttpSession getSession(boolean create) // 获取Session
```

**设置属性（用于请求转发）：**

**java**

```
void setAttribute(String name, Object value)
Object getAttribute(String name)
void removeAttribute(String name)
```

### 2. `javax.servlet.http.HttpServletResponse` - 响应对象

**设置响应：**

**java**

```
void setContentType(String type) // 设置内容类型
void setCharacterEncoding(String charset) // 设置字符编码
PrintWriter getWriter() // 获取字符输出流
ServletOutputStream getOutputStream() // 获取字节输出流

void setStatus(int sc) // 设置状态码
void sendError(int sc) // 发送错误状态
void sendError(int sc, String msg) // 发送错误状态和信息
void sendRedirect(String location) // 重定向

void addCookie(Cookie cookie) // 添加Cookie
void setHeader(String name, String value) // 设置响应头
void addHeader(String name, String value) // 添加响应头
```

---

### 四、其他重要接口和类

1. `javax.servlet.ServletConfig`

每个 Servlet 都有自己的配置信息。

| 方法                                            | 说明                 |
| ----------------------------------------------- | -------------------- |
| `String getServletName()`                     | 获取 Servlet 名称    |
| `ServletContext getServletContext()`          | 获取 ServletContext  |
| `String getInitParameter(String name)`        | 获取初始化参数       |
| `Enumeration<String> getInitParameterNames()` | 获取所有初始化参数名 |

### 2. `javax.servlet.ServletContext`（应用上下文）

代表整个 Web 应用，所有 Servlet 共享。

| 方法                                                    | 说明                 |
| ------------------------------------------------------- | -------------------- |
| `String getContextPath()`                             | 获取应用上下文路径   |
| `String getInitParameter(String name)`                | 获取应用级初始化参数 |
| `void setAttribute(String name, Object value)`        | 设置应用级属性       |
| `Object getAttribute(String name)`                    | 获取应用级属性       |
| `RequestDispatcher getRequestDispatcher(String path)` | 获取请求分发器       |
| `String getRealPath(String path)`                     | 获取资源真实路径     |

### 3. `javax.servlet.RequestDispatcher`（请求分发器）

用于请求转发。

| 方法                                                       | 说明         |
| ---------------------------------------------------------- | ------------ |
| `void forward(ServletRequest req, ServletResponse resp)` | 转发请求     |
| `void include(ServletRequest req, ServletResponse resp)` | 包含其他资源 |

### 4. `javax.servlet.http.HttpSession`（会话）

用于跟踪用户会话状态。

| 方法                                             | 说明             |
| ------------------------------------------------ | ---------------- |
| `String getId()`                               | 获取 Session ID  |
| `long getCreationTime()`                       | 获取创建时间     |
| `long getLastAccessedTime()`                   | 获取最后访问时间 |
| `void setMaxInactiveInterval(int interval)`    | 设置超时时间     |
| `void invalidate()`                            | 使 Session 失效  |
| `void setAttribute(String name, Object value)` | 设置会话属性     |
| `Object getAttribute(String name)`             | 获取会话属性     |
| `void removeAttribute(String name)`            | 移除会话属性     |

### 5. `javax.servlet.Filter`（过滤器）

用于对请求和响应进行预处理和后处理。

| 方法                                                                           | 说明         |
| ------------------------------------------------------------------------------ | ------------ |
| `void init(FilterConfig config)`                                             | 过滤器初始化 |
| `void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain)` | 执行过滤操作 |
| `void destroy()`                                                             | 过滤器销毁   |

---

### 五、Servlet 示例代码

1. 基础 Servlet 示例

**java**

```
@WebServlet("/hello")
public class HelloServlet extends HttpServlet {
  
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) 
            throws ServletException, IOException {
  
        // 设置响应类型和编码
        resp.setContentType("text/html;charset=UTF-8");
  
        // 获取请求参数
        String name = req.getParameter("name");
        if (name == null) {
            name = "Guest";
        }
  
        // 写入响应
        PrintWriter out = resp.getWriter();
        out.println("<!DOCTYPE html>");
        out.println("<html>");
        out.println("<head><title>Hello Servlet</title></head>");
        out.println("<body>");
        out.println("<h1>Hello, " + name + "!</h1>");
        out.println("<p>当前时间: " + new Date() + "</p>");
        out.println("</body>");
        out.println("</html>");
    }
  
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) 
            throws ServletException, IOException {
        doGet(req, resp); // 通常POST也调用doGet处理
    }
}
```

### 2. 用户登录示例

**java**

```
@WebServlet("/login")
public class LoginServlet extends HttpServlet {
  
    // 模拟用户数据库
    private Map<String, String> users = new HashMap<>();
  
    @Override
    public void init() throws ServletException {
        users.put("admin", "admin123");
        users.put("user", "user123");
    }
  
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) 
            throws ServletException, IOException {
  
        String username = req.getParameter("username");
        String password = req.getParameter("password");
  
        if (isValidUser(username, password)) {
            // 登录成功，创建Session
            HttpSession session = req.getSession();
            session.setAttribute("username", username);
            session.setAttribute("loginTime", new Date());
  
            // 重定向到欢迎页面
            resp.sendRedirect("welcome.jsp");
        } else {
            // 登录失败，返回错误信息
            req.setAttribute("error", "用户名或密码错误");
            req.getRequestDispatcher("login.jsp").forward(req, resp);
        }
    }
  
    private boolean isValidUser(String username, String password) {
        return users.containsKey(username) && users.get(username).equals(password);
    }
}
```

### 3. 文件下载 Servlet

**java**

```
@WebServlet("/download")
public class DownloadServlet extends HttpServlet {
  
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) 
            throws ServletException, IOException {
  
        String filename = req.getParameter("file");
        if (filename == null) {
            resp.sendError(HttpServletResponse.SC_BAD_REQUEST, "文件名不能为空");
            return;
        }
  
        // 获取文件真实路径
        String filePath = getServletContext().getRealPath("/downloads/" + filename);
        File file = new File(filePath);
  
        if (!file.exists()) {
            resp.sendError(HttpServletResponse.SC_NOT_FOUND, "文件不存在");
            return;
        }
  
        // 设置响应头
        resp.setContentType("application/octet-stream");
        resp.setHeader("Content-Disposition", 
                      "attachment; filename=\"" + filename + "\"");
        resp.setContentLength((int) file.length());
  
        // 输出文件内容
        try (FileInputStream in = new FileInputStream(file);
             OutputStream out = resp.getOutputStream()) {
  
            byte[] buffer = new byte[4096];
            int bytesRead;
            while ((bytesRead = in.read(buffer)) != -1) {
                out.write(buffer, 0, bytesRead);
            }
        }
    }
}
```

### 4. 过滤器示例：字符编码过滤器

**java**

```
@WebFilter("/*")
public class CharacterEncodingFilter implements Filter {
  
    private String encoding;
  
    @Override
    public void init(FilterConfig config) throws ServletException {
        encoding = config.getInitParameter("encoding");
        if (encoding == null) {
            encoding = "UTF-8";
        }
    }
  
    @Override
    public void doFilter(ServletRequest req, ServletResponse resp, 
                        FilterChain chain) throws IOException, ServletException {
  
        // 设置请求和响应编码
        req.setCharacterEncoding(encoding);
        resp.setCharacterEncoding(encoding);
        resp.setContentType("text/html;charset=" + encoding);
  
        // 继续处理请求
        chain.doFilter(req, resp);
    }
  
    @Override
    public void destroy() {
        // 清理资源
    }
}
```

---

### 六、Servlet 配置方式

1. 注解配置（Servlet 3.0+）

**java**

```
@WebServlet(
    name = "MyServlet",
    urlPatterns = {"/path1", "/path2"},
    initParams = {
        @WebInitParam(name = "param1", value = "value1"),
        @WebInitParam(name = "param2", value = "value2")
    },
    loadOnStartup = 1
)
public class MyServlet extends HttpServlet {
    // ...
}
```

### 2. web.xml 配置（传统方式）

**xml**

```
<servlet>
    <servlet-name>MyServlet</servlet-name>
    <servlet-class>com.example.MyServlet</servlet-class>
    <init-param>
        <param-name>param1</param-name>
        <param-value>value1</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>

<servlet-mapping>
    <servlet-name>MyServlet</servlet-name>
    <url-pattern>/path1</url-pattern>
    <url-pattern>/path2</url-pattern>
</servlet-mapping>
```

---

### 七、Servlet 生命周期

1. **加载和实例化** ：容器加载 Servlet 类并创建实例
2. **初始化** ：调用 `init()` 方法，只执行一次
3. **处理请求** ：调用 `service()` 方法，每次请求都会执行
4. **销毁** ：调用 `destroy()` 方法，应用关闭时执行

### 总结

| 组件类型           | 主要用途       | 核心接口/类                         |
| ------------------ | -------------- | ----------------------------------- |
| **Servlet**  | 处理业务逻辑   | `HttpServlet`, `GenericServlet` |
| **Filter**   | 预处理和后处理 | `Filter`                          |
| **Listener** | 监听事件       | 各种 `XxxListener` 接口           |
| **Request**  | 封装请求信息   | `HttpServletRequest`              |
| **Response** | 封装响应信息   | `HttpServletResponse`             |
| **Session**  | 维护会话状态   | `HttpSession`                     |
| **Context**  | 应用全局信息   | `ServletContext`                  |

## Filter

### 一、Filter 核心概念

**Filter** 是 Java Web 中的一种组件，它可以在请求到达 Servlet 之前和响应发送给客户端之前，对请求和响应进行 **预处理和后处理** 。Filter 形成了一个 **过滤器链** ，多个 Filter 可以按顺序对请求进行处理。

#### Filter 的主要特点：

1. **拦截请求和响应** ：在 Servlet 前后执行
2. **可链式调用** ：多个 Filter 可以形成处理链
3. **可配置性** ：可以配置拦截特定的 URL 模式
4. **可重用性** ：一个 Filter 可以应用于多个 Servlet

#### Filter 的典型应用场景：

* 身份认证和授权检查
* 日志记录和审计
* 数据压缩和加密
* 字符编码设置
* XSS 防护和 SQL 注入防护
* 性能监控和统计

---

### 二、Filter 核心接口和类

#### 1. `javax.servlet.Filter` 接口（核心接口）

所有过滤器必须实现的接口。

| 方法                                                                           | 说明                                     |
| ------------------------------------------------------------------------------ | ---------------------------------------- |
| `void init(FilterConfig config)`                                             | 过滤器初始化时调用，只执行一次           |
| `void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain)` | 执行过滤操作的核心方法，每次请求都会调用 |
| `void destroy()`                                                             | 过滤器销毁时调用，用于清理资源           |

#### 2. `javax.servlet.FilterConfig` 接口

用于获取过滤器的配置信息。

| 方法                                            | 说明                 |
| ----------------------------------------------- | -------------------- |
| `String getFilterName()`                      | 获取过滤器的名称     |
| `ServletContext getServletContext()`          | 获取 ServletContext  |
| `String getInitParameter(String name)`        | 获取初始化参数       |
| `Enumeration<String> getInitParameterNames()` | 获取所有初始化参数名 |

#### 3. `javax.servlet.FilterChain` 接口

代表过滤器链，用于调用链中的下一个过滤器或目标资源。

| 方法                                                        | 说明                                                             |
| ----------------------------------------------------------- | ---------------------------------------------------------------- |
| `void doFilter(ServletRequest req, ServletResponse resp)` | 调用链中的下一个过滤器，如果当前是最后一个过滤器，则调用目标资源 |

#### 4. `javax.servlet.GenericFilter` 类（抽象类）

实现了 `Filter` 接口的抽象类，提供了基本实现。

#### 5. `javax.servlet.http.HttpFilter` 类（抽象类，Servlet 4.0+）

专门用于 HTTP 过滤的抽象类，提供了更方便的方法。

| 方法                                                                                   | 说明                      |
| -------------------------------------------------------------------------------------- | ------------------------- |
| `void doFilter(HttpServletRequest req, HttpServletResponse resp, FilterChain chain)` | HTTP 专用的 doFilter 方法 |

---

### 三、Filter 配置方式

#### 1. 注解配置（Servlet 3.0+）

**java**

```
@WebFilter(
    filterName = "MyFilter",
    urlPatterns = {"/*"},
    initParams = {
        @WebInitParam(name = "param1", value = "value1"),
        @WebInitParam(name = "param2", value = "value2")
    },
    dispatcherTypes = {DispatcherType.REQUEST, DispatcherType.FORWARD}
)
public class MyFilter implements Filter {
    // ...
}
```

#### 2. web.xml 配置（传统方式）

**xml**

```
<filter>
    <filter-name>MyFilter</filter-name>
    <filter-class>com.example.MyFilter</filter-class>
    <init-param>
        <param-name>param1</param-name>
        <param-value>value1</param-value>
    </init-param>
</filter>

<filter-mapping>
    <filter-name>MyFilter</filter-name>
    <url-pattern>/*</url-pattern>
    <dispatcher>REQUEST</dispatcher>
    <dispatcher>FORWARD</dispatcher>
</filter-mapping>
```

#### 3. 使用 `@WebFilter` 的多种配置方式

**java**

```
// 方式1：通过 urlPatterns
@WebFilter(urlPatterns = "/admin/*")

// 方式2：通过 value（简写）
@WebFilter("/api/*")

// 方式3：通过 servletNames
@WebFilter(servletNames = {"MyServlet1", "MyServlet2"})

// 方式4：多个URL模式
@WebFilter(urlPatterns = {"/user/*", "/api/*", "*.jsp"})
```

---

### 四、Filter 示例代码

#### 1. 基础字符编码过滤器

**java**

```
@WebFilter("/*")
public class CharacterEncodingFilter implements Filter {
  
    private String encoding = "UTF-8";
  
    @Override
    public void init(FilterConfig config) throws ServletException {
        String encodingParam = config.getInitParameter("encoding");
        if (encodingParam != null) {
            encoding = encodingParam;
        }
        System.out.println("CharacterEncodingFilter 初始化，编码设置为: " + encoding);
    }
  
    @Override
    public void doFilter(ServletRequest req, ServletResponse resp, 
                        FilterChain chain) throws IOException, ServletException {
    
        // 设置请求和响应编码
        req.setCharacterEncoding(encoding);
        resp.setCharacterEncoding(encoding);
        resp.setContentType("text/html;charset=" + encoding);
    
        System.out.println("CharacterEncodingFilter: 设置编码为 " + encoding);
    
        // 继续处理请求
        chain.doFilter(req, resp);
    
        System.out.println("CharacterEncodingFilter: 响应处理完成");
    }
  
    @Override
    public void destroy() {
        System.out.println("CharacterEncodingFilter 销毁");
    }
}
```

#### 2. 身份认证过滤器

**java**

```
@WebFilter("/admin/*")
public class AuthenticationFilter implements Filter {
  
    @Override
    public void doFilter(ServletRequest req, ServletResponse resp, 
                        FilterChain chain) throws IOException, ServletException {
    
        HttpServletRequest httpReq = (HttpServletRequest) req;
        HttpServletResponse httpResp = (HttpServletResponse) resp;
    
        HttpSession session = httpReq.getSession(false);
    
        // 检查用户是否登录
        if (session == null || session.getAttribute("user") == null) {
            // 未登录，重定向到登录页面
            httpResp.sendRedirect(httpReq.getContextPath() + "/login.jsp");
            return;
        }
    
        // 检查用户权限
        User user = (User) session.getAttribute("user");
        if (!"admin".equals(user.getRole())) {
            // 权限不足
            httpResp.sendError(HttpServletResponse.SC_FORBIDDEN, "权限不足");
            return;
        }
    
        System.out.println("用户 " + user.getUsername() + " 访问管理页面");
    
        // 继续处理请求
        chain.doFilter(req, resp);
    }
  
    @Override
    public void init(FilterConfig config) throws ServletException {
        System.out.println("AuthenticationFilter 初始化");
    }
  
    @Override
    public void destroy() {
        System.out.println("AuthenticationFilter 销毁");
    }
}
```

#### 3. 日志记录过滤器

**java**

```
@WebFilter("/*")
public class LoggingFilter implements Filter {
  
    @Override
    public void doFilter(ServletRequest req, ServletResponse resp, 
                        FilterChain chain) throws IOException, ServletException {
    
        HttpServletRequest httpReq = (HttpServletRequest) req;
    
        long startTime = System.currentTimeMillis();
        String clientIP = httpReq.getRemoteAddr();
        String requestURI = httpReq.getRequestURI();
        String method = httpReq.getMethod();
    
        System.out.println(String.format(
            "请求开始: %s %s from %s at %tT", 
            method, requestURI, clientIP, new Date()
        ));
    
        // 继续处理请求
        chain.doFilter(req, resp);
    
        long endTime = System.currentTimeMillis();
        long duration = endTime - startTime;
    
        System.out.println(String.format(
            "请求完成: %s %s 耗时 %dms", 
            method, requestURI, duration
        ));
    }
  
    @Override
    public void init(FilterConfig config) throws ServletException {
        System.out.println("LoggingFilter 初始化");
    }
  
    @Override
    public void destroy() {
        System.out.println("LoggingFilter 销毁");
    }
}
```

#### 4. XSS 防护过滤器

**java**

```
@WebFilter("/*")
public class XSSFilter implements Filter {
  
    @Override
    public void doFilter(ServletRequest req, ServletResponse resp, 
                        FilterChain chain) throws IOException, ServletException {
    
        // 包装请求对象，重写获取参数的方法
        XSSRequestWrapper wrappedRequest = new XSSRequestWrapper((HttpServletRequest) req);
    
        // 继续处理请求（使用包装后的请求对象）
        chain.doFilter(wrappedRequest, resp);
    }
  
    // 自定义请求包装器
    private static class XSSRequestWrapper extends HttpServletRequestWrapper {
    
        public XSSRequestWrapper(HttpServletRequest request) {
            super(request);
        }
    
        @Override
        public String getParameter(String name) {
            String value = super.getParameter(name);
            return escapeHtml(value);
        }
    
        @Override
        public String[] getParameterValues(String name) {
            String[] values = super.getParameterValues(name);
            if (values == null) return null;
        
            String[] escapedValues = new String[values.length];
            for (int i = 0; i < values.length; i++) {
                escapedValues[i] = escapeHtml(values[i]);
            }
            return escapedValues;
        }
    
        @Override
        public String getHeader(String name) {
            String value = super.getHeader(name);
            return escapeHtml(value);
        }
    
        private String escapeHtml(String input) {
            if (input == null) return null;
        
            return input.replaceAll("&", "&")
                       .replaceAll("<", "<")
                       .replaceAll(">", ">")
                       .replaceAll("\"", """)
                       .replaceAll("'", "'")
                       .replaceAll("/", "/");
        }
    }
}
```

#### 5. 响应压缩过滤器

**java**

```
@WebFilter(urlPatterns = {"*.html", "*.css", "*.js", "*.json", "*.xml"})
public class CompressionFilter implements Filter {
  
    @Override
    public void doFilter(ServletRequest req, ServletResponse resp, 
                        FilterChain chain) throws IOException, ServletException {
    
        HttpServletRequest httpReq = (HttpServletRequest) req;
        HttpServletResponse httpResp = (HttpServletResponse) resp;
    
        String acceptEncoding = httpReq.getHeader("Accept-Encoding");
    
        if (acceptEncoding != null && acceptEncoding.contains("gzip")) {
            // 客户端支持gzip压缩
            CompressionResponseWrapper wrappedResponse = 
                new CompressionResponseWrapper(httpResp);
            wrappedResponse.setHeader("Content-Encoding", "gzip");
        
            chain.doFilter(req, wrappedResponse);
        
            // 完成压缩并输出
            wrappedResponse.finish();
        } else {
            // 客户端不支持压缩，直接处理
            chain.doFilter(req, resp);
        }
    }
  
    // 自定义响应包装器
    private static class CompressionResponseWrapper extends HttpServletResponseWrapper {
        private GZIPOutputStream gzipStream;
        private ServletOutputStream outputStream;
        private PrintWriter writer;
    
        public CompressionResponseWrapper(HttpServletResponse response) {
            super(response);
        }
    
        @Override
        public ServletOutputStream getOutputStream() throws IOException {
            if (writer != null) {
                throw new IllegalStateException("getWriter() has already been called");
            }
        
            if (outputStream == null) {
                outputStream = getResponse().getOutputStream();
                gzipStream = new GZIPOutputStream(outputStream);
            }
            return new FilterServletOutputStream(gzipStream);
        }
    
        @Override
        public PrintWriter getWriter() throws IOException {
            if (outputStream != null) {
                throw new IllegalStateException("getOutputStream() has already been called");
            }
        
            if (writer == null) {
                gzipStream = new GZIPOutputStream(getResponse().getOutputStream());
                writer = new PrintWriter(new OutputStreamWriter(gzipStream, getResponse().getCharacterEncoding()));
            }
            return writer;
        }
    
        public void finish() throws IOException {
            if (writer != null) {
                writer.close();
            } else if (gzipStream != null) {
                gzipStream.close();
            }
        }
    }
  
    // 自定义Servlet输出流
    private static class FilterServletOutputStream extends ServletOutputStream {
        private final OutputStream stream;
    
        public FilterServletOutputStream(OutputStream stream) {
            this.stream = stream;
        }
    
        @Override
        public void write(int b) throws IOException {
            stream.write(b);
        }
    
        @Override
        public void write(byte[] b) throws IOException {
            stream.write(b);
        }
    
        @Override
        public void write(byte[] b, int off, int len) throws IOException {
            stream.write(b, off, len);
        }
    
        @Override
        public boolean isReady() {
            return true;
        }
    
        @Override
        public void setWriteListener(WriteListener listener) {
            // 不需要实现
        }
    }
}
```

#### 6. 跨域请求过滤器（CORS）

**java**

```
@WebFilter("/api/*")
public class CorsFilter implements Filter {
  
    @Override
    public void doFilter(ServletRequest req, ServletResponse resp, 
                        FilterChain chain) throws IOException, ServletException {
    
        HttpServletRequest httpReq = (HttpServletRequest) req;
        HttpServletResponse httpResp = (HttpServletResponse) resp;
    
        // 设置CORS头部
        httpResp.setHeader("Access-Control-Allow-Origin", "*");
        httpResp.setHeader("Access-Control-Allow-Methods", "GET, POST, PUT, DELETE, OPTIONS");
        httpResp.setHeader("Access-Control-Allow-Headers", "Content-Type, Authorization");
        httpResp.setHeader("Access-Control-Max-Age", "3600");
    
        // 处理预检请求（OPTIONS）
        if ("OPTIONS".equalsIgnoreCase(httpReq.getMethod())) {
            httpResp.setStatus(HttpServletResponse.SC_OK);
            return;
        }
    
        // 继续处理请求
        chain.doFilter(req, resp);
    }
}
```

#### 7. 多个过滤器链式调用示例

**java**

```
// 第一个过滤器：日志记录
@WebFilter(urlPatterns = "/*", order = 1)
public class LogFilter implements Filter {
    public void doFilter(ServletRequest req, ServletResponse resp, 
                        FilterChain chain) throws IOException, ServletException {
        System.out.println("LogFilter: 请求开始");
        chain.doFilter(req, resp);
        System.out.println("LogFilter: 请求结束");
    }
}

// 第二个过滤器：认证检查
@WebFilter(urlPatterns = "/secure/*", order = 2)
public class AuthFilter implements Filter {
    public void doFilter(ServletRequest req, ServletResponse resp, 
                        FilterChain chain) throws IOException, ServletException {
        System.out.println("AuthFilter: 检查认证");
        chain.doFilter(req, resp);
    }
}

// 第三个过滤器：数据压缩
@WebFilter(urlPatterns = "/*", order = 3)
public class CompressFilter implements Filter {
    public void doFilter(ServletRequest req, ServletResponse resp, 
                        FilterChain chain) throws IOException, ServletException {
        System.out.println("CompressFilter: 压缩响应");
        chain.doFilter(req, resp);
    }
}
```

---

### 五、Filter 生命周期

1. **初始化阶段** ：Web 应用启动时，调用 `init()` 方法
2. **过滤阶段** ：每次匹配的请求都会调用 `doFilter()` 方法
3. **销毁阶段** ：Web 应用关闭时，调用 `destroy()` 方法

---

### 六、Filter 执行顺序

#### 1. 执行顺序规则：

* `web.xml` 中定义的 Filter 按配置顺序执行
* 注解配置的 Filter 按类名字母顺序执行
* 可以通过 `@WebFilter(order = n)` 指定顺序（数字小的先执行）

#### 2. 完整的请求处理流程：

**text**

```
客户端请求 → Filter1 → Filter2 → ... → FilterN → Servlet → FilterN → ... → Filter2 → Filter1 → 客户端响应
```

---

### 七、DispatcherType 类型

Filter 可以配置处理不同类型的请求：

| DispatcherType | 说明                         |
| -------------- | ---------------------------- |
| `REQUEST`    | 直接来自客户端的请求（默认） |
| `FORWARD`    | 通过请求转发过来的请求       |
| `INCLUDE`    | 通过包含过来的请求           |
| `ERROR`      | 错误处理请求                 |
| `ASYNC`      | 异步请求                     |

**配置示例：**

**java**

```
@WebFilter(
    urlPatterns = "/*",
    dispatcherTypes = {DispatcherType.REQUEST, DispatcherType.FORWARD}
)
```

---

### 总结

| 特性               | 说明                                               |
| ------------------ | -------------------------------------------------- |
| **核心接口** | `Filter`, `FilterConfig`, `FilterChain`      |
| **配置方式** | 注解 `@WebFilter` 或 `web.xml`                 |
| **执行顺序** | 可通过 order 属性或配置顺序控制                    |
| **请求类型** | 可通过 dispatcherTypes 配置处理不同类型的请求      |
| **典型应用** | 编码设置、身份认证、日志记录、安全防护、性能优化等 |

## Listener概念

ServletContextListener
	default void contextInitialized(ServletContextEvent sce)
	default void contextDestroyed(ServletContextEvent sce)

HttpSessionListener
	default void sessionCreated(HttpSessionEvent se)
	default void sessionDestroyed(HttpSessionEvent se)

ServletRequestListener
	default void requestInitialized(ServletRequestEvent sre)
	default void requestDestroyed(ServletRequestEvent sre)

ServletContextAttributeListener
	default void attributeAdded(ServletContextAttributeEvent event)
	default void attributeReplaced(ServletContextAttributeEvent event)
	default void attributeRemoved(ServletContextAttributeEvent event)

HttpSessionAttributeListener
	default void attributeAdded(HttpSessionBindingEvent event)
	default void attributeReplaced(HttpSessionBindingEvent event)
	default void attributeRemoved(HttpSessionBindingEvent event)

ServletRequestAttributeListener
	default void attributeAdded(ServletRequestAttributeEvent srae)
	default void attributeReplaced(ServletRequestAttributeEvent srae)
	default void attributeRemoved(ServletRequestAttributeEvent srae)

HttpSessionBindingListener
	default void valueBound(HttpSessionBindingEvent event)
	default void valueUnbound(HttpSessionBindingEvent event)

HttpSessionActivationListener
	default void sessionDidActivate(HttpSessionEvent se)
	default void sessionWillPassivate(HttpSessionEvent se)

ServletContextEvent
	public ServletContext getServletContext()
ServletContextAttributeEvent
	public String getName()
	public Object getValue()

ServletRequestEvent
	public ServletRequest getServletRequest()
	public ServletContext getServletContext()
ServletRequestAttributeEvent
	public String getName()
	public Object getValue()

HttpSessionEvent
	public HttpSession getSession()
HttpSessionBindingEvent
	public HttpSession getSession()
	public String getName()
	public Object getValue()

### 一、Listener 概念

在 Java Web 中，Listener（监听器）是一种基于**观察者模式**的组件。它的核心功能是 **监听 Web 应用中特定事件的发生** （如：应用的启动与关闭、用户会话的创建与销毁、请求的到来与结束、对象属性的增删改等），并在这些事件发生时，自动触发预先定义好的回调方法，执行相应的业务逻辑。

你可以把它想象成一个 **潜伏在系统里的“耳朵”** ，时刻监听着它感兴趣的事情，一旦事情发生，它就立刻做出反应。

**主要作用：**

1. **初始化与清理资源** ：在应用启动时初始化数据库连接池、加载缓存数据；在应用关闭时优雅地释放资源。
2. **统计与监控** ：统计在线人数、网站访问量、请求耗时等。
3. **实现业务逻辑** ：在用户登录时（Session 创建），为其初始化一些数据；在属性被修改时进行日志记录或权限校验。
4. **在分布式环境中管理 Session** ：与 `HttpSessionActivationListener` 配合，实现 Session 的钝化与活化。

---

### 二、监听器分类详解

监听器主要分为三类： **生命周期监听器** 、**属性监听器** 和  **特殊用途监听器** 。

#### 1. 生命周期监听器 (Lifecycle Listeners)

这类监听器监听 `ServletContext`, `HttpSession`, `ServletRequest` 三大核心对象的创建和销毁事件。

| 监听器接口                           | 监听事件               | 触发时机                                                                                                        |
| ------------------------------------ | ---------------------- | --------------------------------------------------------------------------------------------------------------- |
| **`ServletContextListener`** | `contextInitialized` | **Web 应用启动**时触发。在所有 Filter 和 Servlet 初始化之前执行。是初始化全局资源的 **最佳位置** 。 |
|                                      | `contextDestroyed`   | **Web 应用关闭**时触发。用于安全地关闭和清理资源（如数据库连接池）。                                      |
| **`HttpSessionListener`**    | `sessionCreated`     | 每当一个**新的用户会话 (HttpSession) 被创建**时触发。可用于统计在线用户数。                               |
|                                      | `sessionDestroyed`   | 当一个用户会话**被销毁** （超时、调用 `invalidate()`、应用关闭）时触发。                                |
| **`ServletRequestListener`** | `requestInitialized` | **每次收到一个客户端请求**时，在请求被 Servlet 处理**之前**触发。                                   |
|                                      | `requestDestroyed`   | **每次请求处理完毕** ，生成响应之后，在请求对象被销毁**之前**触发。可用于计算请求处理耗时。         |

#### 2. 属性监听器 (Attribute Listeners)

这类监听器监听上述三大对象（`ServletContext`, `HttpSession`, `ServletRequest`）中属性的添加、替换和移除事件。

| 监听器接口                                    | 监听事件              | 触发时机                                                                |
| --------------------------------------------- | --------------------- | ----------------------------------------------------------------------- |
| **`ServletContextAttributeListener`** | `attributeAdded`    | 向 `ServletContext` (Application 域) 中**添加**了一个新属性时。 |
|                                               | `attributeReplaced` | `ServletContext` 中的某个属性被**新值替换**时。                 |
|                                               | `attributeRemoved`  | `ServletContext` 中的某个属性被**移除**时。                     |
| **`HttpSessionAttributeListener`**    | `attributeAdded`    | 向 `HttpSession` (Session 域) 中**添加**了一个新属性时。        |
|                                               | `attributeReplaced` | `HttpSession` 中的某个属性被**新值替换**时。                    |
|                                               | `attributeRemoved`  | `HttpSession` 中的某个属性被**移除**时。                        |
| **`ServletRequestAttributeListener`** | `attributeAdded`    | 向 `ServletRequest` (Request 域) 中**添加**了一个新属性时。     |
|                                               | `attributeReplaced` | `ServletRequest` 中的某个属性被**新值替换**时。                 |
|                                               | `attributeRemoved`  | `ServletRequest` 中的某个属性被**移除**时。                     |

#### 3. 特殊用途监听器 (Special-purpose Listeners)

这类监听器功能更具体，通常需要由被监听的对象自己实现。

| 监听器接口                                  | 监听事件                 | 触发时机与说明                                                                                                                                                            |
| ------------------------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`HttpSessionBindingListener`**    | `valueBound`           | 当实现了此接口的对象，被**绑定** （即通过 `session.setAttribute("key", thisObject)`）到某个 Session 时，由该对象自己触发。 **无需在 `web.xml` 中注册** 。 |
|                                             | `valueUnbound`         | 当实现了此接口的对象，从 Session 中**被解除绑定** （即通过 `session.removeAttribute("key")` 或 Session 失效）时，由该对象自己触发。                               |
| **`HttpSessionActivationListener`** | `sessionWillPassivate` | 当 Session 即将被**钝化** （序列化到硬盘）到硬盘前，通知 Session 中的所有**实现了此接口**的对象。用于分布式环境或服务器内存管理。                             |
|                                             | `sessionDidActivate`   | 当 Session 被**活化** （从硬盘反序列化到内存）后，通知 Session 中的所有**实现了此接口**的对象。                                                               |

---

### 三、事件对象 (Event Objects)

每个监听器方法都会接收一个事件对象，这个对象包含了与事件相关的所有信息。

| 事件类                                     | 核心方法                | 说明                                                                                                              |
| ------------------------------------------ | ----------------------- | ----------------------------------------------------------------------------------------------------------------- |
| **`ServletContextEvent`**          | `getServletContext()` | 获取产生事件的 `ServletContext` 应用对象。                                                                      |
| **`ServletContextAttributeEvent`** | `getName()`           | 获取发生变化的属性的**名字** 。                                                                             |
|                                            | `getValue()`          | 获取发生变化的属性的**值** 。``- `added`: 新值``- `removed`: 旧值``- `replaced`: **被替换的旧值** |
| **`ServletRequestEvent`**          | `getServletRequest()` | 获取产生事件的 `ServletRequest` 请求对象。                                                                      |
|                                            | `getServletContext()` | 获取当前的 `ServletContext`。                                                                                   |
| **`ServletRequestAttributeEvent`** | `getName()`           | 获取发生变化的属性的**名字** 。                                                                             |
|                                            | `getValue()`          | 获取发生变化的属性的**值** （规则同上）。                                                                   |
| **`HttpSessionEvent`**             | `getSession()`        | 获取产生事件的 `HttpSession` 会话对象。                                                                         |
| **`HttpSessionBindingEvent`**      | `getSession()`        | 获取绑定或解除绑定发生的 `HttpSession`。                                                                        |
|                                            | `getName()`           | 获取绑定或解除绑定的属性的**名字** （即 `setAttribute` 时用的 key）。                                     |
|                                            | `getValue()`          | 获取绑定或解除绑定的属性的**值** （即 `setAttribute` 时传入的 value，也就是对象自己）。                   |

---

### 四、如何使用？

1. **创建监听器类** ：创建一个 Java 类，实现上面任意一个或多个监听器接口，并重写你关心的方法。
2. **注册监听器** （`HttpSessionBindingListener` 和 `HttpSessionActivationListener` 除外）：

* **注解方式** （Servlet 3.0+）：在类上添加 `@WebListener` 注解。
* **配置文件方式** （`web.xml`）：
  **xml**

  ``<listener>          <listener-class>com.yourpackage.YourListenerClass</listener-class>      </listener>``

### 五、简单示例：统计在线人数

**java**

```
import javax.servlet.annotation.WebListener;
import javax.servlet.http.HttpSessionEvent;
import javax.servlet.http.HttpSessionListener;

@WebListener // 注册监听器
public class OnlineUserCounter implements HttpSessionListener {

    private static int onlineCount = 0;

    @Override
    public void sessionCreated(HttpSessionEvent se) {
        // 会话创建，在线人数+1
        onlineCount++;
        System.out.println("新用户上线！当前在线人数: " + onlineCount);
        // 可以将 onlineCount 存入 ServletContext 供页面显示
        se.getSession().getServletContext().setAttribute("onlineCount", onlineCount);
    }

    @Override
    public void sessionDestroyed(HttpSessionEvent se) {
        // 会话销毁，在线人数-1
        onlineCount--;
        System.out.println("用户下线！当前在线人数: " + onlineCount);
        se.getSession().getServletContext().setAttribute("onlineCount", onlineCount);
    }

    public static int getOnlineCount() {
        return onlineCount;
    }
}
```

### 总结

| 监听对象                 | 生命周期监听器             | 属性监听器                          | 特殊监听器                                                    |
| ------------------------ | -------------------------- | ----------------------------------- | ------------------------------------------------------------- |
| **应用 (Context)** | `ServletContextListener` | `ServletContextAttributeListener` | -                                                             |
| **会话 (Session)** | `HttpSessionListener`    | `HttpSessionAttributeListener`    | `HttpSessionBindingListener``HttpSessionActivationListener` |
| **请求 (Request)** | `ServletRequestListener` | `ServletRequestAttributeListener` | -                                                             |
