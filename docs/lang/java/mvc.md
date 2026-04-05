
![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/java/mvc.png)

@RestController
It is a convenience syntax for @Controller and @ResponseBody together,This indicates that the class is a controller,and that all the methods in the marked class will return a JSON response.


@ResponseBody
The @ResponseBody is a utility annotation that tells Spring to automatically seriarlize return value(s) of this classes methods into HTTP responses.When building a JSON endpoint,this is an amazing way to "magically" convert your objects into JSON for easier consumption. if we use the @RestController annotation on our class,we don't need this annotation at all,because @RestController inherits from it.


@RequestMapping(method = RequestMethod.GET,value = "/path")
The @RequestMapping(method = RequestMethod.GET,value = "path") annotation specifies a method in the controller that should be reponsible for serving the HTTP request to the given path,or endpoint,Spring handles the mechaincal details of how this is achieved for you. You simply specify the method and path parameters on the annotation and Spring will route the requests into the correct action methods.If you don't specify a method value,it will default to GET.


@GetMapping(value = "/path")
An abbreviated form of @RequestMapping specifically for HTTP GET requests,which only takes an optional value argument,no method argument.The read in CRUD. 查询资源


@PostMapping(value = "/path")
An abbreviated form of @RequestMapping specifically for HTTP POST requests,which only takes an optional value argument,no method argument.The create in CRUD. 创建资源


@PutMapping(value = "/path")
An abbreviated form of @RequestMapping specifically for HTTP PUT requests,which only takes an optional value argument,no method argument.The update in CRUD. 修改资源


@DeleteMapping(value = "/path")
An abbreviated form of @RequestMapping specifically for HTTP DELETE requests,which only takes an optional value argument,no method argument.The delete in CRUD. 删除资源


@RequestParam(value = "/path")
Naturally,the methods handling the requests might take parameters.To help you with binding the HTTP parameters into the action method arguments,you can use the @RequestParam(value="name",defaultValue="World") annotation.Spring will parse the request parameters and put the appropriate ones into your method arguments.

模型（Model）:
    模型代表应用程序的数据和业务逻辑。
    它负责处理数据的获取、存储、更新以及对数据进行各种操作。
    模型通常独立于用户界面和控制流程。

视图（View）:
    视图负责显示模型的数据，通常是用户界面的一部分。
    视图将用户界面元素呈现给用户，并反映模型的当前状态。
    视图通常被设计为可复用的组件，用于显示不同的模型数据。

控制器（Controller）:
    控制器是用户界面和应用程序逻辑之间的协调者。
    它接收用户的输入并相应地更新模型和视图。
    控制器负责将用户的操作转化为对模型的操作，同时更新相关的视图。

> view（前端）通过controller（API接口）触发model（程序响应）

MVC 架构的优势：

- 分离关注点（Separation of Concerns）: 将应用程序的不同方面分离，使得每个组件都专注于特定的任务，提高了代码的可维护性和可扩展性。

- 模块化（Modularity）: 可以更容易地修改、扩展和测试各个组件，因为它们是相对独立的。

- 可重用性（Reusability）: 模型和视图可以在不同的上下文中重用，使得开发更加高效。