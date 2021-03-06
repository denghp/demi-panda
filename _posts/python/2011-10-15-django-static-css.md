---
layout: post
title: 在Django模板中使用css、javascript 等静态文件
categories:
- code
- python
- archives
tags:
- python
- django
UUID: 201110152330
date: 2011-10-15
---

  　　在使用Django开发的Web项目中是避免不了使用css、javascript、js等静态文件的，而对于这些静态文件的处理，django官 网这样写：Django itself doesn’t serve static (media) files, such as images, style sheets, or video. It leaves that job to whichever Web server you choose.就是说django本身不处理类似于图片、样式表、视频等静态文件，它将此项工作交给了你选择的Web服务器。
  　　在网上搜索到的django项目处理静态文件的示例中，大家似乎都在使用如下的方法让django处理静态文件：
<pre id="python">
urlpatterns += patterns('',  
      (r'^static/(?P.*)$', 'django.views.static.serve',  
      {'document_root':  settings.MEDIA_ROOT}),  
)
</pre>
   　　而对于django.views.static.serve方法，django官网说得很清楚：Using this method is inefficient and insecure . Do not use this in a production setting. Use this only for development.就是说这种方法是低效且不安全的，不要在生产环境使用此方法，只在开发环境使用。
   　　这时对于静态文件的处理，我们只能使用我们选择的Web服务器来处理了。比如使用nginx服务器的话，可以如下设置：
###设置Django settings.py
<pre id="python">
MEDIA_ROOT = '/home/denghaiping/workspace/djcode/mysite/static/'

# URL that handles the media served from MEDIA_ROOT. Make sure to use a
# trailing slash.
# Examples: "http://media.lawrence.com/media/", "http://example.com/media/"
MEDIA_URL = '/static/'

# Absolute path to the directory static files should be collected to.
# Don't put anything in this directory yourself; store your static files
# in apps' "static/" subdirectories and in STATICFILES_DIRS.
# Example: "/home/media/media.lawrence.com/static/"
STATIC_ROOT = ''

# URL prefix for static files.
# Example: "http://media.lawrence.com/static/"
STATIC_URL = '/static/'

# URL prefix for admin static files -- CSS, JavaScript and images.
# Make sure to use a trailing slash.
# Examples: "http://foo.com/static/admin/", "/static/admin/".
ADMIN_MEDIA_PREFIX = '/static/admin/'
</pre>

###apache2 配置
<pre id="bash">
&lt;Location "/static/"&gt;
SetHandler None
Order allow,deny
Allow from all 
&lt;/Location&gt;
Alias /static /home/denghaiping/workspace/djcode/mysite/static
Alias /static/admin /usr/local/lib/python2.6/dist-packages/django/contrib/admin/media

&lt;Location "/static/admin"&gt;
SetHandler None                                                             
Order allow,deny
Allow from all 
&lt;/Location&gt;
</pre>
