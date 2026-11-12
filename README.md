# 县域照护衔接分析

encounters 保存有知识时间的服务记录和转诊，pathways 构造路径、占用和覆盖，waiting_analysis 管理分析定义，exports 提供小群组抑制与归档。

业务观察日与记录可知日分别选择。已接受转诊才组成链。等待、重入院和仍在院记录各有总体口径，不能混为已完成服务的均值。所有患者与机构均为合成身份，结果不用于临床建议。

## 运行

Python 3.11，使用标准库及内置 SQLite，无第三方依赖。无需安装第三方包。 在项目根目录执行，命令不访问公网。演示在临时目录内创建数据，结束后自行清理，不需要预置凭据或另启服务。

```sh
python3 continuity.py demo --fixture fixtures/care.json
```

## 开发检查

测试覆盖当前产品的核心计算、边界输入、事务或版本恢复及异常路径。编译或语法检查可单独运行。

```sh
python3 -m unittest discover -s tests -v
python3 -m compileall -q encounters pathways waiting_analysis exports continuity.py
```

## 容器复现

先在有网络的准备阶段取得运行镜像。之后可在断网容器中复制项目到临时工作区运行，避免向源目录写入缓存。

```sh
docker build -t local/analysis-product .
docker run --rm --network none local/analysis-product
```

具体接口参数可通过主入口的帮助命令查看。数据库、归档和导出文件应存入调用者指定的工作目录，不提交到源码仓库。
