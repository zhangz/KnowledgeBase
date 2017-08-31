## Python Installation
- [Python Setup on Mac](http://sourabhbajaj.com/mac-setup/Python/README.html)
- [Python Dev environment with Visual Studio Code](https://lnx.azurewebsites.net/python-dev-environment-with-visual-studio-code-on-linux/)
- [Python and VS Code](http://mahugh.com/2016/04/27/python-and-vs-code/)

## Dependency Management
- [pipreqs - Generate pip requirements.txt file based on imports of any project](https://github.com/bndr/pipreqs)
- [A set of tools to keep your pinned Python dependencies fresh](https://github.com/jazzband/pip-tools)
- [pipfile](https://github.com/pypa/pipfile)
- [Marriage of Pipfile, Pip, & Virtualenv](https://github.com/kennethreitz/pipenv)
- [pip-review](https://github.com/jgonggrijp/pip-review)
Upgrade all Python packages
```
sudo pip2 freeze — local | grep -v ‘^\-e’ | cut -d = -f 1 | xargs -n1 sudo pip2 install -U
```

## Version/Environment Management
#### Windows
- [How to run multiple Python versions on Windows](https://stackoverflow.com/questions/4583367/how-to-run-multiple-python-versions-on-windows)
- [Python Launcher for Windows](https://docs.python.org/3/using/windows.html?highlight=shebang#python-launcher-for-windows)

#### Linux/Mac
- [使用pyenv管理工作环境](https://zhuanlan.zhihu.com/p/27294128)
- [Python 多版本共存之 pyenv](http://seisman.info/python-pyenv.html)
- [The definitive guide to setup my Python workspace](https://medium.com/@henriquebastos/the-definitive-guide-to-setup-my-python-workspace-628d68552e14)
- [autoenv](https://github.com/kennethreitz/autoenv)
- [Python Virtual Environments - a Primer](https://realpython.com/blog/python/python-virtual-environments-a-primer/)

#### Both
- [最省心的Python版本和第三方库管理——初探Anaconda](https://zhuanlan.zhihu.com/p/25198543)
- [Managing multiple Python environments using Anaconda](http://www.wilsonsayreslab.org/blog/2015/10/26/managing-multiple-python-environments-using-anaconda)
- [My Python Environment Workflow with Conda](http://tdhopper.com/blog/2015/Nov/24/my-python-environment-workflow-with-conda/)
- Conda Configuration
```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --set show_channel_urls yes
```


#### Switch Version
There's a handy way to do this, which comes built right in to the latest Python versions and requires no additional tools or preplanning. You can simply use the `py` launcher instead of `python.exe`, and it will run your highest-numbered Python 2.x version installed. Alternatively, you can use `py -3`, and it will run your highest-numbered Python 3.x version installed.

## Deployment
- [Fabric](http://www.fabfile.org/)
- [Python 进程管理工具 Supervisor 使用教程](http://www.restran.net/2015/10/04/supervisord-tutorial/)
- [Circus - a program that runs and watches processes and sockets](https://github.com/circus-tent/circus)
- [Gunicorn - a Python WSGI HTTP Server](http://gunicorn.org/)

## References
- [The Hitchhiker's Guide to Python](http://docs.python-guide.org/en/latest)
- [Python Tutor](http://www.pythontutor.com)
- [Python API Checklist](http://python.apichecklist.com/)
- [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html)
- [Code Style - The Hitchhiker's Guide to Python](http://docs.python-guide.org/en/latest/writing/style/)
- [Pragmatic Unicode](https://nedbatchelder.com/text/unipain.html)

## Learning
- [Requests源码阅读](https://github.com/zhangz/read_requests)
- [Reading Great Code](http://python-guide-pt-br.readthedocs.io/en/latest/writing/reading/)
- [Let's read code: the python-requests library](https://www.slideshare.net/onceuponatimeforever/lets-read-code)
- [值得看的Python的开源项目有哪些？](https://www.zhihu.com/question/19840137)
