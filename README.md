# Robot Framework

## 背景

Robot Framework 可用于测试接口也可用于测试 WEB UI ，并且可以自定义 Python Library 以定义 Key Word 完成特定的测试需求。

## 运用

- 条件
  - Python
    - 验证输入：python --version
    - 验证输出：Python 3.x.x
  - 安装 Robot Framework
    - 命令：pip install robotframework
    - 验证输入：robot --version
    - 验证输出: Robot Framework 3.1.2 (Python 3.x.x on win32)
  - 安装 Ride，[参考]
    - 命令：pip install robotframework-ride
    - 在“运行”中输入：ride.py
  - 安装 Selenium 2 (WebDriver) Library，[参考]
    - 命令：pip install robotframework-selenium2library
    - 升级 selenium2library：pip install robotframework-selenium2library

## 登录脚本

``` python
*** Settings ***
Library           Selenium2Library

*** Variables ***
${Username}       test
${Password}       123456
${Browser}        chrome
${SiteUrl}        https://test.com
${ValidateKey}    testkey

*** Test Cases ***
登录 应该登录成功
    Open Url
    Enter User Name
    Enter Password
    Enter ValidateKey
    Click Login
    Assert Error Message

*** Keywords ***
Open Url
    Open Browser    ${SiteUrl}    ${Browser}
    Maximize Browser Window

Enter User Name
    Input Text    id=username    ${Username}

Enter Password
    Input Text    id=password    ${Password}

Enter ValidateKey
    Input Text    id=chkkey    ${ValidateKey}

Click Login
    Click Button    Tag=button

Assert Error Message
    Page Should Not Contain Textfield    id=username
```

## Ride

  Ride 是 Robot Framework 的官方操作 UI ,使用 Python 写的。

### 自定义 Key Word

  使用 Python 定义想要处理的内容，例如：根据某些条件从数据库中读出当前申请单处理人，通过脚本重新打开一个浏览器，使用处理人的账号密码登录，处理该阶段该处理的内容。
  在 Key Word 逻辑中处理内容，下面 Python 代码中的 printAllInfo 就是关键字，在 Ride 中可以写微 Print All Info 或者 print all info 等

`HelloWorld.py`

``` python
from User import User


class HelloWorld:
    """这是一个示例 Library"""
    ROBOT_LIBRARY_SCOPE = 'TEST SUITE'

    @staticmethod
    def printHelloWorld(text=None):
        if text is None or len(text) == 0:
            print("Hello world")
        else:
            print(text)

    """Key Word 打印用户所有信息"""
    def printAllInfo(self):
        self.printHelloWorld()

        user = User()
        self.printHelloWorld(user.getName())
        self.printHelloWorld(str(user.getAge()))


if __name__ == '__main__':
    HelloWorld().printAllInfo()
```
