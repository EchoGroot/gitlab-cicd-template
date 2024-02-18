# 代码检测 job

检查规则，参照kratos

## 如何使用

修改目标项目中的`.gitlab-ci.yml`，注意不是此项目，是引入此yml的项目

1. 增加 lint 阶段
    ```
    stages:
      - lint
    ```
2. 引入此`yml`
   ```
   include:
   - project: 'devops/gitlab-cicd-template'
   ref: main
   file: '/golang/golangci-lint/1.52.2/.gitlab-ci.yml'
   ```
    + `devops`是此项目在gitlab中的group

## 本地运行 lint

流水线触发比较耗时，可以先在本地运行 lint，解决完问题再提交代码。

1. 安装 golangci-lint
    ```
    go install github.com/golangci/golangci-lint/cmd/golangci-lint@v1.48.0
    go install mvdan.cc/gofumpt
    ```
2. 创建配置文件`.golangci.yml`
    + 内容为：golang/golangci-lint/lint-rule.yml 文件里第7行 ~ 倒数第二行之间的内容。
3. 运行 lint
    ```
    golangci-lint run -v --config <.golangci.yml文件路径> 
    ```
    + **可以不使用`--config <.golangci.yml文件路径> `**，golangci-lint 会从当前目录开始向上查找`.golangci.yml文件`
      ，以及home目录。

## 注意事项

1. 现在统一使用一套 lint 规则，项目里不用单独配置golangci-lint，也就是项目里的`.golangci.yml`非必要，如果有，跑流水线时会被覆盖。
