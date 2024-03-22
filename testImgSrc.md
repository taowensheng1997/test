<div align="center">
  <img src="./static/logo/v2_200.png" width="100">
  <br>
  <br>
  
![metrics](https://metrics.woa.com/git/orange-ci/orange-ci/-/latest/ci/pipeline-as-code)

![metrics](https://metrics.woa.com/git/orange-ci/orange-ci/-/latest/ci/main/cck)
![metrics](https://metrics.woa.com/git/orange-ci/orange-ci/-/latest/testing/unit/coverage)
![metrics](https://metrics.woa.com/git/orange-ci/orange-ci/-/latest/code-review/auto-merge)

![metrics](https://metrics.woa.com/git/orange-ci/orange-ci/-/latest/ci/status/merge_request)
![metrics](https://metrics.woa.com/git/orange-ci/orange-ci/-/latest/ci/status/push)
![metrics](https://metrics.woa.com/git/orange-ci/orange-ci/-/latest/ci/status/tag_push)

 ![metrics](https://metrics.woa.com/git/orange-ci/orange-ci/-/latest/code/vscode)
![metrics](https://metrics.woa.com/git/orange-ci/orange-ci/-/latest/ci/git-clone-yyds)
![metrics](https://metrics.woa.com/git/orange-ci/orange-ci/-/latest/delivery/time)
![metrics](https://metrics.woa.com/git/orange-ci/orange-ci/-/latest/production/cdn-go/status)

</div>

<h2 align="center">What is it</h2>

以 Git 为中心，基于 Docker 生态的 云原生构建。

<h2 align="center">Highlights</h2>

<table>
  <tr>
    <th><h4 align="center">🚀<h4 align="center">Easy-to-Use</h4 align="center"></th>
    <th><h4 align="center">⌨️<h4 align="center">Pipeline-as-code</h4 align="center"></th>
    <th><h4 align="center">🐳</h4 align="center"><h4 align="center">Docker-First</h4 align="center"></th>
  </tr>
  <tr>
    <td width="33%"><sub>开箱即用。Orange-CI 无需用户提供私有机器。只需几步简单的配置即可接入，体验到工具带来的效率提升。</sub></td>
    <td width="33%"><sub>业界最佳实践，流水线配置即代码。</sub></td>
    <td width="33%"><sub>容器为 Orange-CI 架构中第一等公民，执行环境及插件都首要考量原生容器方案支持。</sub></td>
  </tr>
  <tr>
    <th><h4 align="center">💬<h4 align="center">Wework-Based</h4 align="center"></th>
    <th><h4 align="center">☁️</h4 align="center"><h4 align="center">Docker-as-Plugins</h4 align="center"></th>
    <th><h4 align="center">🌐</h4 align="center"><h4 align="center">NAT-Supported</h4 align="center"></th>
  </tr>
  <tr>
    <td width="33%"><sub>原生支持企业微信消息通知，构建进度尽在掌握。</sub></td>
    <td width="33%"><sub>Docker 镜像即插件。适应下一代行业生态，能够实现跨 CI 复用。<a href="https://oci.woa.com/plugins.html">https://oci.woa.com/plugins.html</a></sub></td>
    <td width="33%"><sub>使用基于 NAT 网络模式的容器作为构建环境，对用户构建时使用的端口没有任何特殊限制。</sub></td>
  </tr>
</table>

<!-- <img src="./static/example.gif" alt="example gif"> -->

<h2 align="center">Quick Start</h2>

### 1、Git 仓库授权

[点击 OAuth2 完成授权](https://orange-ci.woa.com/oauth/authorize)

### 2、增加 CI 配置文件

在项目根目录中增加 `.orange-ci.yml` 配置文件。

其描述了当仓库发生一些事件时，`Orange-CI` 应该如何去进行处理。

将配置文件 push 到远程 master

一个简单的配置文件如下：

```yaml
master:
  push:
    - stages:
        - name: echo
          script: echo "hello world"
```

想了解配置文件更多用法请移步 [配置文件](https://oci.woa.com/configuration.html)。

<h2 align="center">云原生</h2>

### Docker image as pipeline Context

```yaml
master:
  push:
    - docker:
        image: node:14
      stages:
        - name: print node version
          script:
            - node -v
            - npm install
            - npm test
```

### Docker volumes as pipeline Cache

```yaml
master:
  push:
    - docker:
        image: node:14
        volumes:
          - /root/.npm:copy-on-write
      stages:
        - name: print node version
          script:
            - node -v
            - npm install
            - npm test
```

### Docker image as job Context

```yaml
master:
  push:
    - stages:
        - name: run with node 10
          image: node:10
          commands:
            - node -v
            - npm install
            - npm test
        - name: run with node 12
          image: node:12
          commands:
            - node -v
            - npm install
            - npm test
```

### Docker image as plugins

```yaml
master:
  push:
    - stages:
        - name: npm publish
          image: plugins/npm
          imports: https://example.com/example/secret/blob/master/npm.yml
          settings:
            username: $NPM_USER
            password: $NPM_PASS
            email: $NPM_EMAIL
            folder: .
```

<h2 align="center">文档</h2>

<https://oci.woa.com>

<h2 align="center">使用数据</h2>

使用情况数据均为透明信息：<https://oci.woa.com/usage>

<h2 align="center">FAQ</h2>

**什么时候发布新版本？**

视 Feature 或者 Bug fix 的重要程度（开发人员的心情）而定。一般来说两天会有一个新版本，也有可能一天有两个新版本。发布的时机就是打 tag 的时机。所以如果需要确认某个新特性或者 Bug 是否被修复，可以去 [tag 页面](https://git.woa.com/orange-ci/orange-ci/-/tags) 或者 [Changelog 记录](https://git.woa.com/orange-ci/orange-ci/blob/master/CHANGELOG.md) 中查询是否有相关的 Commit 记录，如果有，那恭喜，这个特性已经在外网中了。如果还没有，欢迎打赏催更。：）

**怎么使用 Orange-CI 测试环境来验证问题？**

如果有人说让你去测试环境验证一下，那么有两种途径：

第一种：将工蜂 WebHook 的地址由 <http://orange-ci.oa.com> 改为 <http://dev.orange-ci.woa.com> 后再触发构建即可。

第二种：将已有构建日志的域名由 `orange-ci.oa.com` 改为 `dev.orange-ci.woa.com` 后点击 rebuild 即可。

**为什么 Orange-CI 工蜂账号（orangeci） 权限要的这么大，需要 Master 权限？**

在工蜂的 API 设计中，有一些 CI 的基本功能要用到的接口是需要用 Master 权限进行调用的。比如【允许阻塞 `Merge Request`】这个功能。请注意，我们并不保证在授予低于 Master 权限时程序能正常运行，这有可能导致 CI 在运行时出现一些未知错误！

<h2 align="center">Contribute</h2>

我们十分期待您的任何贡献，无论是修复错别字、提 Bug 还是提交一个新的特性。

如果您使用过程中发现 Bug，请通过 [issues](http://git.woa.com/orange-ci/orange-ci/issues) 来提交并描述相关的问题，您也可以在这里查看其它的 issue，通过解决这些 issue 来贡献代码。

如果您是第一次贡献代码，请阅读 [CONTRIBUTING](http://git.woa.com/orange-ci/orange-ci/blob/master/CONTRIBUTING.md) 了解我们的贡献流程，并提交 Merge Request 给我们。

<h2 align="center">Contributors</h2>

Orange-CI 的发展离不开每个人的贡献，感谢你们！🎉

按英文名首字母排序。

<a href="https://gist.woa.com/contributors/orange-ci-orange-ci.svg" alt="contributors" target="_blank">
  <img src="https://gist.woa.com/contributors/orange-ci-orange-ci.svg">
</a>

<h2 align="center">License</h2>

[MIT](./LICENSE)
