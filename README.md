# Spring-Exception  
异常处理  
  
## 一、概念  

[图片](img/1.png)
 

## 二、异常的处理在Spring MVC 中有四种方式进行处理  
 
　　使用默认的DefaultHandlerExceptionResolver 异常处理类  
　　编程式异常处理  
　　自定义HandlerExceptionResolver（全局的处理）  
　　使用注解的方式@ExceptionHandler/@ControllerAdvice。。。  
 

1、在Spring MVC中如果你抛出异常，自己在后面的方法中没有处理，那么在Spring容器中会有一个DefaultHandlerExceptionResolver异常来处理。  

 

异常的处理集中。    
 
不好做到精确的控制。   

2、编程式异常处理：  

　编程式异常处理的好处是：能精确的捕获到异常，并及时的做出对应的响应；  

　缺点：如果方法变多了，那么这种写法不便于维护；捕获的异常变多了，代码的量会逐渐增加，重复的代码会出现越来越多；  




				package com.wgc.jsonp.controller;

				import com.wgc.jsonp.entity.TestUser;
				import com.wgc.jsonp.exception.DBException;
				import com.wgc.jsonp.exception.DataVlidateException;
				import com.wgc.jsonp.service.TestUserService;
				import org.springframework.beans.factory.annotation.Autowired;
				import org.springframework.stereotype.Controller;
				import org.springframework.web.bind.annotation.RequestMapping;
				import org.springframework.web.bind.annotation.ResponseBody;

				@Controller
				public class TestUserController {
					
					@Autowired
					private TestUserService service;

					@RequestMapping("/add")
					@ResponseBody /*去掉这个注解就是返回页面*/
					public String add(TestUser testUser) {
						try {
							service.addUser(testUser);
						} catch (DBException e) {
							return "返回到指定的页面";
						} catch (DataVlidateException e) {
						   return "返回到原本添加的页面";
						} catch (Exception e) {
							return "未知的异常，返回到指定的页面";
						}
						return "返回到添加成功后的页面";
					}
				}

 

3、自定义HandlerExceptionResolver（全局的处理）  