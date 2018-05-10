- subHealthyModel
-     项目名称：亚健康预警模型工程（17个）
-     建设人：A03770
-     建设时间：2017年
-     关键：模型执行数据均取自庚顿数据库
  - Analysor
  -     分析筛选功能模块
    -  *.py
        -     各个预警模型的结果筛选导出，大体步骤如下
        -     1、query函数输入3个参数：机组号，开始时间，结束时间
        -     2、得到该模型该机组在这段时间内是否有异常结果，筛选是指比如连续多长时间以上才视为异常
        -     3、导出为excel文件（查看路径是否设置正确），以及画图（设置路径导出）
  - config
    - path
      -     FARM_LIST:需要计算的风场的清单,0-不计算，1-计算
      -     风场.xlsx:风场所有机组的mysql数据路径
    - tag
      -     风场.xlsx:风场所有机组在庚顿数据库中的字段和模型二维关联列表
  - DB 
    -     early_warning:sqlite3数据库，存储run_record和各模型的异常中间结果，其中run_record为记录机组和模型运行的时间，方便在后续启动之前时知晓机组的某个模型执行到什么时间了，然后从该时间继续执行，以模型为名的表记录的是模型执行的中间结果
  - getDatasFromGolden
    -     python通过pyjnius包，从java接口获取数据的函数，lib下的jar包即为java的接口
  - Lib
    -     获取庚顿数据库的java的接口，必须有
  - Server 
    -     预警模型定时分析执行模块
    -  CronCalZone.py
    -     定时实现17个模型的入口，平时只需启动这个函数，17个模型均是定时任务
    -  *.py
    -     各个预警模型的具体实现，大体步骤如下
    -     1、获取config下需要执行计算的风场、机组及其机组的标签点
    -     2、获取机组模型计算的最后时间，若较为遥远，默认只运行最近一周或10天
    -     3、从庚顿库查询“插值”类型数据
    -     4、执行模型具体算法或规则
    -     5、模型异常的机组和时间以及其它附属的统计量导出到以模型命名的表中
