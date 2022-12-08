---
title: Anaconda Instructions
tags:
  - Anaconda
date: 2022-06-04 08:40:50
description:
cover: https://github.com/Monaxi/Mona-study/blob/main/blog/wallhaven-wqgr9x_1920x1080.png?raw=true
top_img: https://github.com/Monaxi/Mona-study/blob/main/blog/cover2.png?raw=true
---

# Anaconda创建环境

如果我想创建python=3.9版本的环境，并取名为py39，可以：

```python
conda creat -n py39 python=3.9
```



# Anaconda激活环境

如果我想激活刚刚创建的py39环境，可以：

```python
conda activate py39		(conda4以前的版本是source activate py39)
```



# Anaconda退出环境

如果我想退出所在环境，可以：

```python
conda deactivate		(conda4以前的版本是source deactivate)
```



# Anaconda删除环境

**NOTE**：不要轻易删除环境！！！

```python
conda remove -n py39 --all
```

