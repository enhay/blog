---
title: "使用gitlab ci 提升前端代码质量"
tags: ["gitlab", "eslint", "单元测试"]
date: 2019-04-10
draft: false
---
### gitlab
  gitlab是使用Git的仓库管理工具,很多公司都使用其搭建了内网git仓库,除了代码托管外,gitlab 的ci/cd功能也能在保证代码质量方面帮我们做很多事情
### 使用eslint维持一致的代码风格
  保持团队代码风格的一致性是件很重要的事情,由于历史原因js本身百花齐放,比如茴字的四种写法
  ```javascript
    const obj = {
      f1: function (a) {
        return a * 2;
      },
      f2: (a) => {
        return a * 2
      },
      f3: a => {
        return a * 2;
      },
      f4: a => a * 2
    }
```
  如果再配上`连续赋值` `循环闭包`这些眼花缭乱的技巧,新人开项目代码估计想死的新都有.

  js代码规范很多,可以选择流行的airbnb或者根据实际情况内部商定,但要写出符合规范的代码我们还需要代码检测工具的帮助.流行的如 jslint eslint jshint 等, 下面以eslint为例:

  首先我们安装想要的npm依赖包和IDE(比如vscode)插件,
  然后在要检测的代码目录添加配置文件
  ```javascript
  // .eslintrc .json或者.js
  module.exports = {
  "extends": "airbnb-base", //airbnb规则
  "env": {
    "node": true
  }, 
   "rules": {// 规则重写
    "prefer-template": 0,
  }
};
```
然后我们就可以在编辑器或者命令行中执行`eslint [file.js] [dir]` 查看代码是否符合规范
比如:
```javascript
function sum(a, b) {
  const sum = a + b;
  return sum;
}
module.exports = sum;
```
![eslint](http://km.huya.com/uploadfile/editor/0/1/1422.png)

实际开发中如果使用eslint的话,因为eslint插件支持很好,建议把约定的规则封装重新封装,减少安装过程中的依赖问题和对package.json的污染.


### 使用jest编写测试用例
  单元测试的重要性大家都清楚,虽然不写单元测试的理由也很多,但对于基础包还是强烈建议做这件事情,因为这些代码的影响范围是不可控的.

  单元测试框架有很多,下面以jest为例
  ```javascript
  const sum = require('../sum.js');
  test('adds 1 + 2 to equal 3', () => {
    expect(sum(1, 2)).toBe(3);
  });
```

  执行`jest`结果如下
  
  ![jest](http://km.huya.com/uploadfile/editor/0/1/1426.png)

### 使用gitlab ci 控制代码合并
  虽然有了标准和规范,但仍有可能不被执行,不如忽略错误提示,修改代码或不修改相关测试用例,有些细节在Code Review时很难察觉
  这时候我们就需要一个没得感情的机器人`gitlab ci`来帮我们做这件事

  首先修改合并选项

  `Settings -> general -> Merge request settings`
  
  勾选 `Only allow merge requests to be merged if the pipeline succeeds`
  
  然后创建`.gitlab-ci.yml`的配置文件放在项目根目录下

  ```yaml
  #yaml格式更多细节可阅读官方文档
  image: node:latest # 基础镜像 提供node环境

cache:
  key: "$CI_BUILD_REF_NAME" # 缓存相同的分支的node_modules共用
  paths:
    - node_modules/

stages: # 串行 test依赖lint完成
  - lint 
  - test

lint-job:
  stage: lint 
  script:
    - npm install 
    - npm run lint # elint .

test-job:
  stage: test
  script:
    - npm install
    - npm run test # jest
  ```
上述配置在每次代码提交后将自动执行代码检查和单元测试.

现在我们从master(受保护的分支)切出一个新分支,提交示例代码pr到master
[截图]

可以看到在任务未通过前合并按钮是灰色不可点的,也就是说如果代码检查和单元测试没通过(报错等),代码将无法合并.
~~由于git暂未配置runner所以这个ci永远不会被执行~~

gitlab ci 还有很多有趣有用的玩法有机会我们再聊

