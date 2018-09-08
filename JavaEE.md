* Servlet
    * HttpServlet
* ServletConfig
* ServletRequest
    * HttpServletRequest（强制转换得到）
* ServletResponse
    * HttpServletResponse（强制转换得到）
        * 不要关闭流，谁打开的谁关闭
        * setContentLength 只接受 int 类型
        * sendRedirect

Web 应用程序

文件夹结构：

```
    /first                      // 名称，随便起
        WEB-INF/
            classes/            // class 文件
            lib/                // jar 文件
            web.xml             // 非必须，配置信息与描述性信息（重点）
```