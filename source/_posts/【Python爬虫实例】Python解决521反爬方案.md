---
title: 【Python爬虫实例】Python解决521反爬方案
type: categories
copyright: false
date: 2019-05-05 23:49:14
tags: Python爬虫实例
categories: Python
title_en: python_anti_spider_521
---


> 参考文献：https://github.com/xiantang/Spider/blob/master/Anti_Anti_Spider_521/pass_521.py

## 写在前面的话

`Python`在爬虫方面的优势，想必业界无人不知，随着互联网信息时代的的发展，`Python`爬虫日益突出的地位越来越明显，爬虫与反爬虫愈演愈烈。下面分析一例关于返回`HTTP`状态码为`521`的案例。

## 案例准备

- 案例网站：[【中国一带一路官网】](https://www.yidaiyilu.gov.cn)， 以抓取文章[【“一带一路”建设成果图鉴丨陆海内外联动，湖北推动产能合作纵深推进】](https://www.yidaiyilu.gov.cn/xwzx/gnxw/87373.htm)为例，进行深度剖析。

## 案例剖析
 
1） 浏览器访问[【“一带一路”建设成果图鉴丨陆海内外联动，湖北推动产能合作纵深推进】](https://www.yidaiyilu.gov.cn/xwzx/gnxw/87373.htm)：
![URL访问](/images/python_anti_spider_521_url_yidaiyilu_20190505.png)

2）写`ython`代码访问，查看`http(s)`返回状态

```python
# coding:utf-8


import requests
from fake_useragent import UserAgent


USER_AGENT = UserAgent()
ua = USER_AGENT.random
url = r'https://www.yidaiyilu.gov.cn/xwzx/gnxw/87373.htm'
headers = {
    "Host": "www.yidaiyilu.gov.cn",
    "User-Agent": ua
}
rs = requests.session()
resp = rs.get(url)
print(resp.status_code)
print(resp.text)
```

不幸的是，返回的`http`的状态码却是`501`，`text`为一段混淆的`js`代码。

![request_501](/images/python_anti_spider_521_requests_20190505.png)

3）百度查资料，推荐为文首的[【参考文献】](https://github.com/xiantang/Spider/blob/master/Anti_Anti_Spider_521/pass_521.py)

继续参照资料修改代码，`Python`执行`JS`首选`execjs`，`pip`安装如下：
```shell
pip install PyExecJS
```

将请求到的`js`执行：

```python
text_521 = ''.join(re.findall('<script>(.*?)</script>', resp.text))
func_return = text_521.replace('eval', 'return')
content = execjs.compile(func_return)
print(content.call('f'))
```

将返回的结果`print`发现还是一段`JS`，标准格式化（[【格式化`Javascript`工具】](https://www.html.cn/tool/js_beautify/)），结果如下所示：

```javascript
var _2i = function () {
        setTimeout('location.href=location.pathname+location.search.replace(/[\?|&]captcha-challenge/,\'\')', 1500);
        document.cookie = '__jsl_clearance=1557019601.296|0|' + (function () {
                    var _2i = [([(-~[] << -~[])] * (((+!+{}) + [(-~[] << -~[])] >> (-~[] << -~[]))) + []), (-~{} + [] + [
                                []
                            ][0]) + [3 - ~(+!+{}) - ~(+!+{})], (-~{} + [] + [
                                []
                            ][0]) + [5], (-~{} + [] + [
                                []
                            ][0]) + [~~''], (-~{} + [] + [
                                []
                            ][0]), [-~~~!{} + [~~
                                []
                            ] - (-~~~!{})], (-~{} + [] + [
                                []
                            ][0]) + [-~{} - ~[-~{} - ~{}]], (-~{} + [] + [
                                []
                            ][0]) + (-~{} + [] + [
                                []
                            ][0]), [-~(+!+{})], (-~{} + [] + [
                                []
                            ][0]) + ([(-~[] << -~[])] * (((+!+{}) + [(-~[] << -~[])] >> (-~[] << -~[]))) + []), (-~{} + [] + [
                                []
                            ][0]) + (-~[-~{} - ~{}] + [
                                []
                            ][0]), (((-~[] << -~[]) << (-~[] << -~[])) + [
                                []
                            ][0]), [3 - ~(+!+{}) - ~(+!+{})], (-~{} + [] + [
                                []
                            ][0]) + (((-~[] << -~[]) << (-~[] << -~[])) + [
                                []
                            ][0]), [5],
                            [-~{} - ~[-~{} - ~{}]], (-~{} + [] + [
                                []
                            ][0]) + [-~(+!+{})], (-~[-~{} - ~{}] + [
                                []
                            ][0]), [~~'']
                        ],
                        _1d = Array(_2i.length);
                    for (var _5 = 0; _5 < _2i.length; _5++) {
                        _1d[_2i[_5]] = ['Bz', (-~[-~{} - ~{}] + [
                                    []
                                ][0]), [{} + [] + [
                                    []
                                ][0]][0].charAt(-~~~!{}), 'DR', ([(-~[] << -~[])] / (+!/!/) + [] + [
                                    []
                                ][0]).charAt(-~[-~~~!{} - ~(-~[] - ~{} - ~{})]) + (+[(+!+{}), (+!+{})] + []).charAt((+!+{})), [
                                    [][
                                        []
                                    ] + [] + [
                                        []
                                    ][0]
                                ][0].charAt(-~{} - ~[-~{} - ~{}]), 'qM', (((-~[] << -~[]) << (-~[] << -~[])) + [
                                    []
                                ][0]) + (+[(+!+{}), (+!+{})] + []).charAt((+!+{})) + (-~{}
                                    /~~''+[]+[[]][0]).charAt((+!/!/)),'S','g%',(((-~[]<<-~[])<<(-~[]<<-~[]))+[[]][0]),'HxXL',[[][[]]+[]+[[]][0]][0].charAt(-~{}-~[-~{}-~{}]),'D',[-~(+!+{})],'T%','YW',[{}+[]+[[]][0]][0].charAt(-~~~!{}),'vw'][_5]};return _1d.join('')})()+';Expires=Sun, 05-May-19 02:26:41 GMT;Path=/;
                                    '};if((function(){try{return !!window.addEventListener;}catch(e){return false;}})()){document.addEventListener('
                                    DOMContentLoaded ',_2i,false)}else{document.attachEvent('
                                    onreadystatechange ',_2i)}
```

4）修改与浏览器相关的代码，然后放入浏览器的`console`进行调试。

![JS执行结果](/images/python_anti_spider_521_js_debug_20190505.png)

**注意**，在调试过程中，不难发现，`js`变量是动态生成的。最初还嵌套有`document.createElement('div')`，`Python`的`execjs`包不支持处理这类代码，需要做相应处理。

5）综上分析，完整代码如下：

```python
#!/usr/bin/env python
# coding:utf-8


import sys
import re
import requests
import execjs
from fake_useragent import UserAgent


reload(sys)
sys.setdefaultencoding('utf8')


class YiDaiYiLuSpider(object):
    """
    中国一带一路网（521反爬）
    """

    USER_AGENT = UserAgent()
    ua = USER_AGENT.random
    url = r'https://www.yidaiyilu.gov.cn/xwzx/gnxw/87373.htm'
    headers = {
        "Host": "www.yidaiyilu.gov.cn",
        "User-Agent": ua
    }

    @classmethod
    def get_text521(cls):
        """

        :return:
        """

        rs = requests.session()
        resp = rs.get(url=cls.url, headers=cls.headers)
        text_521 = ''.join(re.findall('<script>(.*?)</script>', resp.text))
        cookie_id = '; '.join(['='.join(item) for item in resp.cookies.items()])
        return cookie_id, text_521

    @classmethod
    def generate_cookies(cls, func):
        """

        :param func:
        :return:
        """

        func_return = func.replace('eval', 'return')
        content = execjs.compile(func_return)
        eval_func = content.call('f')
        var = str(eval_func.split('=')[0]).split(' ')[1]
        rex = r">(.*?)</a>"
        rex_var = re.findall(rex, eval_func)[0]
        mode_func = eval_func.replace('document.cookie=', 'return ').replace(';if((function(){try{return !!window.addEventListener;}', ''). \
            replace("catch(e){return false;}})()){document.addEventListener('DOMContentLoaded'," + var + ",false)}", ''). \
            replace("else{document.attachEvent('onreadystatechange'," + var + ")}", '').\
            replace(r"setTimeout('location.href=location.pathname+location.search.replace(/[\?|&]captcha-challenge/,\'\')',1500);", '').\
            replace('return return', 'return').\
            replace("document.createElement('div')", '"https://www.yidaiyilu.gov.cn/"').\
            replace(r"{0}.innerHTML='<a href=\'/\'>{1}</a>';{0}={0}.firstChild.href;".format(var, rex_var), '')

        content = execjs.compile(mode_func)
        cookies_js = content.call(var)
        __jsl_clearance = cookies_js.split(';')[0]
        return __jsl_clearance

    @classmethod
    def crawler(cls):
        """

        :return:
        """

        url = r'https://www.yidaiyilu.gov.cn/zchj/sbwj/87255.htm'
        cookie_id, text_521 = cls.get_text521()
        __jsl_clearance = cls.generate_cookies(text_521)
        cookies = "{0};{1};".format(cookie_id, __jsl_clearance)
        cls.headers["Cookie"] = cookies
        print(cls.headers)
        res = requests.get(url=url, headers=cls.headers)
        res.encoding = 'utf-8'
        print(res.text)


if __name__ == '__main__':

    YiDaiYiLuSpider.crawler()

```

运行结果如下：
![运行结果](/images/python_anti_spider_521_js_result_20190505.png)