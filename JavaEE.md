* Servlet
    * **HttpServlet** 考试重点
* ServletConfig
* ServletRequest
    * **HttpServletRequest（强制转换得到）** 考试重点
    * 多组件共享数据
        * setAttribute()
        * getAttribute()
    * *getParameter()*
    * *getParameterMap()*
    * *getParameterNames()*
    * *getParameterValues()*
* ServletResponse
    * **HttpServletResponse（强制转换得到）** 考试重点
        * 不要关闭流，谁打开的谁关闭
        * setContentLength 只接受 int 类型
        * **sendRedirect()** 考试重点
        * addCookie()
        * getCookies()
        * getSession()
* **RequestDispatcher** 考试重点
    * 派发请求到其他 Servlet
    * forward
    * include
    * 不能两个组件各自处理一部分内容
* HttpSession
    * invalidate()

## Web 应用程序

### 文件夹结构：

```
/first                      // 名称，随便起
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