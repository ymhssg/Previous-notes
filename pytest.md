# 1.用例规则

    1. 文件名需要以  test_  或者  _test  结尾
    2. 测试类 需要以Test开头 , 且不能有init方法
    3. 测试方法 必须以test开头

```
// -v 显示更详细信息
// -s 显示执行文件结果
// -n 多线程执行测试用例
// -x 只要一个用例出错就停止
```



执行

```
pytest 文件名.py

#运行test_demo.py 测试类里面的某个方法
	pytest test_demo.py::test_in
```

```
#通过 pytest.ini配置文件运行 ---一般放在项目根目录

```



# 2.断言

      name = '名字'
      assert name = '名字','不是名字'



# 3.pytest.ini ---配置文件

```
[pytest]
#命令行参数
addopts = --vs	
#测试用例执行位置
testpaths = 
#测试文件名用什么模式开头
python_files = test_*.py
#测试文件名类名用什么模式开头
python_classes = Test*
#测试文件函数名用什么模式开头
python_functions = test_*


#用于分组执行用例
markers=
	smoke:冒烟用例
	uesrmanage:用户管理模块用例
-------------------------------------------------------	
pytest -m "smoke"         # 运行所有标记为 smoke 的用例
```



# 4.高级用法

## 	1.mark

​		标记为了用例之间彼此不同

​			1.注册标记----创建pytest.ini文件------>**pytest配置文件**(需要手动创建)

​			![1739614033625(1)](C:\Users\ymhssg\Documents\WeChat Files\wxid_0yh640h3dm4r22\FileStorage\Temp\1739614033625(1).png)

​			2.贴上标记

​                       ![1739614266362](C:\Users\ymhssg\Documents\WeChat Files\wxid_0yh640h3dm4r22\FileStorage\Temp\1739614266362.png)     

​			3.筛选标记

​			![1739621069181(1)](C:\Users\ymhssg\Documents\WeChat Files\wxid_0yh640h3dm4r22\FileStorage\Temp\1739621069181(1).png)

```
#设置执行顺序
@pytest.mark.run(order=1)
@pytest.mark.run(order=2)
```

```
#跳过某个测试用例
#reason="功能未实现"--->跳过原因
@pytest.mark.skip
例如
@pytest.mark.skip(reason="功能未实现")
def test_unfinished():
    assert False
```

```
#根据条件跳过测试（如环境依赖不满足时）
@pytest.mark.skipif import sys
例如
import sys
@pytest.mark.skipif(sys.version_info < (3, 8), reason="需要 Python 3.8+")
def test_python38_feature():
    assert True
```

```
#标记预期失败的测试（如已知 Bug 尚未修复）
@pytest.mark.xfail
```

```
#强制测试用例使用指定的 Fixture
@pytest.mark.usefixtures
例如
@pytest.mark.usefixtures("clean_database")
def test_query():
    assert True
    
```

#### **1自定义标记的作用**

```
#区分冒烟测试（smoke）、回归测试（regression）
```

```
#标记测试环境--本地环境（local）、生产环境（production）
```

```
自定义标记的执行---****记得在pytest.ini中分组

	pytest -m "smoke"         # 运行所有标记为 smoke 的用例
	pytest -m "not smoke"     # 排除 smoke 标记的用例
	
#支持 and、or、not 等逻辑运算符，灵活筛选用例
	pytest -m "smoke and P0" # 同时满足 smoke 和 P0 的用例
```

## 2. fixture

​	自动在用例前,之后完成,用于测试环的构建和销毁

​	使用生成器(yield)实现前置,后置的分离

​	conftest.py 创建全局范围fixture

​	fixture的定义及使用

​	![1739622362930](C:\Users\ymhssg\Documents\WeChat Files\wxid_0yh640h3dm4r22\FileStorage\Temp\1739622362930.png)





## 3.hook

​	允许进入和退出pytest核心内部

​	目的 : 改变pytest原有处理方式,运行模式

​	![1739623734244](C:\Users\ymhssg\Documents\WeChat Files\wxid_0yh640h3dm4r22\FileStorage\Temp\1739623734244.png)



