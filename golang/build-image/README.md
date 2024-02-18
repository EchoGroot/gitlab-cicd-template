# 打包镜像 job

## 功能

+ 构建二进制可执行文件
+ 构建镜像

## 如何使用

修改目标项目中的`.gitlab-ci.yml`，注意不是此项目，是引入此yml的项目

1. `Dockerfile`放到项目根目录
2. 增加 build 阶段
    ```
    stages:
      - build
    ```
3. 引入此`yml`
    ```
    include:
      - project: 'devops/gitlab-cicd-template'
        ref: main
        file: '/golang/build-image/.gitlab-ci.yml'
    ```
    + `devops`是此项目在gitlab中的group
4. 配置全局变量，每个项目只配置一个`variables`
    ```
    variables: &global-variables
      # 用于拼接镜像名
      MODULE_PREFIX: user-manage
      # 镜像私仓里的项目
      IMAGE_GROUP: user-manage
    ```
5. 新增一个job，并设置变量，以项目`user-manage`的admin模块举例：
    ```   
    build_admin_image:
      extends:
        - .golang_build_image_base
      variables:
        <<: *global-variables
        SUB_MODULE: admin
    ```
    + 如果项目是多模块，就需要为每个模块新增一个这样的job
