## Things not strings
- 语义网络(Semantic Network): 概念与概念之间的关系
    - 缺少标准
    - 改进
        - RDF: `rdf:type`
        - RDFS/OWL: `rdf:type rdfs/owl:Class` `rdfs:subClassOf`
- 语义网技术栈: 万维网中资源, 数据之间的关系
    - 语义网(Semantic Web)
    - 链接数据(Linked Data)
    - Web 3.0
- 知识图谱: 表示知识, 实体之间的关联, 本体作为Schema层, 和RDF数据模型兼容的结构化数据集
    - 本体(ontology): 概念和关系的形式化描述
        - 实例: `www.kg.com/person/1 rdf:type kg:Person`
        - 属性: 
            - 对象属性: `kg:hasBirthPlace`
            - 数据属性: `kg:fullName`
    - RDF(Resource Description Framework): 数据模型
        - 组成
            - Subject: IRI or blank node(`www.kg.com/person/1`) 实例
            - Predicate: IRI(`kg:chineseName`=`http://www.kg.com/ontology/chineseName`) 关系
            - Object: IRI or blank nodes or literals(`罗纳尔多`)
        - RDF序列化: 存储和传输RDF数据
            - RDF/XML
            - N-Triples 
            - Turtle
            ```
            @prefix person: <http://www.kg.com/person/> .
            @prefix place: <http://www.kg.com/place/> .
            @prefix : <http://www.kg.com/ontology/> .

            person:1 :chineseName "罗纳尔多·路易斯·纳萨里奥·德·利马"^^string;
                    :career "足球运动员"^^string;
                    :fullName "Ronaldo Luís Nazário de Lima"^^string;
                    :birthDate "1976-09-18"^^date;
                    :height "180"^^int;
                    :weight "98"^^int;
                    :nationality "巴西"^^string; 
                    :hasBirthPlace place:10086.
            place:10086 :address "里约热内卢"^^string;
                        :coordinate "-22.908333, -43.196389"^^string.

            ``` 
            - RDFa
            - JSON-LD
    - 扩展RDF: RDF无法区分类和对象, 以及定义和描述类的关属性, 缺乏抽象能力
        - RDFS
            - `rdfs:Class`
            - `rdfs:domain`
            - `rdfs:range`
        - OWL: Web Ontology Language
            - 快速, 灵活的数据建模能力
            - 高效的自动推理
            - 属性特征
                - `owl:TransitiveProperty`
                - `owl:SymmetricProperty`
                - `owl:FunctionalProperty`
                - `owl:inverseOf`
            - 本体映射
                - `owl:sameAs`
    - 推理
        - 基于本体的推理
        - 基于规则的推理
- 实践
    - 数据准备: `TMDb`
        - 结构化数据
        - 半结构化数据
        - 非结构化数据
    - 本体建模: 
        - 方法
            - 自顶向下: 领域知识图谱
            - 自底向上: 开放域知识图谱
        - `Protege`: 本体编辑和知识获取
            - 创建类
            - 对象属性
            - 数据属性
        - 关系型数据库转换为RDF
            - 标准
                - Direct mapping: ORM
                - R2RML: `D2RQ`:  SPARQL to SQL
                    - SPAQL endpoint服务与交互
                        - `SPARQLWrapper`
                    - 不支持直接将RDF数据通过endpoint发布到网络上
                    - 不支持推理
        - 查询语句: SPARQL(SPARQL Protocol and RDF Query Language)
            - 协议: 通过HTTP协议在客户端和SPARQL服务器（SPARQL endpoint）之间传输查询和结果
            - 步骤
                - 构建查询图模式，表现形式就是带有变量的RDF
                - 匹配，匹配到符合指定图模式的子图
                - 绑定，将结果绑定到查询图模式对应的变量上
            - 开放世界假定: 知识图谱没有包含的信息是未知的
        - `Apache Jena`
            - `TDB`:存储RDF
            - `rule reasoner`: 规则推理机
            - `Fuseki`: SPARQL服务器
    - KBQA
        - 分词/实体识别
        - 正则表达式做语义匹配
        - SPARQL模板

