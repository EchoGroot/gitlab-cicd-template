# 推送镜像到公司私仓

## 如何使用

修改目标项目中的`.gitlab-ci.yml`，注意不是此项目，是引入此yml的项目

1. 增加 push 阶段
    ```
    stages:
      - push
    ```
2. 引入此`yml`
    ```
    include:
      - project: 'devops/gitlab-cicd-template'
        ref: main
        file: '/common/push-image/.gitlab-ci.yml'
    ```
    + `devops`是此项目在gitlab中的group
3. 配置全局变量，每个项目只配置一个`variables`
    ```
    variables: &global-variables
      # 用于拼接镜像名
      MODULE_PREFIX: user-manage
      # 镜像私仓里的项目
      IMAGE_GROUP: user-manage
    ```
4. 新增一个job，并设置变量，以项目`user-manage`的admin模块举例：
    ```
    push_admin_image:
      extends:
        - .push_image_base
      variables:
        <<: *global-variables
        SUB_MODULE: admin
      needs: [ build_admin_image ]
    ```
    + 如果项目是多模块，就需要为每个模块新增一个这样的job。
    + `needs`：如果`build_admin_image`运行结束，就可以运行当前job，不用等`build_admin_image`所在阶段的所有job运行结束才运行。
