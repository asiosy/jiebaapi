# jiebaapi
利用该分词API，可以直接在前端调取服务获得分词(去掉停用词)后的结果，示例如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190606101903857.png)
 Python 和DJingo的安装可参考：https://www.runoob.com/django/django-install.html
Idea下搭建DJingo项目可参考：https://blog.csdn.net/yybk426/article/details/75042833

实现分词部分代码结构如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019060610091498.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjM2MjM5Mw==,size_16,color_FFFFFF,t_70)
调用jieba分词：
```
def fcfunction(section):#分词
    jsonr=jieba.cut(section)#, cut_all=True
    stopwords = stopwordslist()
    revj=''
    for word in jsonr:
        if word not in stopwords:
            if word != '\t':
                revj += word+'|'
    return revj
```
去掉停用词：
```
def stopwordslist():
    stopwords = [line.strip() for line in open('templates/stopword.txt',encoding='UTF-8').readlines()]
    return stopwords
```
输出结果：

```
def index(request):#定义一个函数，第一个参数必须是request
    #return HttpResponse("Hello, world. Hello，python.")#返回HttpResonse对象，最终将这行字符显示在页面上
    request.encoding='utf=8'
    section =urllib.parse.unquote(request.GET['section'])
    return HttpResponse(fcfunction(section))
```
前端页面调用：

```
function search() {
        $.ajax({
            type: "GET",
            async: false,
            data: { "section": encodeURI($("#searchValue").val()) },
            url: "http://127.0.0.1:8000/lawsearch/hello",
            success: function (res) {
                var resp = res.split('|');
                //...具体方法               
            }
        });
    }
```
BTW:可通过cmd修改服务IP，例如：python manage.py runserver 192.168.0.1:8000
并在setting.py的ALLOWED_HOSTS中增加该IP：ALLOWED_HOSTS = ['192.168.0.1']

