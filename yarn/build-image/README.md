# 打包镜像 job

## 功能

+ yarn build
+ 构建镜像

## 如何使用

修改目标项目中的`.gitlab-ci.yml`，注意不是此项目，是引入此yml的项目

1. 增加 build 阶段
    ```
    stages:
      - build
    ```
2. 引入此`yml`
    ```
    include:
      - project: 'devops/gitlab-cicd-template'
        ref: main
        file: '/yarn/build-image/.gitlab-ci.yml'
    ```
    + `devops`是此项目在gitlab中的group
3. 配置全局变量，每个项目只配置一个`variables`
    ```
    variables: &global-variables
      # 用于拼接镜像名
      MODULE_PREFIX: user-manage
      SUB_MODULE: dashboard
      # 镜像私仓里的项目
      IMAGE_GROUP: user-manage
    ```
4. 新增一个job，并设置变量，以项目`dashboard`举例：
    ```   
    build_dashboard_image:
      extends:
        - .build_image_base
      variables:
        <<: *global-variables
      image: 172.1.1.1/common/node:14.20.0  
    ``` 
    + image字段默认为`172.1.1.1/common/node:14.20.0`，如果需要其他node版本，请自行替换。
