---
layout: post
title: 搭建 Python+Selenium+Mitmproxy 爬虫开发环境
categories: Python
description: 新手在 macOS 上搭建 Python+Selenium+Mitmproxy 爬虫开发环境遇到的一些坑，以及用到的工具。
keywords: python, selenium, mitmproxy, 爬虫
---

# 简介

最近学习并使用 Python + Selenium + Mitmproxy 来搭建爬虫系统，刚刚入手踩了很多的坑，写下来以备不时之需。

## 安装 Python3.*

macOS 中自带 Python2.* ，我的系统中是 Python2.7，学习了几天才发现和最新的 3.* 版本不兼容 !- -，既然是学习，还是选择了更高的 3.* 版本，况且官方也更推荐 3.* 的版本。

**下载安装：**

到这里下载适合你的版本 <https://www.python.org/downloads/mac-osx/> 直接安装就可以了。

**使用**

安装完成后想要在终端中打开 Python3.* 的解释器不能再使用 `python` 了，而是使用 `python3`，相应的 `pip` 包管理工具的命令也变成了 `pip3`：

    puthon # 打开 python2.* 的解释器界面
    pip # 打开 python2.* 的包管理工具命令
    
    puthon3 # 打开 python3.* 的解释器界面
    pip3 # 打开 python2.* 的包管理工具命令

当然我们很少直接使用 mac 终端下的 Python 环境，而是使用虚拟环境。

**搭建虚拟环境**

搭建虚拟环境的方式有两种，第一种使用 Python 虚拟环境管理包 `virtualenv` 创建，第二中是使用 IDE 创建。

**第一种 virtualenv：**

下载安装 virtualenv，在终端中运行如下命令：

    pip3 install virtualenv
    
创建一个项目目录，并进入到目录：

    Mac:~ michael$ mkdir myproject
    Mac:~ michael$ cd myproject/
    Mac:myproject michael$
    
创建一个独立的 Python 运行环境，命名为venv：

    virtualenv --no-site-packages venv
    
进入项目虚拟环境：

    source venv/bin/activate
    
退出项目虚拟环境：

    deactivate
    
**第二种 使用 IDE 创建**

我使用的 IDE 是 PyCharm，PyCharm 提供了创建虚拟环境的功能，打开 PyCharm 点击创建项目，选择相应的 Python 版本然后点击确定就可以自动创建虚拟环境：

