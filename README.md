# @Travis-CI && @GithubAction,你帮我下载文件行吗QvQ

## 前言

此项目是一个**鰯鲫**下午在学校用下载Github下载PicGo时的无力吐槽。

网传加速Github的项目有很多种，包括但不限于：

- Gitee加速克隆 【缺点：账号在异地登陆很有可能风控需要手机验证码（然而人在学校没有手机】
- cnpm加速 【缺点：无法下载Release，且移动网络下载速度鸡贼】
- CloudFlareWorkers加速 【缺点：电信网络下下载速度比百度网盘稍微好些，而且丢包严重】
- jsdelivr加速 【缺点：无法下载>20MB的文件，并且官方屏蔽exe下载】
- 改host 【一句话，麻烦（拒绝任何反驳qwq】

此项目均解决以上问题，但是仍有缺点：

- 第一次配置略麻烦
- 每下载一次文件需要修改一次Github
- 若大文件下载分包较多麻烦

## 开始

### 第一次配置

1. 先Star该仓库 ~~【可选】~~
2. Fork此仓库
3. 修改 `.travis.yml` 14行单引号括起来的链接地址，将其设置为你所需要下载的链接。

> **注意，其中不得包含特殊字符如\&**

> 实际上我可以把它写在travis-ci的变量里，可以避免公布下载链接，但是个人测试，github访问速度比travis-ci快多了，而且travis-ci登陆还是要Github...

4. 进入你的Github个人资料，[新建一个token](https://github.com/settings/tokens) ，权限至少repo读写、profile读【建议全勾】，生成token并复制，妥善保管。

5. 进入 <https://travis-ci.org> 或 <https://travis-ci.com>，用github账号登入，进入dashboard-setting，设置环境变量：变量名字: `GH_TOKEN` ，变量值: 你生成的TOKEN，tigger此项目。

6. 大约一分钟，这个项目就会变绿，如果没有，请检查日志；成功后进入仓库，选择 `gh-pages` 分支，你大概会看到以下文件：

```

gh-pages
  - re\
    - ...
  - torcn.tar.bz2.aa
  - torcn.tar.bz2.ab
  - torcn.tar.bz2.ac
  - torcn.tar.bz2.ad
  
```

原来下载文件在 `re\` 文件夹下，但是jsdelivr禁止下载exe文件且不得大于20MB，所以我们将其分割为20MB的包，下载时请使用以下链接：

```
https://cdn.jsdelivr.net/gh/用户名/仓库名字@gh-pages/分块名字
```

以本仓库为例，下载以下所有链接：

```
https://cdn.jsdelivr.net/gh/ChenYFan-Tester/travis-ci-help-me-download@gh-pages/dl.tar.bz2.aa
https://cdn.jsdelivr.net/gh/ChenYFan-Tester/travis-ci-help-me-download@gh-pages/dl.tar.bz2.ab
https://cdn.jsdelivr.net/gh/ChenYFan-Tester/travis-ci-help-me-download@gh-pages/dl.tar.bz2.ac
https://cdn.jsdelivr.net/gh/ChenYFan-Tester/travis-ci-help-me-download@gh-pages/dl.tar.bz2.ad
https://cdn.jsdelivr.net/gh/ChenYFan-Tester/travis-ci-help-me-download@gh-pages/end.bat
```

剪切以下文件至新建文件夹，运行 `end.bat` 合并所有分块，解压 `dl.tar.bz2` 即可

## 以后...

1. 改 `.travis.yml` 14行单引号括起来的链接地址，将其设置为你所需要下载的链接。
2. 为绕开jsdelivr缓存，请修改16行、17行默认的名字 `dl`【可选】
3. 进入github，gh-pages分支等待一分钟即可


# 已知的问题：

- [x] 下载Release时可能会下载到Github默认的跳转页面【此时为一个html】
  - ~~暂时解决方法：用记事本打开下载的文件，复制跳转链接，用双引号包裹该链接下载~~
  - ~~注意：建议直接下载Source打包，避免跳转至亚马逊云【亚马逊云链接有特殊字符】~~
  - 已修复，采用-L支持重定向

# Todo：

- [ ] 编写GithubAction版本

# 关于

travis-ci集成部署，通过curl下载、tar打包、split切块、git部署至Github、jsdelivr加速下载、windows下copy命令合并。

此脚本可以下载任意直链文件，包括但不限于Github，但是**请勿滥用**。

同样支持GithubAction部署，原理类似，请自行更改

# 许可

MIT
