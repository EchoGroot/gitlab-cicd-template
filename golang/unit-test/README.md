# 单元测试、检查数据竞争 job

## 如何使用

修改目标项目中的`.gitlab-ci.yml`，注意不是此项目，是引入此yml的项目

1. 增加 test 阶段
    ```
    stages:
      - test
    ```
2. 引入此`yml`
    ```
    include: 
      - project: 'devops/gitlab-cicd-template'
        ref: main
        file: '/golang/unit-test/.gitlab-ci.yml'
    ```
    + `devops`是此项目在gitlab中的group
