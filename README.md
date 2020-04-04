参考链接：https://q1mi.github.io/Django-REST-framework-documentation/api-guide/views_zh/#authentication_classes
### serializer 序列化
#### 序列化与反序列化的过程
1.
content = restframework.JSONRenderer().render(serializer.data) --> bytes
2.
from six import BytesIO
stream = BytesIO(content)
data = JSONParser().parse(stream) ---> dict
3.
serializer = SnippetSerializer(data=data)
serializer.is_valid()
# True
serializer.validated_data
# OrderedDict([('title', ''), ('code', 'print "hello, world"\n'), ('linenos', False), ('language', 'python'), ('style', 'friendly')])
serializer.save()

#### 序列化多个值, 加上many参数
SnippetSerializer(Snippet.objects.all(), many=True)

#### modelserializer 与 serializer的一些区别
区别：
    - serializer没有 create和update 所以serializer.save() 请务必有这两个方法
    - 序列化嵌套：serializer需要自己嵌套，modelserializer 可以指定 depth参数
    - 字段的定义：serializer 手动指定， 而modelserializer 指定fields里就行了

### request / response
参数：POST request.data , GET request.query_params
可以使用api_view 定制api
from rest_framework.decorators import api_view
@api_view(['GET', 'PUT', 'DELETE'])
def fun(request,params=None)

### 视图
APIVIew return csrf_exempt(view) 禁用CSRF
mixins.RetrieveModelMixin, put method
mixins.UpdateModelMixin, update method
mixins.DestroyModelMixin, delete method
mixins.ListModelMixin, get method
mixins.CreateModelMixin, post method
generics.GenericAPIView 提供核心功能
 
自定义操作 detail_route
@detail_route(renderer_classes=[renderers.StaticHTMLRenderer])
    def highlight
    
    
    

API策略属性
下面这些属性控制了API视图可拔插的那些方面。

.renderer_classes
.parser_classes
.authentication_classes
.throttle_classes
.permission_classes
.content_negotiation_class

API policy instantiation methods
下面这些方法被REST framework用来实例化各种可拔插的API策略。你通常不需要重写这些方法。

.get_renderers(self)
.get_parsers(self)
.get_authenticators(self)
.get_throttles(self)
.get_permissions(self)
.get_content_negotiator(self)
.get_exception_handler(self)

参数解析器什么时候会触发
一组视图的有效解析器总是被定义为一个类的列表。当访问request.data时，REST框架将检查传入请求中的Content-Type头，并确定用于解析请求内容的解析器。
