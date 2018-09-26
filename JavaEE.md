* Servlet
    * **HttpServlet** 考试重点
* ServletConfig
* ServletRequest
    * **HttpServletRequest（强制转换得到）** 考试重点
        * Cookie 与 Session 相关
            * getCookies() 得到一个 *Cookie 对象数组*
            * getSession() 得到与请求相关的 Session。*如果 Session 不存在就创建一个*
    * 多组件共享数据
        * setAttribute()
        * getAttribute()
    * 获取 Get 或 Post 请求的数据
        * getParameter(String name) 得到 name 对应的值
        * getParameters(String name) 得到 name 对应的值的数组
        * getParameterMap() 返回一个 Map，里面是请求参数的键值对
        * getParameterNames() 返回一个枚举对象
        * getParameterValues() 返回一个 String 数组
    * setCharacterEncoding()
* ServletResponse
    * **HttpServletResponse（强制转换得到）** 考试重点
        * 不要关闭流，谁打开的谁关闭
        * setContentLength 只接受 int 类型
        * **sendRedirect()** 考试重点
* **RequestDispatcher** 考试重点
    * 派发请求到其他 Servlet
    * forward
    * include
    * 不能两个组件各自处理一部分内容
* HttpSession
    * invalidate()
* URLEncoder
* URLDecoder
---
## Web 应用程序

### 文件夹结构：

```
/first/                     // 名称，随便起
    WEB-INF/
        classes/            // class 文件
        lib/                // jar 文件
        tags/               //
        web.xml             // 配置信息与描述性信息（重点）,受控 XML 文件
    META-INF/
        content.xml         // 声明上下文地址
```
---
### xml
```xml
<?xml version="" encoding="" ?>
    <root color="red">
        <childNode></childNode>
        <childNode></childNode>
        <childNode></childNode>
    </root>
```
属性可以用 XML 属性和子节点表示，官方推荐使用子节点

#### Well-format
* 物理格式良好：遵守 XML 语法规则
* 逻辑格式良好：符合应用程序定义的格式
* DTD(Document Type Definition)
```xml
<!--一个 Students 中至少有一个 Student-->
<!ELEMENT students(student+)>
<!--一个 Student 中至少有 id, name, gender, age 属性-->
<!ELEMENT student(id, name, gender, age)>
```
* XML Schema

#### 命名空间
先声明再使用
```xml
<student 
    xmlns="https://www.google.com/"
    xmlns:p="https://soulike.tech/">

    <p:childNode p:attr="value"></p:childNode>
    <!--以上等价于-->
    <https://soulike.tech:childNode
        https://soulike.tech/:attr="value">

    </https://soulike.tech:childNode>

    <!--默认命名空间-->
    <childNode></childNode>
    <!--以上等价于-->
    <https://www.google.com/:childNode></https://www.google.com/:childNode>

</student>
```

#### 受控 XML 文件
```xml
<?xml version="" encoding="" ?>
    <root 
        schemaLocation="[namespace] [xsd file URL]"
        xmlns:x="namespacex"
        xmlns:y="namespacey">
        <x:a></x:a>
        <y:a></y:a>
    </root>
```

---

### 会话
#### 实现方式
- Cookies
- Url rewriting
    - SessionID 附加在 URL 中
- Hidden field
    - 在表单中隐藏一个域包含 Session 信息
#### Session 过期时间设置
通过在 web.xml 中设置。单位为**分钟**
```xml
<session-config>
    <session-timeout>30</session-timeout>
</session-config>
```
设置 30 分钟后 Session 过期

---
### Filter
预处理请求，后处理响应

用户 => Filter1 => ... => FilterN => Servlet => Filter1 => ... => FilterN => 用户

也可以就地处理请求（拦截）

- `destroy()`
- `doFilter(ServletRequest request, ServletResponse response, FilterChain chain)`
- `init(FilterConfig filterConfig)`

一般用 HttpFilter

#### FilterChain
- `doFilter(ServletRequest request, ServletResponse response)` 下一个过滤器的 doFilter() 方法
---
### Filter 在 web.xml
```xml
<filter>...</filter>
<filter-mapping>
    <filter-name>...</filter-name>
    <url-pattern>...</url-pattern>
    <dispatcher>...</dispather>
</filter-mapping>
```
需要注意的
- `<dispather>` 的作用

---

### Listener
各种 Event（考试重点）
- `ServletContextEvent` 与 `ServletContextListener`
    - `contextInitialized(ServletContextEvent sce)` 应用程序启动后*第一个调用*的函数，可以用来做各种初始化
- `HttpSessionEvent`
- `HttpSessionBindingEvent`
    - 调用 `HttpSession.setAttribute()` 时触发
- `HttpSessionAttributeListener`
    - `attributeReplaced(HttpSessionBindingEvent event)`
        - **`event.getValue()` 返回值为修改前的值**

---

### Web 应用程序部署方法（考）
1. 复制文件夹到 tomcat 安装目录下的 webapps 文件夹
2. 在 server.xml 的 `<Host>` 标签下添加 `<Context path="" docBase="" />`
    - path 是 URL 路径，如果是根可留空
    - docBase 是 Servlet 文件夹路径
```xml
<Host name="localhost" appbase="webapps" ……>
    <Context path="/hello" docBase="/home/soulike/hello" />
</Host>
```
3. 在 `META-INF` 文件夹下的 context.xml 中添加 `<Context path="/abc" />`

### Web 应用程序打包方法
```bash
jar cvf hello.war -C /home/soulike/hello .
```

---

### JSP (Java Server Page)
- `<%@ %>` 内嵌 JSP 指令
    - page 指令：指定页面属性
        - import
        - contentType: 废话，一般不写
        - pageEncoding: 指定页面编码
        - session: 默认为 true，可不写
        - isErrorPage: 默认为 false，可不写
        - errorPage
    - include 指令：宏替换，代码复用，相当于把指定页面代码直接放在当前文件中
```jsp
<%@ page import="java.util.*"
         contentType="text/html;charset=utf-8"
         pageEncoding="utf-8"
         session="true"
         isErrorPage="false"
         errorPage="/error.jsp"
%>
```
```jsp
<%@ include file="copyright.jsp" %>
```
- `<%= %>` 任何合法 Java 表达式
    - 避免声明变量，因为变量会变为成员变量，而 Servlet 是单实例的，*线程不安全*
- `<%! %>` 声明标签，可放入变量定义
- `<% %>` 任何合法的 Java 语句