- @RequestMapping
	- 指定 Handler，可用在 **方法** 和 **类** 上
	- 参数
		- name
		  collapsed:: true
			- 为 Handler 取一个名称
		- value（alias path）， method
		  collapsed:: true
			- value：请求路径，可以指定为 `URI Template` 模式
			- method：请求的类型，GET，PUT，POST，DELETE 等
		- consumes， produces
			- consumes：指定处理请求的提交内容（content-type）类型，如 application/json，text/html
				- ```java
				  // 只接收 content-type=application/json 的请求
				  // 需要和 @RequestBody 一起使用，否则无法注入
				  @RequestMapping(value = "/handle07", 
				                  consumes = "application/json")
				  public User handle07(@RequestBody User user) {
				    user.setName("张三丰");
				    return user;
				  }
				  ```
			- produces：指定返回的内容类型，仅当请求头中的类型（Accept）中包含该指定类型才返回
				- ```java
				  @RequestMapping(value = "/pets/{petId}", 
				                  method = RequestMethod.GET, 
				                  produces = "application/json")
				  // 方法仅处理request请求中Accept头中包含了"application/json"的请求，
				  // 同时暗示了返回的内容类型为application/json
				  @ResponseBody
				  public Pet getPet(@PathVariable String petId, Model model) {    
				      // implementation omitted
				  }
				  ```
		- params， headers
			- params：必须包含某些参数值才处理
				- ```java
				  @RequestMapping(value = "/pets/{petId}", 
				                  method = RequestMethod.GET, 
				                  params = "myParam=myValue")
				  // 仅处理请求中包含了名为“myParam”，值为“myValue”的请求
				  public void findPet(@PathVariable String ownerId, 
				                      @PathVariable String petId, 
				                      Model model) {    
				    // implementation omitted
				  }
				  ```
			- headers：必须包含某些 header 值才处理
				- ```java
				  @RequestMapping(value = "/pets", 
				                  method = RequestMethod.GET, 
				                  headers="Referer=http://www.ifeng.com/")
				  // 仅处理request的header中包含了指定 “Refer” 请求头
				  // 并且对应值为 “http://www.ifeng.com/” 的请求
				  public void findPet(@PathVariable String ownerId, 
				                      @PathVariable String petId, 
				                      Model model) {    
				    // implementation omitted
				  }
				  ```
	- url 指定可以是普通 url，也可以是 URI Template（包括`变量`和`正则`）
	  collapsed:: true
		- 正则表达式 `{name: 正则}`
			- ```java
			  
			  @RequestMapping("/spring-web/{symbolicName:[a-z-]+}-{version:\d\.\d\.\d}.{extension:\.[a-z]}")
			  public void handle(@PathVariable String version, @PathVariable String extension) {    
			    // ...
			  }
			  ```
		- 变量 `{name}`
			- ```java
			  
			  @RequestMapping(value="/owners/{ownerId}", method=RequestMethod.GET)
			  public String findOwner(@PathVariable String ownerId, Model model) {
			    Owner owner = ownerService.findOwner(ownerId);  
			    model.addAttribute("owner", owner);  
			    return "displayOwner"; 
			  }
			  ```
- @GetMapping
- @PostMapping
- @DeleteMapping
- @PutMapping
- @RequestParam
- @RequestBody
  collapsed:: true
  id:: 6187c818-ca36-4222-a52f-be386bf07804
	- 解析请求的 json 参数
	- 要求
	  collapsed:: true
		- 请求体为一个 json 字符串
		- 请求的 `contextType="application/json;charset=utf-8"`
- @ResponseBody
  id:: 6187c838-d407-4aab-930c-74068078276d
  collapsed:: true
	- 将返回的数据转换成 json 字符串
	- 要求，这个要求对后台没有影响，对前台解析有影响
	  collapsed:: true
		- 请求的返回格式 `dataType="json"`
		- 如果格式 dataType="xml"，那么将不会回调 success 方法，因为前台尝试使用 xml 去解析，会解析错误，而调用 error 方法，而对于请求来说，是调用成功的，是 `200`
- @RestController 
  collapsed:: true
	- @Controller + @ResponseBody
