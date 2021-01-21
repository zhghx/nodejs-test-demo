# nodejs-test-demo
nodejs-unit 测试练习 

# NodeJs测试文档

## 一. 介绍
1. Mocha：一种测试框架
2. Chai：一种断言库
3. Sinon：一种测试替身库f
4. Istanbul：一种代码覆盖率库

## 二. 相关代码
- https://github.com/jstevenperry/IBM-Developer/tree/master/Node.js/Course

## 三.框架介绍

### 1.  「mocha测试框架」

- 测试 API，用于指定如何编写测试代码
- 测试发现，用于确定哪些 JavaScript 文件是测试代码。
- 测试生命周期，用于定义在运行测试之前、期间和之后发生的事件。
- 测试报告，用于记录运行测试时发生的情况。
- describe() 表示一个任意嵌套的测试用例分组（一个 Description() 可以包含其他 Description()）。
- it() 表示单个测试用例。
- DEMO
``` javascript
const {describe} = require('mocha');
const assert = require('assert');
describe('Simple test suite:', function() {
    before(function() {
        //在组内的任何测试用例之前运行一次
    });
    after(function() {
        //在所有测试用例运行之后运行一次
    });
    it('1 === 1 should be true', function() {
        assert(1 === 1);
    });
});
```
上文代码输出
```
$ cd src/projects/IBM-Developer/Node.js/Course/Unit-9
$ ./node_modules/.bin/mocha test/example1.js
  Simple test suite:
    ✓ 1 === 1 should be true
  1 passing (5ms)
```

### 2. 「chai」断言库
- 这个断言库可以应用在mocha框架中
- 断言风格
1. Assert：assertEqual(1, 1)
2. BDD（行为驱动开发）：expect(1 === 1).to.be.true 或 expect(1).to.equal(1) ``//这种形式的语法被使用的比较多``

- DEMO 
```javascript
const {describe} = require('mocha');
const {expect} = require('chai');
describe('Simple test suite (with chai):', function(){
    it('1 === 1 should be true', function() {
        expect(1).to.equal(1);
    });
});
```
上文代码输出
```
$ cd ~/src/projects/IBM-Developer/Node.js/Course/Unit-9
$ ./node_modules/.bin/mocha test/example2.js
  Simple test suite (with chai):
    ✓ 1 === 1 should be true
  1 passing (6ms)
```
### 3. 「sinon」替身库
- 在测试的时候，做替换使用，类似与java中的when
- 替身类型
    1. 间谍 (spy) 用于包装一个真实函数，以便记录其相关信息，比如它被调用了多少次以及使用哪些参数来调用。
    2. 伪造对象 (fake) 是一个间谍对象，它只会假装是一个真实函数，这样它就可以记录有关该函数的信息。
    3. 模拟对象 (mock) 类似于一个伪造函数，只是使用了您所指定的期望值，例如函数被调用多少次以及使用哪些参数来调用。
    4. 存根 (stub) 类似于间谍，只是用您所指定的行为替换了真实函数
    - 间谍DEMO
    ```javascript
    const {describe, it} = require('mocha');
    const {expect} = require('chai');
    const sinon = require('sinon');
    describe('When spying on console.log()', function() {
        it('console.log() should still be called', function() {
            let consoleLogSpy = sinon.spy(console, 'log');
            let message = 'You will see this line of output in the test report';
            console.log(message);
            expect(consoleLogSpy.calledWith(message)).to.be.true;
            consoleLogSpy.restore();
        });
    });
    ```
    上文代码输出
    ```
    $ cd ~/src/projects/IBM-Developer/Node.js/Course/Unit-9
    $ ./node_modules/.bin/mocha test/example3.js
    When spying on console.log()
    You will see this line of output in the test report
        ✓ console.log() should still be called
    1 passing (7ms)
    ```
    ***
    ⬆️ 测试报告中显示了 console.log 输出。这是因为间谍不会替换函数，而只是窥探函数。如果您希望调用真实函数，但需要进行相关断言，就可以使用间谍。您可以在清单 3 中看到这一点，其中的测试规定必须使用精确的 message 来调用 console.log()。
    ***
    - 存根函数
    ```javascript
    const {describe, it} = require('mocha');
    const {expect} = require('chai');
    const sinon = require('sinon');
    describe('When stubbing console.log()', function() {
        it('console.log() is replaced', function() {
            let consoleLogStub = sinon.stub(console, 'log');
            let message = 'You will NOT see this line of output in the test report';
            console.log(message);
            consoleLogStub.restore();
            expect(consoleLogStub.calledWith(message)).to.be.true;
        });
    });
    ```
    上文输出
    ```
    $ cd ~/src/projects/IBM-Developer/Node.js/Course/Unit-9
    $ ./node_modules/.bin/mocha test/example4.js
    When stubbing console.log()
        ✓ console.log() is replaced and the stub is called instead
    1 passing (8ms)
    ```
    ***
    ⬆️ 例中，该消息没有显示在测试报告中。这是因为用存根替换了真正的 console.log()。另须注意，必须在expect() 断言调用之前调用 consoleLogStub.restore()，否则测试报告看上去将不正确。

    挑战问题 2
    为什么在调用任何断言逻辑之前必须在 Sinon 存根上调用 restore() 查看 Sinon 存根文档，看看您是否可以找到原因。
    模拟对象与存根非常相似，所以在这里，我就不再演示如何编写模拟对象了。我们将在本单元后面的内容中再次涉及到模拟对象和存根。
    ***
