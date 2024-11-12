
# 问题

1. <strong>创建一个新的 Python 虚拟环境后，默认安装的最小包集合包括哪些？</strong>
2. pip: 它是 Python 的标准包管理工具，用于安装和管理其他 Python 包。提供了从 PyPI server 和其他 Python 包索引服务中查找、下载和安装包的基本核心功能
3. setuptools: 用于安装和管理 Python 包的工具。它提供了许多用于打包、分发和安装 Python 项目的工具。许多其他包和工具依赖于 setuptools。
4. wheel: 用于构建和分发 Python 包的工具。Wheel 是一种内置的打包格式，它可以加快包的安装速度，特别是对于包含编译组件的包。

- wheel 包为 setuptools 提供一种扩展支持 wheel 的分发包格式
- 是一种由<strong>PEP 427</strong><strong>， </strong>引入的构建分发包格式，用于取代 egg 分发包格式。

  1. `importlib_metadata` 用途：用于读取已安装包的元数据。它允许你获取已安装包的版本、依赖关系等信息。

1. <strong>python 包的分发格式有哪些？</strong>

| 格式                        | 描述                         | 优点                                                                                    | 缺点                                                       | 使用情况                                  |
| --------------------------- | ---------------------------- | --------------------------------------------------------------------------------------- | ---------------------------------------------------------- | ----------------------------------------- |
| Wheel (.whl)                | PEP 427 引入的二进制分发格式 | - 安装快速<br/>- 不需要编译<br/>- 更安全（无需执行安装时代码）<br/>- 支持多 Python 版本 | • 不支持动态生成文件                                      | 现代标准，广泛使用                        |
| Egg (.egg)                  | 由 setuptools 引入的格式     | - 支持动态生成文件<br/>- 可以包含编译后的 Python 文件                                   | - 安装速度较慢<br/>- 安全性较低<br/>- 不支持多 Python 版本 | 正在被 Wheel 取代，但仍有使用             |
| Source Distribution (sdist) | 源代码分发格式               | - 包含完整源代码<br/>- 适用于所有平台                                                   | - 安装时需要编译<br/>- 安装速度较慢                        | 仍广泛使用，特别是对于纯 Python 包        |
| Windows Installer (.msi)    | Windows 专用安装程序         | • 提供图形安装界面• 支持系统级安装                                                    | • 仅限 Windows 平台<br/>- 需要管理员权限                  | 主要用于 Windows 系统的 Python 发行版     |
| Python Executable (.exe)    | 自解压可执行文件             | • 简化 Windows 上的安装• 可以捆绑依赖                                                 | • 仅限 Windows 平台<br/>- 文件体积较大                    | 用于一些 Windows 上的 Python 应用程序分发 |

- <strong>setuptools 支持哪些打包和分发格式？</strong>

1. Source Distribution (sdist):

- 这是 setuptools 最基本和最广泛支持的格式。
- 创建一个包含所有源代码和必要元数据的 tar.gz 文件。
- 使用 `python setup.py sdist` 命令创建。

1. Egg 格式:

- setuptools 最初引入的格式。
- 可以使用 `python setup.py bdist_egg` 命令创建。
- 虽然还支持，但现在已经不推荐使用了。

1. Wheel 格式:

- 虽然 wheel 包是独立的，但 setuptools 提供了对 wheel 格式的支持。
- 从 setuptools 40.0.0 版本开始，默认包含了 wheel 支持。
- 可以使用 `python setup.py bdist_wheel` 命令创建（需要安装 wheel 包）。

1. Windows Installers (.msi):

- 可以使用 `python setup.py bdist_msi` 命令创建 Windows 安装程序。

1. RPM 包 (用于 Linux):

- 可以使用 `python setup.py bdist_rpm` 命令创建 RPM 包。
- <strong>setuptools 40.0.0 版本开始，默认包含了 wheel 支持，是不是不需要再安装 wheel 包了</strong>

  - 使用场景：

    - 如果您只是想安装 wheel 格式的包，那么只有 setuptools 就足够了。
    - 但如果您想要创建 wheel 格式的分发，您通常还是需要安装 wheel 包。

# 其他工具

- python 推荐的开发工具：[https://packaging.python.org/en/latest/key_projects/#build](https://packaging.python.org/en/latest/key_projects/#build)
- devpi：PyPI server and packaging/testing/release tool
- pyenv:多 python 版本控制管理
- 软件包依赖关系规范：[https://peps.python.org/pep-0508/](https://peps.python.org/pep-0508/)
- Pipenv是Python的依赖管理工具，结合了pip和virtualenv的功能。

# package 开发者模式

- [https://setuptools.pypa.io/en/latest/deprecated/commands.html#develop-deploy-the-project-source-in-development-mode](https://setuptools.pypa.io/en/latest/deprecated/commands.html#develop-deploy-the-project-source-in-development-mode)
- 在开发过程中，修改源包后能够应用到当前环境，省去每次修改后重新构建、安装操作
- 原理：把当前的源码文件夹指向 site-pacakges，所以修改能立即被应用
- 开启开发者模式

```
python setup.py develop
```

- 关闭开发者模式

```
python setup.py develop --uninstall
```

- 开发完后打包分发

# 踩过的坑

## 升级 setuptool 导致安装报错

RUN pip install --upgrade setuptools wheel

14.33 packaging.requirements.InvalidRequirement: Expected matching RIGHT_PARENTHESIS for LEFT_PARENTHESIS, after version specifier

14.33     pytz (>dev)

这 setuptools 到底是个什么？为啥会有这个影响？

# 参考链接

- [https://packaging.python.org/en/latest/overview/#packaging-python-libraries-and-tools](https://packaging.python.org/en/latest/overview/#packaging-python-libraries-and-tools)
