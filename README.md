####话不多说，先来个图。
![resume.gif](http://upload-images.jianshu.io/upload_images/1503554-6cf5d5b45fc63885.gif?imageMogr2/auto-orient/strip)

最近在学Django，因为看Python上都要求会至少一门web框架，如Django，flask...，既然有要求那就来学学呗。
光学习还是太无聊了些，我们来做点东西，既然想要投简历，我们不妨用Django来做份简历，到是否发个链接过去。
>一来，可以解放一下HR大人，不至于每天看着枯燥的简历。
二来，了解我的都知道，学习嘛，做点东西会学的更快，学到的也会更多。

写在开始之前。
>一开始我以为可以在自己电脑上使用Nginx就可以实现发布，结果发现并不能实现。后来问公司运维说，需要域名备案然后部署到服务器才行。所以现在写的项目还是无法给HR看的。不过就当是记录学习了。

来吧，开始干~
额。。一开始就遇到了一个问题，环境，好在之前刚写了一篇关于环境搭建的文章。[python环境搭建](http://www.jianshu.com/p/5697521e37f8)。有需要的可以看一下这篇文章。
#####1.下面我们还是要先创建环境，来吧
很简单~
```
mkvirtualenv myresume
```
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1503554-1a7376b255a2fbf0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们需要记录一下virtualenv的位置
现在环境里东西不多等后面用我们在回来添加。
#####2.新建项目
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1503554-45f7dffe3b3d799e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

OK,来安装Django，回到virtualenv myresume环境下。
```
pip install django
```
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1503554-4b7b78954fb7f556.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

OK,切换一下，重新安装选择一下myresume这个环境，发现黄色部分消失，click  create。

#####3.数据库
很开心，这里我们用不到数据库，因为我们不对数据进行保存，也就意味着我们的Model也用不到。

#####4.运行项目
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1503554-555bc718123570bd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1503554-2530e16f62f0b8b1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

恭喜你运行成功，我们离成功还剩50%。

#####5.创建resume app
在django中是通过一个一个的app来分割项目，所以这里我们需要创建我们的resume app。
工具栏Tools中有一个Run manager.py Task... 我们来运行它来创建，运行效果是这样。
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1503554-cc5c186dc0c39daa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果你的是测试版的Pycharm，则需要在Terminal中进入到项目的目录然后执行 python manager.py startapp resume。运行大概是这样。
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1503554-77af54ee96a3050e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所以说专业版会有很多简单的功能供我们使用，看来收费也是有道理的。
这样我们发现我们的项目中多了一个文件目录。resume就是我们刚才创建的。
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1503554-3b3a99676f6a558f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>紧接着的一步是在settings文件中添加上我们创建的app

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1503554-e8bb845c05b5c1dd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

忘记就会报错，最好创建完接着来添加。

#####6.创建Static文件
本来我们需要创建html文件了，但是我们有可能会用到css，js这样的文件。所以我们先来创建一个static文件目录。目录大概是这样。
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1503554-7cc4ca75a5f7fcb9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

为了后期更方便的使用，我们需要，于是我们来到settings最下面。
添加上，这样我们就不需要输入static目录全称了。
```
#正式上线时用到
# STATIC_ROOT = os.path.join(BASE_DIR, 'static')
#正式上线时需要注释掉
STATICFILES_DIRS = (
	os.path.join(BASE_DIR, 'static'),
)
```
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1503554-6fee74cd04bb0022.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#####7.创建html
django html我们是需要卸载templates中，否则在运行的时候会报错。
这里我们创建的html是resume.html。
这个上面的目录图已经截到了，这里不重复，为什么有两个，因为之前写过，这里就先拿过来用了。
html我是路上的学徒，有更好的大家可以自己写。这里我就不献丑了。
有了html后，我们要做的就是网络处理了。

#####8.网络处理
首先我们来到resume app目录下的views.py，引入View然后创建myResumeView Class，重写get方法。为什么使用View来管理，因为当我们有get与post的使用，我们就不需要判断request的类型，节省代码。
```
from django.views.generic import View


class myResumeView(View):
	def get(self, request):
		return render(request, 'myresume.html', {})

```
然后我们来到myresume 目录下的urls.py来配置我们的url
```
from django.conf.urls import url
from django.contrib import admin
#引入myResumeView
from resume.views import myResumeView

urlpatterns = [
	url(r'^admin/', admin.site.urls),
	#添加url配置
	url(r'^resume$', myResumeView.as_view(), name='resume')
	]
```
OK，到这里我们的项目算是基本成功了。
我们运行一下看看。
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1503554-2e93c34513adb15d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>因为我们配置了url所以当我们再输入http://127.0.0.1:8000/ 时会报错，不要慌张，在后面加上resume 即可。http://127.0.0.1:8000/resume 

这样我们基本成功了。不过跟普通没什么区别吗，我们可以来添加点动画多好。
来哦，添加动画。
#####8.添加动画
首先我们需要的是fullpage这个神器。[fullpage github](https://github.com/alvarotrigo/fullPage.js#creating-links-to-sections-or-slides)
另外一个是animation.css [animation.css github](https://github.com/daneden/animate.css)
这两个绝对是神器使用方法我就不说了打击可以看一下这篇文章[fullpage with animation.css小技巧](https://webdesign.tutsplus.com/zh-hans/tutorials/quick-tip-scroll-animations-with-fullpagejs-and-animatecss--cms-25235)，我会把我的发到github上，有需要可以看一下。
#####9.需要注意的地方
							```
{% load staticfiles %} 

{% static 'css/jquery.fullPage.css' %}
```
我们在html中添加了这样一行，这里是为了下面使用{% static  '/xxx/' %}。为什么使用，如果我们修改了static的目录，我们这里不需要任何修改，如果使用绝对路径，我们后期可能需要一个一个修改，为了以后这里先麻烦一下。

OK，不全后我们看一下效果。
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1503554-135cc85f39c9f89f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

OK，这样就与我们开始一样了，剩下的就是发布到服务器上了。这个待我研究研究。

get到的新的强大技能
- django静态文件在html中引入新解。
- fullpage使用，这个很多都用在首页
- animation.css使用，这的很强大很方便。
- django View使用，使用请求逻辑划分的更加清楚。