### 4. 覆盖率测试「Istanbul」
- 有命令行版本

### 5. 「ESlint」代码分析工具
- 可检测的问题
    1. 未声明的变量
    2. 未使用的变量或函数
    3. 过长的源代码行
    4. 格式错误的注释
    5. 缺少的文档注释

## 四. 项目演示
### 1. 初始化项目
### 2. 安装以下包
- Mocha
- Chai
- Sinon
- Istanbul CLI – nyc
### 3. 配置 package.json
- 按照以上步骤创建一个测试项目
    #### 1. 初始化项目：任意创建一个目录，然后添加一个package.json的文件
    ```json
    {
        "name": "logger",
        "version": "1.0.0",
        "main": "logger.js",
        "license": "Apache-2.0",
        "scripts": {
        },
        "repository": {
            "type": "git",
            "url": "https://github.com/jstevenperry/node-modules"
        }
    }
    ```
    #### 2. 安装测试包
    - 安装mocha: npm i --save-dev mocha
    - 安装chai: npm i --save-dev chai
    - 安装sinon: npm i sinon
    - 安装istanbul cli: npm i --save-dev nyc  
        ***
        这将安装 Istanbul 命令行接口（称为 nyc）的最新版本，并将 nyc 保存到 package.json 的 devDependencies 节中。
        ***
        
    #### 3. 配置package.json
    - 现在打开 package.json，并将以下内容添加到 scripts 元素中：
    ```shell
    ## 这个脚本告诉 npm 调用 Istanbul CLI (nyc) 和 Mocha，后者将发现    和  行位于 ./test 目录中的测试。
    "test": "nyc mocha ./test"
    ```
    - 最终的package.json
    ```json
    {
        "name": "logger",
        "version": "1.0.0",
        "main": "logger.js",
        "license": "Apache-2.0",
        "scripts": {
            "test": "nyc mocha ./test"
        },
        "repository": {
            "type": "git",
            "url": "https://github.com/jstevenperry/node-modules"
        },
        "devDependencies": {
            "chai": "^4.1.2",
            "mocha": "^5.2.0",
            "nyc": "^12.0.2",
            "sinon": "^6.1.3"
        }
    }
    ```
    #### 4. 编写测试代码（DEMO）
    - 使用 Mocha 和 Chai 编写测试
    1. 在项目的目录中创建一个test文件夹
    2. 在文件夹中创建一个test-logger.js文件
    ```javascript
    describe('Module-level features:', function() {
        describe('when log level isLevel.TRACE', function() {
            it('should have a priority order lower than Level.DEBUG', function() {
                expect(Level.TRACE.priority).to.be.lessThan(Level.DEBUG.priority);
            });
            it('should have outputString value of TRACE', function() {
                expect(Level.TRACE.outputString).to.equal('TRACE');
            });
        });
    });
    ```
    ****
    该用例中嵌套了两个测试用例： 1. 第一个测试用例可确保 Level.TRACE 优先级属性值低于 Level.DEBUG 的值。 2. 第二个测试用例可确保 outputString 属性是 TRACE。 
    3.在断言 expect().to.be.lessThan() 和 expect().to.be.equal() 中使   用了函数链。函数链在 BDD 风格的断言中很常见，这可提高它们的可读性。只需查  看函数链，就可以清楚地看到每个断言的作用。
    ****
    - 编写存根函数：函数的替身，与java的单元测试中的when函数类似
        1. 例子：
        ```javascript
        //当 实际代码执行 Date.now() 的时候，返回值变成了1111111111
        let dateStub = sinon.stub(Date, 'now').returns(1111111111);
        //启动存根函数
        dateStub.restore();
        ```
    ### 4. 安装 ESLint
    - 安装命令 
    ```
    npm i --save-dev eslint eslint-config-google
    ```
    - 配置并运行 linter：将以下代码复制到package.json中
    ```json
    "eslintConfig": {
    "extends": ["eslint:recommended", "google"],
    "env": {
        "node" : true
    },
    "parserOptions": {
      "ecmaVersion": 6
    },
    "rules" : {
      "max-len": [2, 120, 4, {"ignoreUrls": true}],
      "no-console": 0
    }
    },
    "eslintIgnore": [
        "node_modules"
    ]
    ```
    
    ***
    extends 元素表示要使用的配置数组，在此情况下，是 eslint:recommended 和 google。
    添加 env 元素和值为 true 的 node，便可以使用全局变量，如 require 和 module。
    ***
    ***
    eslintIgnore表示忽略检测
    ***
    - 其他脚本：将以下代码复制到package.json中的script节点中
    ```
    "lint": "eslint .",    //对当前目录中的文件进行静态分析
    "lint-fix": "npm run lint -- --fix",  //常见修复，比如，行中的多余空格，或者在单行注释后无空格
    "pretest": "npm run lint" //在每次执行 npm test 时都会先静态分析代码中是否有不规范的地方
    ```
    ### 5. 演示单元测试
    - 个人创建的DEMO-做参考使用
    https://github.com/zhghx/nodejs-test-demo
    1.  git clone https://github.com/zhghx/nodejs-test-demo.git
    2.  npm install
    3.  npm test