- @ExceptionHandler
  id:: 6187f3c8-c7ae-4f4f-a381-8e0cc88b3b25
	- 异常处理器
	  collapsed:: true
		- collapsed:: true
		  ```java
		  // SpringMVC的异常处理机制（异常处理器）
		  @ExceptionHandler(ArithmeticException.class)
		  public void handleException(ArithmeticException exception,HttpServletResponse response) {
		    // 异常处理逻辑
		    try {
		      response.getWriter().write(exception.getMessage());
		    } catch (IOException e) {
		      e.printStackTrace();
		    }
		  }
		  ```
	- > 全局，在 ((6187f46b-c19f-4c4e-b4f5-2d38d1a1075c)) 注解类中配置
	- 局部，在某个 Controller 类中配置，单个 Controller 生效
- @ControllerAdvice
  id:: 6187f46b-c19f-4c4e-b4f5-2d38d1a1075c
	- 一个增强的 Controller。使用这个 Controller ，可以实现三个方面的功能：
		- 全局异常处理
			- ((6187f3c8-c7ae-4f4f-a381-8e0cc88b3b25))
		- 全局数据绑定
			- ((6187f8e6-0ec3-4344-bec3-c2d1869a2a0d))
		- 全局数据预处理
			- ((618d3166-b429-4c5b-92ec-ee9046e80053))
	- collapsed:: true
	  ```java
	  // 可以让我们优雅的捕获所有Controller对象handler方法抛出的异常
	  @ControllerAdvice
	  public class GlobalExceptionResolver {
	    @ExceptionHandler(ArithmeticException.class)
	    public ModelAndView handleException(ArithmeticException exception, HttpServletResponse response) {
	      ModelAndView modelAndView = new ModelAndView();
	      modelAndView.addObject("msg",exception.getMessage());
	      modelAndView.setViewName("error");
	      return modelAndView;
	    }
	  }
	  ```
- @PathVariable
  collapsed:: true
	- 获取路径中的参数, 需要配合特殊的路径声明 `/{param}/{param2}`
	- collapsed:: true
	  ```java
	  @RequestMapping(value = "/handle/{id}",method = {RequestMethod.GET})
	  public ModelAndView handleGet(@PathVariable("id") Integer id) {
	    Date date = new Date();
	    ModelAndView modelAndView = new ModelAndView();
	    modelAndView.addObject("date",date);
	    modelAndView.setViewName("success");
	    return modelAndView;
	  }
	  ```
- @ModelAttribute
  id:: 6187f8e6-0ec3-4344-bec3-c2d1869a2a0d
	- 可以用在 **方法** 上和 **方法的参数** 上
	  collapsed:: true
	- collapsed:: true
	  > 在 Model 中添加和获取数据，配合 ((6187f46b-c19f-4c4e-b4f5-2d38d1a1075c)) 进行全局数据绑定
	- 用在方法上，添加数据
	  collapsed:: true
		- 用在 HandlerAdaptor 上
			- 也就是 @ModelAttribute 和 @RequestMapping 一起使用
			- @ModelAttribute 会把返回值作为 Model 中的数据，从而导致本来返回的 String 会作为视图名，但是现在作为数据了，也就是没有视图名了，就会 ((6187c41a-1723-4b7e-9277-8b9f7888051c)) ，而导致 `404`
			- 但是返回值是 ModelAndView 不会有影响😂
			- ```java
			  // 会回复 404
			  @ModelAttribute
			  @RequestMapping("/handle02")
			  public String handle02() {
			    return "success";
			  }
			  ```
		- 用在普通方法上
			- 在执行当前 Controller 中的 HandlerAdaptor 之前会执行当前 Controller 中的所有 @ModelAttribute 标注的方法
			- 可以完成请求的数据初始化的工作
			- 下面两种方式结果是一样的
			- 向 Model 中添加数据， 方法一
			- ```java
			  
			  // 返回值作为 model 的数据
			  // 注解参数作为 model 的键，默认名称为类型首字母小写，如这边默认就是 string
			  // 等同于 model.addAttribute("attributeName", "abc")
			  @ModelAttribute("attributeName")
			  // 这边的 abc 会自动注入
			  public String addAccount(@RequestParam String abc) {
			    return abc;
			  }
			  
			  @RequestMapping(value = "/helloWorld")
			  public String helloWorld() {
			    return "helloWorld";
			  }
			  ```
			- 向 Model 中添加数据，方法二
			- ```java
			  @ModelAttribute
			  // 这边的 abc 和 model 会自动注入
			  public void addAccount(@RequestParam String abc, Model model) {
			    model.addAttribute("attributeName", "abc");
			  }
			  
			  @RequestMapping(value = "/helloWorld")
			  public String helloWorld() {
			    return "helloWorld";
			  }
			  ```
	- 用在方法的参数上， 获取数据
	  collapsed:: true
		- 获取 Model 中的数据，尝试从 `session` 中获取数据，配合重定向工具  [`RedirectAttributes`](((6187f5b9-a0e8-46af-85a6-673ced30b14f))) 解决重定向带参问题
		- 在这个例子里，@ModelAttribute("user") User user注释方法参数，参数user的值来源于addAccount()方法中的model属性。此时如果方法体没有标注@SessionAttributes("user")，那么scope为request，如果标注了，那么scope为session
		- collapsed:: true
		  ```java
		  @ModelAttribute("user")
		  public User addAccount() {
		    return new User("jz","123");
		  }
		  
		  @RequestMapping(value = "/helloWorld")
		  public String helloWorld(@ModelAttribute("user") User user) {
		    user.setUserName("jizhou");
		    return "helloWorld";
		  }
		  ```
