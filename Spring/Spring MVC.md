---
aliases:
  - SpringMVC
Created-Date: 2024-05-13T15:54:00
Last-Modified-Date: 2024-05-13T15:54:00
---
詳細原理看 [[kaitao-springMVC.pdf]]。
## Spring MVC 的依賴
## Spring MVC 的配置
- 配置 DispatcherServlet 給 tomcat，因此需在 web.xml 配置。
- 將 HandlerMapping、HandlerAdapter、Handler 加入至 IOC Container，供 DispatcherServlet 調用。
- 將 Handler 配置到 HandlerMapping 中，因為 Handler 本質上只是個 Method，需要將它配置成為一個 Handler，這步指的就是在 method 上加入 @RequestMapping 使此 method 註冊到 HandlerMapping 成為一個 Handler。
### 配置範例
#### 配置 DispatcherServlet 給 tomcat。
##### AbstractAnnotationConfigDispatcherServletInitializer
Spring MVC 提供了此 Class 來取代 web.xml 的配置，並且此 Class 還會自動加載 ioc container，我們就不需要去手動創建 ioc container 了。
```java
/**  
* description:  
* 這個 AbstractAnnotationConfigDispatcherServletInitializer 是 Spring MVC 提供的  
* 目的就是用於取代 web.xml，若是沒有這個 Class 則需要去 web.xml 配置 DispatcherServlet 等等訊息  
* 但有了這個 Class 後，它會被 web 項目加載，會初始化 ioc container，並且設置 DispatcherServlet 的地址  
* 也因為此 Class 會初始化 ioc container，因此我們就不需要去手動創建並且把 xml 配置檔丟給 container 了  
*/  
public class SpringMVCInit extends AbstractAnnotationConfigDispatcherServletInitializer {  
// 用來創建 service、mapper 層的 ioc container@Override  
protected Class<?>[] getRootConfigClasses() {  
return new Class[0];  
}  
  
// 用來創建 controller 層的 ioc container@Override  
protected Class<?>[] getServletConfigClasses() {  
// 將配置類傳遞進去  
return new Class[]{MvcConfig.class};  
}  
  
// 配置 DispatcherServlet 的攔截訪問地址  
@Override  
protected String[] getServletMappings() {  
return new String[]{"/"};  
}  
}
```
##### web.xml
若是沒有上面介紹的 Class，那就需要在 web.xml 去把 Spring MVC 的 Servlet 加入到 tomcat。
![[Pasted image 20240514105600.png]]
#### 將 HandlerMapping、HandlerAdapter、Handler 加入至 IOC Container
```java
/**  
* description:  
* 1. 將 controller 配置到 ioc container  
* 2. 將 HandlerMapping、HandlerAdapter 配置到 ioc container  
*/  
  
@Configuration  
@ComponentScan("com.atguigu.controller") // 掃描 package controller 將此 controller 配置到 ioc containerpublic class MvcConfig {  
  
@Bean  
public HandlerMapping getHandlerMapping() {  
return new RequestMappingHandlerMapping();  
}  
  
@Bean  
public HandlerAdapter getHandlerAdapter() {  
return new RequestMappingHandlerAdapter();  
}  
  
}
```
#### 將 Handler 配置到 HandlerMapping
```java
/**  
* ClassName: Controller  
* Package: com.atguigu  
*/  
@org.springframework.stereotype.Controller  
public class Controller {  
@RequestMapping("springmvc/hello") // 透過 @RequestMapping 將 此 method 變成一個 handler 註冊到 handlerMapping@ResponseBody // 直接 return 資料給前端，不走視圖解析  
public String hello() {  
System.out.println("hello mvc");  
return "heelo mvc";  
}  
}
```


### AbstractAnnotationConfigDispatcherServletInitializer
此 Class 最底層實際是一個 `WebApplicationInitializer` 的 interface，而這個 interface 提供了一個 `onStartup()` Method，這個方法會在 Web 項目啟動時自動執行，因此 Spring MVC 就是在此方法裡去實現了創建 IOC Contatiner 、註冊 DispatcherServlet 等等事情，若有興趣就去查看此 Class 的 `onStartup()` 即可。