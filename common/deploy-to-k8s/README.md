# 部署到K8s

## 功能

1. 替换部署文件里的变量，具体请看此脚本`deploy_to_k8s`的内容。
2. 应用`deploy/$SUB_MODULE/$ENV`下的部署文件

## 如何使用

修改目标项目中的`.gitlab-ci.yml`，注意不是此项目，是引入此yml的项目

1. 增加 deploy 阶段
    ```
    stages:
      - deploy
    ```
2. 引入此`yml`
    ```
    include:
      - project: 'devops/gitlab-cicd-template'
        ref: main
        file: '/common/deploy-to-k8s/.gitlab-ci.yml'
    ```
    + `devops`是此项目在gitlab中的group
3. 配置全局变量，每个项目只配置一个`variables`
    ```
    variables: &global-variables
      # 用于拼接镜像名
      MODULE_PREFIX: user-manage
      # 镜像私仓里的项目
      IMAGE_GROUP: user-manage
      # 部署环境
      ENV: test
    ```
4. 新增一个job，并设置变量，以项目`user-manage`的admin模块举例：
    ```
    deploy_admin_to_test_k8s:      
      extends:                     
        - .deploy_to_k8s_base
      before_script:               
        - K8S_CONFIG=$K8S_CONFIG_192
        - NAME_SPACE=default
      variables:                    
        <<: *global-variables      
        SUB_MODULE: admin          
      needs: [ push_admin_image ]
    ```
    + 如果项目是多模块，就需要为每个模块新增一个这样的job。
    + `K8S_CONFIG_192`：目标k8s的配置，通过gitlab配置变量。
    + `needs`：如果`push_admin_image`运行结束，就可以运行当前job，不用等`push_admin_image`所在阶段的所有job运行结束才运行。
    + 如果该应用需要**部署到多集群**，参照下面的示例设置`K8S_CONFIG`和`NAME_SPACE`
      ```
       - K8S_CONFIG=($K8S_CONFIG_192 $K8S_CONFIG_193 $K8S_CONFIG_194)
       - NAME_SPACE=(default default1 default2)
      ```
        + 按空格分隔
        + `K8S_CONFIG`和`NAME_SPACE`的映射关系，取决于上面两个数组的index，例如`$K8S_CONFIG_192`对应`default`。