- @InitBinder
  id:: 618d3166-b429-4c5b-92ec-ee9046e80053
	- > 配合 ((6187f46b-c19f-4c4e-b4f5-2d38d1a1075c)) 进行全局数据预处理配置
	- 用在含有 @Controller 类中的 **方法** 上，作用范围为当前 Controller
	- 执行和 @ModelAttribute 类似，都是在 Handler 执行前执行当前 Controller 中的所有 InitBinder
	- 作用
	  collapsed:: true
		- 类型转换
		- 参数绑定
			- ```html
			  <form action="/testBean" method="post">
			    name: <input type="text" name="u.name"> <br>
			    age: <input type="text" name="u.age"> <br>
			    name: <input type="text" name="s.name"> <br>
			    age: <input type="text" name="s.age"> <br>
			    <input type="submit">
			  </form>
			  ```
			- ```java
			  @InitBinder("user")
			  public void init1(WebDataBinder binder) {
			    binder.setFieldDefaultPrefix("u.");
			  }
			  
			  @InitBinder("stu")
			  public void init2(WebDataBinder binder) {
			    binder.setFieldDefaultPrefix("s.");
			  }
			  
			  @RequestMapping("/testBean")
			  public ModelAndView testBean(User user, @ModelAttribute("stu") Student stu) {
			    System.out.println(stu);
			    System.out.println(user);
			    String viewName = "success";
			    ModelAndView modelAndView = new ModelAndView(viewName);
			    modelAndView.addObject("user", user);
			    modelAndView.addObject("student", stu);
			    return modelAndView;
			  }
			  ```
	- 例子
		- 将值为空字符串的参数转为 null
			- ```java
			  @ControllerAdvice
			  public class GlobalControllerAdiviceController {
			    //WebDataBinder是用来绑定请求参数到指定的属性编辑器，可以继承WebBindingInitializer
			    //来实现一个全部controller共享的dataBiner Java代码
			    @InitBinder
			    public void dataBind(WebDataBinder binder) {
			      ///注册
			      binder.registerCustomEditor(String.class, new StringTrimmerEditor(true));
			    }
			  }
			  ```
			- 以下来处理 post json 报文
				- ```java
				  @Bean
				  public Jackson2ObjectMapperBuilderCustomizer jackson2ObjectMapperBuilderCustomizer() {
				    return new Jackson2ObjectMapperBuilderCustomizer() {
				      @Override
				      public void customize(Jackson2ObjectMapperBuilder jacksonObjectMapperBuilder) {
				        jacksonObjectMapperBuilder
				          .deserializerByType(String.class, new StdScalarDeserializer<String>(String.class) {
				            @Override
				            public String deserialize(JsonParser jsonParser, DeserializationContext ctx)
				              throws IOException {
				              // 重点在这儿:如果为空白则返回null
				              String value = jsonParser.getValueAsString();
				              if (value == null || "".equals(value.trim())) {
				                return null;
				              }
				              return value;
				            }
				          });
				      }
				    };
				  }
				  ```
-
-
-