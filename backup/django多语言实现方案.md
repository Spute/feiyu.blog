- 使用`python manage.py makemessages -l en`命令生成英文文本文件。多语言文件使用.po格式，中文作为默认语言。
- 编辑文本文件，为中文添加对应英文。
- 使用`python manage.py compilemessages`命令编译文本文件，生成二进制的.mo格式文件。Django在运行时使用这个文件来加载翻译。相对于使用JSON文件映射，mo二进制文件加载翻译的方式，具备性能更好、内存消耗更低、标准支持更好等优点。
- 通过请求头识别客户端需求使用的语言。基于django的内置中间件`'django.middleware.locale.LocaleMiddleware'`实现。例如：
  - Accept-Language:zh-hans 表示简体中文
  - Accept-Language:en 表示英文
- 代码中使用翻译方法
```
from django.utils.translation import gettext as _
user_message=_('请检查您的输入并重试。')
```