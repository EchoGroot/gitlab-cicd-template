# 清理镜像

## 功能

1. 清理流水线build_image job生成的镜像，在push-image后使用

## 如何使用

修改目标项目中的`.gitlab-ci.yml`，注意不是此项目，是引入此yml的项目

1. 增加 deploy 阶段
    ```
    stages:
      - clean
    ```
2. 引入此`yml`
    ```
    include:
      - project: 'devops/gitlab-cicd-template'
        ref: main
        file: '/common/clean-image/.gitlab-ci.yml'
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
    clean_admin_image:
      extends:
        - .clean_image_base
      variables:
        <<: *global-variables
        SUB_MODULE: admin
    ```
    + 如果项目是多模块，就需要为每个模块新增一个这样的job
