# 打包镜像 job

## 功能

+ eslint代码检查

## 如何使用

+ 配置文件可参考此目录下的。
+ 修改目标项目中的`.gitlab-ci.yml`，注意不是此项目，是引入此yml的项目。
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
            file: '/yarn/eslint/.gitlab-ci.yml'
        ```

    + `devops`是此项目在gitlab中的group
