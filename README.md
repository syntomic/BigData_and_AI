## 内容介绍
- [数据分析与挖掘](Data/analysis.md): 有目的地从现有数据中提取数据的模式和模型
    - [python](Data/basic.md): 轻松集成C,C++以及Fortran代码, 不仅适用于研究和原型构建,同时也适用与构建生产系统
        - Numpy: 科学计算基本包
        - Pandas: 处理结构化数据
        - Matplotlib: 绘制图表
        - Ipython: 交互和探索性计算环境
        - Scipy: 解决科学计算中各种标准问题域的包的集合
- [大数据](BigData/bigdata.md): 数据量大 数据类型繁多 处理速度快 价值密度低
    - 过程
        - 数据采集
        - 数据存储
        - 数据计算: GFS + MapReduce
            - 离线运算
            - 实时运算
        - 数据应用
    - 相关技术
        - 云计算: 通过网络提供可伸缩的, 廉价的分布式计算能力
        - 物联网: 物物相连的互联网
    - 工具
        - Hadoop: 分布式计算平台 HDFS + MapReduce
        - Spark: 快速 通用引擎 RDD
        - Flink: 无边界和有边界数据流上进行有状态的计算
    - 隐私: **Personal Identifiable Information**
- AI: 把人类从无意义的、重复性的思维模式中解放出来, 人机协作
    - 方法
        - 基于规则
        - [统计学习](AI/tech/NLP/statistics.md)
        - [机器学习](AI/tech/ML/ml.md): 特征表示+目标函数+优化算法
        - [深度学习](AI/tech/DL/dl.md): 分层表示学习, 连续的几何空间变换
    - 软件
        - [Scikit-learn](AI/tech/ML/sklearn.md): 通用机器学习包
        - 深度学习框架
            - [TensorFlow](AI/tech/DL/tensorflow.md): 工业界
            - [Pytorch](AI/tech/DL/hello_pytorch.py): 学术界
    - 计算架构
        - GPU: CUDA
        - FPGA: 云端AI加速
        - ASIC
            - TPU
            - NPU
    - [具体任务](https://github.com/syntomic/BigData_and_AI/tree/master/AI/)
        - 语音
            - 语音助手
        - CV(计算机视觉)
            - 自动驾驶
        - [NLP](https://github.com/syntomic/BigData_and_AI/tree/master/AI/task/NLP/)(自然语言处理)
            - [深度学习演变](AI/task/NLP/survey.md)
            - [知识图谱](AI/task/NLP/KG.md): 知识的表达方式及推理
            - 机器翻译
            - 信息检索
            - [推荐系统](AI/task/NLP/RS.md)
        - 强化学习
            - Alpha Zero
    - 工程实践
        - [美团机器学习实践](AI/project/meituan.md)
