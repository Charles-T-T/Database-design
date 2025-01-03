# **实验1：mini-pixels环境部署和TPCH测试** 

:man_student: Charles

## 实验描述

本地部署 `mini-pixel` 开发环境，并对其进行TPCH测试。

## 实验过程

1. 准备实验代码：

   ```bash
   # fork项目并clone代码
   git clone https://github.com/Charles-T-T/mini-pixels.git
   # 拉取submodule
   cd mini-pixels
   make pull
   # 配置环境变量
   echo 'export PIXELS_HOME=$PWD' >> ~/.bashrc
   echo 'export PIXELS_SRC=$PWD' >> ~/.bashrc
   source ~/.bashrc
   # 编译
   make -j$(nproc)
   ```

   开发环境部署完毕：

   <img src="./images/image-20241015150032500.png" alt="image-20241015150032500" style="zoom:50%;" /> 

2. 准备实验数据：

   ```bash
   # 下载数据并解压
   wget http://10.77.110.75/pixels/pixels-tpch-1.zip
   unzip pixels-tpch-1.zip
   # 修改测试数据路径为实际 `mini-pixels路径
   cd pixels-duckdb/benchmark/tpch/pixels/
   vim pixels_tpch_template.benchmark.in
   # in vim
   :%s#/data/9a3-02/tpch-1#/home/charles/study/db_dev/mini-pixels#g
   ```

3. 进行TPCH测试（见实验结果）。

## 实验结果

1. 运行 `pixels reader` 测试：

   <img src="./images/image-20241015151732551.png" alt="image-20241015151732551" style="zoom: 50%;" /> 

2. 执行一条查询：

   <img src="./images/image-20241016195759751.png" alt="image-20241016195759751" style="zoom:50%;" /> 

3. 执行整个benchmark：

   ```bash
   python3 run_benchmark_simple.py --dir benchmark/tpch/pixels/tpch_1/
   ```

   <img src="./images/image-20241015232134121.png" alt="image-20241015232134121" style="zoom:50%;" /> 

## 遇到的问题及解决方案

执行一条查询时，出现报错：

```bash
DirectUringRandomAccessFile::RegisterBuffer: register buffer fails
```

查询资料得知这应该是在使用 `io_uring` （Linux 内核中一种较新的异步 I/O 接口）进行高性能 I/O 操作时发生的。由于我的开发环境是WSL2，对此不完全兼容，改为在虚拟机上操作，成功解决。