## 知识图谱
- Web2.0: 用户交互
    - P2P
    - Wiki
    - XML
    - Ajax
- 语义网: 网络内容转化为知识
    - 实现
        - 数据的语义标注和链接
        - 具有语义分析能力的信息搜索引擎
    - 关键技术
        - XML:标准的元数据语法规范
        - RDF:标准的元数据语义描述规范
        - Ontology:客观世界的概念化规范
- 知识图谱: 语义网的基件
    - 模具
        - Ontology
        - Taxonomy
        - Folksonomy
    - 知识库: 有向图
        - 多关系数据
        - 节点: 实体/概念
        - 边:关系/属性
        - 关系事实:`(head, relation, tail)`
    - 知识分类
    - 生命周期
        - 知识体系(表示):建模领域知识结构
            - Ontology Engineering
        - 知识获取: 获取领域内的事实知识
            - 信息抽取
            - 文本挖掘
        - 知识集成: 估计知识的可信度,将碎片知识组装成知识网络
            - Ontology Matching
            - Entity Linking
        - 知识存储/查询/应用:提供高性能知识服务
            - 知识表示:
            - 知识查询语言:
            - 存储/检索引擎:
            - 推理引擎
    - 代表性知识图谱
        - 人工构建:Cyc, WorNet
        - 基于维基百科:DBPedia, YAGO, Freebase, WikiTaxonomy, BabelNet
        - 开放知识抽取:KnowItAll, NELL, Probase
        - 企业知识图谱: 百度知心, 搜狗知立方, Google KG, MS sotori
    - 应用
        - 问答
        - 精准语义搜索
        - 关系搜索
        - 分类浏览
        - 推荐
        - 推理
        - 企业风险评估
        - 中医药知识服务平台
    - 限制和不足
        - 领域限制
        - 时空属性的建模
        - 自动构建
        - 与LOD的集成
    - 展望
        - 新的知识表示模型
        - 新类型的知识图谱
        - 自动构建技术
- 知识表示方法
    - 一阶谓词逻辑表示:个体、谓词、量词、逻辑联结词来表示事物的状态,属性等事实性知识,以及事物间因果关系的规则性知识
    - 产生式规则表示:用规则序列的形式来描述问题的思维过程,形成求解问题的思维模式
    - 框架表示:框架理论为基础发展起来的一种结构化知识表示方式,适用于表达多种类型的知识
    - 脚本表示:用来表示特定领域内一些事件的发生序列
    - 语义网表示:让机器或设备能够自动识别和理解万维网上的内容
        - 语言体系: XML -> RDF -> RDF Schema -> OWL
    - 知识图谱中的知识表示: 实体,关系, 事实
    - 分布式知识表示:符号化的实体和关系在低维连续向量空间进行表示,在简化计算的同时最大程度保留原始的图结构
- Ontology:对于概念、术语及其相互关系的规范化描述,勾画出某一领域的基本知识体系和描述语言
    - Ontology Matching
- 实体识别
    - 中文分词
        - 有词典/无词典
        - 规则/统计
            - HMM
            - 最大熵
    - 命名实体识别: LSTM+CRF
    - 开发域实体识别:给定某一类别的实体实例,从网页中抽取同一类别其他实体实例
- 实体消歧:确定一个实体指称项所指向的真实世界实体
    - 无监督:把所有实体指称项按其指向的目标实体进行聚类
    - 知识库链接: 将实体指称项与目标实体列表中的对应实体进行链接实现消歧
- 关系抽取
    - 人工标注语料+机器学习算法
    - 开放域关系抽取
- 事件抽取:从自然语言文本中抽取出用户感兴趣的事件信息并以结构化的形式呈现出来
- 知识图谱的存储与检索
    - 基于表结构的存储: SQL
    - 基于图结构的存储: SPARQL
- 知识推理
    - 归纳推理
    - 演绎推理
    - 常识推理
    - 基于表示学习的推理
- 知识图谱构建