![PyCharm](https://jichao257.github.io/images/posts/python/pycharm_venv.png)

你可以看到 IDE 依然是使用的上面的命令进行创建的，所以进入和退出虚拟环境和第一种是一样的。

**使用虚拟环境进行开发的好处**
    
当我们进入项目虚拟环境后就可以直接使用 `python` 和 `pip` 来运行和管理我们的项目了，这个时候因为是在虚拟环境中，所以依然使用的是 3.* 的环境。

使用虚拟环境的另外的好处是开发环境的隔离，以及包依赖管理更加方便，而不是一个项目依赖很多不需要的包。

我们再安装一个包管理工具 `pipreqs` 吧：

    pip install pipreqs

在项目根目录下运行下面的命令就可以自动生成我们的项目依赖：

    pipreqs ./ --force

这时候我们会得到项目的依赖文件 `requirements.txt`，这样如果我们迁移项目的话使用下面的命令就可以很轻松的安装依赖了：

    pip install -r ./requirements.txt

## 安装 Selenium

Selenium 是一个用于Web应用程序测试的工具。Selenium测试直接运行在浏览器中，就像真正的用户在操作一样。支持的浏览器包括IE（7, 8, 9, 10, 11），Mozilla Firefox，Safari，Google Chrome，Opera等。这个工具的主要功能包括：测试与浏览器的兼容性——测试你的应用程序看是否能够很好得工作在不同浏览器和操作系统之上。测试系统功能——创建回归测试检验软件功能和用户需求。支持自动录制动作和自动生成 .Net、Java、Perl等不同语言的测试脚本。(百度百科)

想要使用 selenium 我们需要安装两个东西，一是 Python 的 selenium 包，二是浏览器提供的驱动（webdriver）：

进入虚拟环境中使用如下命令就可以安装 selenium 了：

    pip install selenium
    
我使用的是 Chrome 浏览器 ，所以我要去下载 Chrome 的 webdriver，点击如下链接到下载页面直接下载:

- 官方地址：<https://sites.google.com/a/chromium.org/chromedriver/home>
- 淘宝地址：<https://npm.taobao.org/mirrors/chromedriver>

唯一需要注意的是，你应该下载对应浏览器版本的 webdriver，下载好之后将 webdriver 放到 `/usr/local/bin/` 目录下就可以了。

现在我们可以使用如下代码测试一下：

    from selenium import webdriver

    driver = webdriver.Chrome()
    driver.get('https://www.baidu.com')
    
    print(driver.title)
    
    driver.quit()
    
如果能够正常打开浏览器并访问百度就说明安装成功了。

## 安装 Mitmproxy

Mitmproxy 就是用于 MITM 的 proxy，MITM 即中间人攻击（Man-in-the-middle attack）。用于中间人攻击的代理首先会向正常的代理一样转发请求，保障服务端与客户端的通信，其次，会适时的查、记录其截获的数据，或篡改数据，引发服务端或客户端特定的行为。

**安装 Mitmproxy，在虚拟环境中运行如下命令：**

    pip install mitmproxy

完成后，系统将拥有 mitmproxy、mitmdump、mitmweb 三个命令。

**运行**

要启动 mitmproxy 用 mitmproxy、mitmdump、mitmweb 这三个命令中的任意一个即可，这三个命令功能一致，且都可以加载自定义脚本，唯一的区别是交互界面的不同。

- mitmproxy 命令启动后，会提供一个命令行界面，用户可以实时看到发生的请求，并通过命令过滤请求，查看请求数据。
- mitmweb 命令启动后，会提供一个 web 界面，用户可以实时看到发生的请求，并通过 GUI 交互来过滤请求，查看请求数据。
- mitmdump 命令启动后——你应该猜到了，没有界面，程序默默运行，所以 mitmdump 无法提供过滤请求、查看数据的功能，只能结合自定义脚本，默默工作。

启动并加载过滤脚本：

    mitmweb -s xxx.py

## 使用 Mitmproxy 来绕过网站服务器对 Selenium 的屏蔽

在我们使用 Selenium 通过 webdriver 来访问网站的时候，网站服务器可以通过一些特别饿 js 变量来识别是否使用了 Selenium 如 `webdriver`，从而使得网站服务器能够识别我们编写的爬虫脚本。

这里我们使用 Mitmproxy 来替换关键变量，使得网站服务器无法识别 Selenium，这里我们编写的 Mitmproxy 的过滤脚本如下：

    # file: mitmproxy_local.py
    
    from mitmproxy import ctx

    injected_javascript = '''
    // overwrite the `languages` property to use a custom getter
    Object.defineProperty(navigator, "languages", {
      get: function() {
        return ["zh-CN","zh","zh-TW","en-US","en"];
      }
    });
    // Overwrite the `plugins` property to use a custom getter.
    Object.defineProperty(navigator, 'plugins', {
      get: () => [1, 2, 3, 4, 5],
    });
    // Pass the Webdriver test
    Object.defineProperty(navigator, 'webdriver', {
      get: () => false,
    });
    // Pass the Chrome Test.
    // We can mock this in as much depth as we need for the test.
    window.navigator.chrome = {
      runtime: {},
      // etc.
    };
    // Pass the Permissions Test.
    const originalQuery = window.navigator.permissions.query;
    window.navigator.permissions.query = (parameters) => (
      parameters.name === 'notifications' ?
        Promise.resolve({ state: Notification.permission }) :
        originalQuery(parameters)
    );
    '''
    
    
    def response(flow):
        # Only process 200 responses of HTML content.
        if not flow.response.status_code == 200:
            return
    
        # Inject a script tag containing the JavaScript.
        html = flow.response.text
        html = html.replace('<head>', '<head><script>%s</script>' % injected_javascript)
        flow.response.text = str(html)
        ctx.log.info('>>>> js代码插入成功 <<<<')
    
        # 只要url链接以target开头，则将网页内容替换为目前网址
        # target = 'https://target-url.com'
        # if flow.url.startswith(target):
        #     flow.response.text = flow.url

然后使用 Mitmproxy 的启动命令来启动并加载过滤脚本：

    mitmweb -s ./mitmproxy_local.py 

现在我们指定 Selenium 来使用我们启动的 Mitmproxy 代理：

    from selenium import webdriver
    
    chrome_options = webdriver.ChromeOptions()
    chrome_options.add_argument('--proxy-server=http://127.0.0.1:8080')
    driver = webdriver.Chrome(chrome_options=chrome_options)
    
    driver.get('http://www.baidu.com')
    
这时候当我们看到 Mitmproxy 打印的 js 替换成功的信息，就说明我们代理成功了。

## 让 Selenium 打开指定的（打开过的）浏览器窗口

让 Selenium 打开指定的（打开过的）浏览器窗口，可以更方便的维持登录，但 Selenium 并不支持打开指定的浏览器窗口，所以我们需要自己使用代码来实现。我们可以编写一个类来继承 Chrom 的 WebDriver 类：

    from selenium.webdriver import Remote
    from selenium.webdriver.chrome import options
    from selenium.common.exceptions import InvalidArgumentException
    
    
    class ReuseChrome(Remote):
    
        def __init__(self, command_executor, session_id):
            self.r_session_id = session_id
            Remote.__init__(self, command_executor=command_executor, desired_capabilities={})
    
        def start_session(self, capabilities, browser_profile=None):
            """
            重写start_session方法
            """
            if not isinstance(capabilities, dict):
                raise InvalidArgumentException("Capabilities must be a dictionary")
            if browser_profile:
                if "moz:firefoxOptions" in capabilities:
                    capabilities["moz:firefoxOptions"]["profile"] = browser_profile.encoded
                else:
                    capabilities.update({'firefox_profile': browser_profile.encoded})
    
            self.capabilities = options.Options().to_capabilities()
            self.session_id = self.r_session_id
            self.w3c = False

这样我们使用这个类和之前保存下载的 `command_executor` 和 `session_id` 就可以打开之前打开的窗口了：

        def build_driver():
    
            filename = './session.txt'
    
            try:
                with open(filename, 'r') as f:
                    sessions = json.load(f)
            except FileNotFoundError:
                sessions = dict()
    
            if not len(sessions):
                driver = webdriver.Chrome()
    
                executor_url = driver.command_executor._url
                session_id = driver.session_id
    
                with open(filename, 'w') as f:
                    json.dump({'executor_url': executor_url, 'session_id': session_id}, f)
                    else:
                    
            self._driver = ReuseChrome(command_executor=sessions['executor_url'], session_id=sessions['session_id'])
                windows = driver.window_handles
                driver.switch_to.window(windows[0])
    
            return driver

## 参考文献

- [廖雪峰的 Python 教程](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001432712108300322c61f256c74803b43bfd65c6f8d0d0000)
- [如何解决 Python 包依赖问题](https://www.jianshu.com/p/8a7de18e0ffb)
- [Selenium Python 简易教程](http://www.testclass.net/selenium_python)
- [使用 mitmproxy + python 做拦截代理](https://www.cnblogs.com/grandlulu/p/9525417.html)
- [Python Webdriver 重新使用已经打开的浏览器实例](https://www.cnblogs.com/jhao/p/8267929.html)