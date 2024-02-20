![](https://bed.yuyy.info:8088/i/202402201443360.png)

# gitlab-cicd-template

gitlab ci/cd 公共脚本仓库示例。包含yarn和golang的代码检查、单元测试、构建镜像、部署到k8s等功能的ci/cd脚本。其他项目可以通过include引入使用。

> 详细介绍请见[Gitlab CI/CD 实践七：公共脚本仓库](https://yuyy.info/?p=2047)

## 公共脚本仓库带来的好处

+ 其他项目使用 gitlab ci/cd 时，只需引用公共脚本仓库里的脚本，设置对应参数即可。隐藏具体实现细节，减轻使用者的心智。
+ 流水线脚本可能会进行改动、优化，只需改动公共脚本仓库即可，避免影响所有使用该脚本的仓库。

## 使用方式

+ 根据需求选择所需job，大部分job都有readme，包含使用说明等。
+ 完整使用可参照[Gitlab CI/CD 实践四：Golang 项目 CI/CD 流水线配置](https://yuyy.info/?p=1946)

## 注意事项

+ 此仓库的脚本被大量引用，修改时一定要注意向后兼容。
+ `172.1.1.1`是镜像私仓地址，相关基础镜像定义在[dockerfile仓库](https://github.com/EchoGroot/dockerfile)中
