---
title: "python开发环境搭建 - conda"
date: "2026-03-20"
lastMod: "2026-03-20"
categories: ["it"]
tags: ["python", "conda", "miniforge", "开发环境"]
---

之前是语言学习阶段，使用自带的pip和venv，管理依赖库和开发环境。
实际应用中建议使用conda，对依赖库和上述跨语言库支持更好（较pip和venv）。

## 常规步骤

1. 安装MiniForge
   1. defaults channel，安装包版本更新滞后且不全、网络连接不好
   2. MiniConda有版权风险（个人免费）
   3. Intel版本的macOS的MiniConda官方已不再更新
2. 用conda命令安装python
3. 创建conda环境

## 下载安装Miniforge

对于使用macOS + zsh环境，与官方安装文档有些许不同，安装过程差异如下：

```shell
source <path to conda>/bin/activate

# Initialize all currently available shells.
conda init --all
```

基本配置

```shell
# 配置通道（国内镜像源）
conda config --add channels defaults
conda config --add channels conda-forge
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/

# 设置严格优先级
conda config --set channel_priority strict

# 显示下载来源
conda config --set show_channel_urls yes

# 查看当前配置
conda config --show channels
```

常用操作

```shell
conda create -n <env> [python=3.12] [numpy pandas...]

# 根据 requirements.txt 创建环境
conda create -n my_project python=3.14.5 --file requirements.txt

# 激活环境 by env name
conda activate <env>
# 激活环境 by env path
conda activate /Users/chris/.local/share/mamba/envs/mcp-mysql

conda env list

conda install <pkg>
pip install <pkg>

conda deactivate

conda remove -n <env> [--all]

# 删除环境 by env name
conda env remove --name <env name>
# 删除环境 by env path
conda env remove -p /Users/chris/.local/share/mamba/envs/mcp-mysql

# 仅导出包列表，没有完整环境描述，适合快速备份或查看当前环境包信息
conda list --export > env.txt
conda create -n <env> --file env.txt

# 导出完整的环境，适合环境重建和分享
conda env export > env.yml
conda env create -f env.yml
```

安装conda后导致zsh启动变慢的问题

```shell
# 设置conda延迟启动，只有在使用时加载环境
# 打开.zshrc，找到conda initialize这一段：
# 实现原理：将原内容做成函数，设置conda别名

# >>> conda initialize >>>
lazy_load_conda() {
  unalias conda 2>/dev/null
  # !! Contents within this block are managed by 'conda init' !!
  __conda_setup="$('/Users/chris/conda/bin/conda' 'shell.zsh' 'hook' 2> /dev/null)"
  if [ $? -eq 0 ]; then
      eval "$__conda_setup"
  else
      if [ -f "/Users/chris/conda/etc/profile.d/conda.sh" ]; then
          . "/Users/chris/conda/etc/profile.d/conda.sh"
      else
          export PATH="/Users/chris/conda/bin:$PATH"
      fi
  fi
  unset __conda_setup

  conda "$@"
}
alias conda="lazy_load_conda"
# <<< conda initialize <<<
```



> https://github.com/conda-forge/miniforge/
>
> https://www.bilibili.com/video/BV1Fm4ZzDEeY/