# 5.如何分组执行

自定义标签+在pytest.ini中分组



# 6.setup 和teardown (后期被fixture替换)

- **作用**：整个测试模块（文件）运行前/后执行一次 //**作用**：测试类中所有测试方法运行前/后执行一次

setup作用 :（如创建资源、连接数据库）

teardown作用 : 清理测试残留（如关闭连接、删除临时文件）

```
class TestDemo:
	#每个用例的开始执行一次
    def setup(self):
        print("在test_method前执行")
        
	#每个用例的结束执行一次
    def teardown(self):
        print("在test_method之后执行")

    def test_method(self):
        assert True
```



# 7.fixture

通过 `scope` 参数控制fixtrue的作用域

​	function（默认）：每个测试函数执行一次。

​	class：每个测试类执行一次。

​	module：每个测试模块执行一次。

​	session：整个测试会话执行一次。

使用 `params` 参数为 Fixture 提供多组输入

通过 `autouse=True` 让 Fixture 自动执行，无需调用

当使用 `params` 参数时 `id=""`用于给每一个参数值设置一个变量名 

 `name=` 给被fixture标记的方法取别名,取了别名之后,原来名称就用不了了

```
@pytest.fixture(scope="",params="",autouse="",id="",name="")
```



scope例子

```
@pytest.fixture(scope="function")
def aaa():
	print("前置")
	yield	    ###ruturn和yield都表示返回,只是yield后面可以接代码
	print("后置")
	
class Test:
	def test_01(self,aaa):
	print("测试中")
	
====结果===
前置 测试中 后置
```



param用例

```python
@pytest.fixture(scope="function",params=["1","2"])
def aaa(request):
	return request.param
	
class Test:
	def test_01(self,aaa):
	print("测试中")
	print(str(aaa))
	
	def test_02(self,aaa):
	print("测试中")
	print(str(aaa))
	
#####结果######
测试中 1
测试中 2
```



```

```



# 8.conftest.py

单独存在的夹具配置文件,可以在不同的py文件中使用同一个前置

```
project/
├── conftest.py               # 全局配置：浏览器、日志、数据库
├── tests/
│   ├── conftest.py           # 通用测试配置（如登录 Fixture）
│   ├── web/
│   │   ├── conftest.py       # Web 测试专用（如页面对象初始化）
│   │   └── test_login.py
│   └── api/
│       ├── conftest.py       # API 测试专用（如鉴权 Fixture）
│       └── test_user.py
└── utils/
    └── helpers.py
```



#### 1.**共享 Fixture**

#### 2.**模块化测试配置**

#### 3.**动态参数化测试**



# alure-pytest插件生成报告

将测试的缓存json存入temp目录中

```
#pytsts.ini

addopts = --vs	--alluredir ./temp
```

```
#allure报告生成

if __name__ =='__main__':
    pytest.main(['-s'])
    os.system('allure generate ./temp -o ./result --clearn')
  
-------------------------------
--alluredir：pytest 需要先将结果保存到指定目录（如 ./pytest）
-o：指定报告输出目录（如 ./result）
--clean：确保每次生成新报告前清理旧文件
     
```



# @pytest.mark.parametrize()

```
@pytest.mark.parametrize(参数名,参数值)
```

```
@pytest.mark.parametrize(aaa,['1','2'])

	
class Test:
	def test_01(self,aaa):
		print(aaa)
		
if __name__=='__main__':
	pytest.main()
	
========结果====
1 2
```



# yaml文件

1.用于全局的配置文件  ini ,yaml

2.用于写测试用例



```python
####yaml_util.py
##加载yaml数据

import yaml
class yaml:
	#通过init将yaml文件传入到这个类
	def __init__(self,yaml_file):
		self.yaml_file = yaml_file
	
    #读取yaml文件
	def load_yaml(yaml_file):
    	with open(self.yaml_file, 'r', 	encoding='utf-8') as f:
            #将yaml格式转化为字典格式
        	value = yaml.load(f,Loader=yaml.FullLoader)
            print(value)
            
if __name__ =='__main__':
    YamlUtil("test_01.yaml").read_yaml()
```

