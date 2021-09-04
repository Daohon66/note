流程说明：

1. 客户端（浏览器）发送请求到   DispatcherServlet 
2. 接着  DispatcherServlet  更具请求信息调用   HandlerMapping  ，解析请求对应的 Handler 
3. 解析到对应的Handler（也就是Controller）后，开始由HandlerAdapter适配器处理
4. 然后HandlerAdapter会根据Handler来调用真正的处理器来处理请求，并处理相应的业务逻辑
5. 处理器处理完业务逻辑后，返回一个ModelAndView对象，Model是返回的数据对象，View是逻辑上的View
6. .ViewResolverModelAndView对象后会接收到根据逻辑View查找实际的View
7. .DispatcherServlet把返回的Model传给View（视图渲染）
8. 把View返回给请求者（浏览器）