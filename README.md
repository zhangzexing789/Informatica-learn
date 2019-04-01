# PowerCenter 应用架构
## 架构简介
- 数据源
- Domain 域
  是在 PowerCenter 中进行管理的主单元
- repository 存储库
  位于关系数据库中。存储库数据库表包含提取、转换和加载数据所需的指令。
- 服务端
- 客户端
  是一个应用程序，用于定义源和目标、构建具有转换逻辑的映射和 Mapplet，以及创建工作流来运行映射逻辑
- 目标数据库
- Informatica Administrator
  用于管理 Informatica 域和 PowerCenter 安全性的 Web 应用程序。
- 域配置
  域配置是一组存储域配置信息的关系数据库表
- 存储库服务
  接受 PowerCenter 客户端的创建和修改存储库元数据的请求，并在工作流运行时接受集成服务的元数据请求。
- 集成服务
  从源提取数据并将数据加载到目标

从源数据库获取到的源数据，经过PowerCenter Server的处理，将转换的数据给到目标，其中来自元数据的指令将会存储到资料库 repository

## PowerCenter 产品组件
1. server 组件
  - Informatica Service：PowerCenter服务引擎
  - Integration Service：数据抽取、转换、装载服务
  - Repository Service：元数据资料库服务
2. client 组件
  - Administratortion Console：基于Web的管理控制台
  - Repository Manager：资料库内容管理的客户端工具，可向用户和组分配权限并管理文件夹，查看那存储库对象的对象。
  - Desinger：ETL映射流程设计的客户端工具
  - Workflow Manager：ETL任务执行流程设计的客户端工具
  - Workflow Monitor：ETL任务执行监控的客户端工具，可监视每个集成服务计划运行和正在运行的工作流。
  - Mapping Architect for Visio：可创建映射模板
## 系统管理
1. Informatica Server管理
  - http://IP-Address:6008
  - Domain 域管理
  - Node 节点管理
  - Repository Service 存储库管理
    在存储库数据库表中检索、插入和更新元数据，确保存储库中元数据的一致性。
  - Integration Service 集成服务管理
    从存储库中读取工作流信息。集成服务通过存储库服务连接到存储库，以便从存储库中提取元数据。
  - WebService Hub 管理
  - License 管理
2. 资料库内容管理
  -  Repository Manager
  - 目录管理
  - 权限管理
  - 版本管理

## 概念解析
- Source 源
- Target 目标
- Transformation 转换组件
- Mapplet 转换组件集
- Mapping 映射
  就是定义ETL处理过程：按照源系统数据和目标数据之间的映射关系，经过数据的抽取(Extraction)、转换(Transformation)和加载(Loading)等环节方可进入目标库
    - 从数据源读取数据
    - 定义数据清洗转换规则
    - 将数据装载到目标
- Task 任务
  可以执行的最小任务单元
    - Session 运行Mapping 的指令集；
    - Command 运行Shell Scripts 或者OS Command；
    - Email 发送邮件；
    - Control组件：如decision、Event等控制组件
- Connection 链接
- Worklet 任务集
- Workflow 工作流
  ETL工作流，使数据抽取可以基于时间、事件
    - 包含各种Task。
    - 可以配置各个Task的执行流程。
    - 可以配置执行计划Schedule。
- Scheduler 计划任务
  定时启动执行workflow的执行计划
    - 可以针对某一个workflow单独定制执行计划
    - 可以制作可重复利用的执行计划
    - 细化到年、月、星期、天、时、分等粒度。
- Monitor 监控
- 元数据：描述数据的数据，即关于某个数据的信息
## 开发步骤

Designer：1、2、3

Workflow Manager:4、5

Workflow Monitor:6

1. 定义源
    将源数据文件通过 `ODBC` 与源分析器连接，使用 `Informatica Service` 将来源的元数据保存到 `repository`
2. 定义目标
    将目标数据文件通过 `ODBC `与目标分析器连接，使用`Informatica Service` 将目标的元数据保存到 `repository`
3. 创建映射
4. 定义任务
5. 创建工作流
6. 工作流调度监控

## 组件
### 组件类型
- Passive 组件<br>
流入流出组件的行数不发生变化，例如：Expression组件
- Active组件<br>
流入流出组件的行数会发生变化，例如：Aggregator组件
### 组件列表
- Source Qualifier: 从数据源读取数据
- Expression: 行级转换
- Filter: 数据过滤
- Sorter: 数据排序
- Aggregator: 聚合
- Joiner: 异构数据关接连接
- Lookup: 查询连接
- Update Strategy: 对目标编辑insert, update, delete, reject
- Router: 条件分发
- Sequence Generator: 序列号生成器
- Normalizer: 记录规范化
- Rank: 对记录进行TOPx
- Union: 数据合并
- Transaction Control: 对装载数据按条件进行事务控制
- Stored Procedure: 存储过程组件
- Custom: 用户自定义组件
- HTTP: WWW组件
- Java:Java自编程组件
### Expression 组件
- Passive组件类型（对于行数无影响）
- 基于行级的数据项赋值、修改、计算
- 在同行记录中可新增、减少数据项
### Filter组件
- Active组件类型（行数发生变化）
- 对流入组件中的记录数据进行过滤
- 类似于关系型数据库Where应用
- 与Source Qualify的过滤功能区别在执行位置上
### Router 组件
- Active组件类型
- 对流入组件中的记录数据按照条件进行分发
- 类似于Java语言中的Switch语句
### Jonier 组件
- Active组件类型
- 对异构数据进行关联（同构关联用Source Qualify组件）
- 类似于SQL 中的Join语句
### lookup 组件
- Active组件类型
- 对Flat File或数据库根据关联的条件进行查询
- 返回符合条件的值，否则为空
- 连接关联与非连接关联
- 类似于SQL 中的Join语句
### Aggregator组件
- Active组件类型
- 对数据集进行聚合
- 聚合分有SUM、AVG、Count、Max、Min……
### Update Strategy组件
- Active组件类型
- 对流过组件的每一条记录赋一个操作标志
- 根据操作标志对目标关系型数据库表生成SQL操作
- 操作标志有DD_INSERT、DD_DELETE、DD_UPDATE、DD_REJECT

## 函数
- 聚合函数
- 字符串函数
- 转换函数
- 数据清洗函数
- 日期函数
- 编码函数
- 财务函数
- 数值函数
- 数学函数
- 特有函数
- 判断函数
- 用户自定义函数
## 任务控制
### 增量抽取实例
### 参数文件控制
### 流程控制实例
### 存储过程实例
### 行列转换实例
### 系统性能调优
