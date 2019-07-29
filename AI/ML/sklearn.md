## scikit-learn 设计
- 一致性：所有对象的接口一致且简单
    - 估计器(estimator)：基于数据集对一些参数进行估计的对象
        - `fit()`方法   
    - 转换器(transformer)：一些估计器也可以转换数据集
        - `transform()`方法
        - `fit_transform()`方法(经过优化) 
    - 预测器(predictor)：一些估计器可以根据给出的数据集做预测
        - 预测：`predict()`
        - 评价：`score()`
- 可检验
    - 超参数：`estimator.strategy`
    - 模型参数：`estimator.statistics_`
- 类不可扩散
    - 数据集：Numpy数组或SciPy稀疏矩阵
    - 超参数：Python字符串或数字
- 可组合：尽可能使用现有的模块
    - 流水线(Pipeline)：转换器序列+估计器
- 合理的默认值

## 快速上手
- 机器学习：问题设定
    - 监督学习
        - 回归
        - 分类
    - 非监督学习
        - 聚类
        - 密度估计
        - 降维
- 导入数据：`from sklearn import datasets` + `digits = datasets.load_digits()`
    - 数据集：`X = digits.data`
        - 形状：`(m_samples, n_features)`
    - 标签集：`y = digits.target`
- 学习和预测：
    - 选择模型：`from sklearn import svm` + `clf = svm.SVC(gamma=0.001, C=100.)`
    - 学习：`clf.fit(X[:-1], y[:-1])`
    - 预测：`clf.predict(X[-1:])`
- 模型存储与读取
    - Python内建模块`pickle`：`s = pickle.dumps(clf)` + `clf = pickle.loads(s)`
    - 较大数据`joblib`：`joblib.dump(clf, 'filename.joblib')` + `clf = joblib.load('filename.joblib')`
- 约定
    - 输入数据类型被转化为`float64`
    - 回归标签类型被转化为`float64`，分类标签不变
- 修改和更新参数：`clf.set_params(kernel='linear').fit(X, y)`
- 多类别vs多标签
    - 多类别：`clfs = sklearn.multiclass.OneVsRestClassifier(estimator=SVC(gamma='scale',random_state=0))`
    - 多标签：`y = sklearn.preprocessing.(Multi)LabelBinarizer().fit_transform(y)`
## 一个完整的机器学习项目
- 通用设置
    - 项目配置：`import os`
    - 画图设置：`%matplotlib inline`
    - 存储图像：`def save_fig()`
- 获得数据
    - 自动化采集，编写函数：`def fetch_data()`
    - 使用Pandas加载数据：`data = pd.read_csv()`
    - 快速查看数据结构：`data.head()` `data.info()` `data.describe()`
    - 创建测试集
        - 简单随机：`sklearn.model_selection.train_test_split(data, test_size=0.2)`
        - 分层抽样：`sklearn.model_selection.StratifiedShuffleSplit(n_splits=1, test_size=0.2).split(X, y)`
    - 使数据回到初始状态
- 数据探索和可视化，发现规律
    - 创建训练集副本
    - 可视化
    - 查找关联：`data.corr()`
    - 属性组合实验
- 为机器学习算法准备数据：编写函数尽量实现自动化
    - 将预测值和标签数据分开
    - 数据处理
        - 处理缺失值：
            - 选择策略：`imputer = sklearn.impute.SimpleImputer(strategy="median")` 
            - 训练：`imputer.fit(data_num)`
            - 转换：`X = imputer.transform(data_num)`
        - 处理文本和类别属性
            - 转换器：`encoder = sklearn.preprocessing.OrdinalEncoder()`
            - 训练和转换：`data_cat_encoded = encoder.fit_transform(data_cat)`
            - OneHoTEncoder：`sklearn.preprocessing.OneHotEncoder().fit_transform(data_cat)`
    - 添加而外特征
        - 自定义类
            - 继承基类：`class CombinedAttributesAdder(BaseEstimator, TransformerMixin):`
            - 定义转换方法：`def transform(self, X, y=None):`
            - 建立实例：`attr_adder = CombinedAttributesAdder()`
            - 转换：`extra_attribs = attr_adder.transform(X)`
        - 可用的类
            - 定义函数：`def add_extra_features(X):`
            - 转换器：`extra_attribs = sklearn.preprocessing.FunctionTransformer(add_extra_features)`
            - 转换：`extra_attribs = attr_adder.transform(X)`
    - 特征缩放
        - 归一化(限制到特定范围)：`MinMaxScaler`
        - 标准化(受异常值影响小)：`StandardScaler`
    - 定义流水线
        - 数值流水线
        ```python
        from sklearn.pipeline import Pipeline
        from sklearn.preprocessing import StandardScaler

        num_pipeline = Pipeline([
            ('imputer', SimpleImputer(strategy="median")),
            ('attribs_adder', FunctionTransformer(add_extra_features)),
            ('std_scaler', StandardScaler()),
        ])

        data_num_tr = num_pipeline.fit_transform(data_num)
        ```
        - 总流水线
        ```python
        from sklearn.compose import ColumnTransformer

        num_attribs = data_num_col
        cat_attribs = data_cat_col

        full_pipeline = ColumnTransformer([
            ("num", num_pipeline, num_attribs),
            ("cat", OneHotEncoder(), cat_attribs),
        ])

        data_prepared = full_pipeline.fit_transform(data)

        ``` 
    - 选择和训练模型
        - 在训练集上训练和评估不同模型
            - 误差：`sklearn.metrics.mean_squared_error(data_labels, data_predictions)`
        - 交叉验证：给出一个可能模型列表(二到五个)
        ```python
        from sklearn.model_selection import cross_val_score

        scores = cross_val_score(tree_reg, data_prepared, data_labels,
                         scoring="neg_mean_squared_error", cv=10)
        tree_rmse_scores = np.sqrt(-scores) # Scikit-Learn交叉验证为效用函数越大越好
        ``` 
        - 保存模型
    - 微调模型
        - 网格搜索
        ```python
        from sklearn.model_selection import GridSearchCV
        param_grid = [
            # try 12 (3×4) combinations of hyperparameters
            {'n_estimators': [3, 10, 30], 'max_features': [2, 4, 6, 8]},
            # then try 6 (2×3) combinations with bootstrap set as False
            {'bootstrap': [False], 'n_estimators': [3, 10], 'max_features': [2, 3, 4]},
        ]

        forest_reg = RandomForestRegressor(random_state=42)
        # train across 5 folds, that's a total of (12+6)*5=90 rounds of training 
        grid_search = GridSearchCV(forest_reg, param_grid, cv=5,
                                scoring='neg_mean_squared_error', return_train_score=True)
        grid_search.fit(data_prepared, data_labels)

        grid_search.best_params_
        grid_search.best_estimator_
        pd.DataFrame(grid_search.cv_results_)
        ``` 
        - 随机搜索：`sklearn.model_selection.RandomizedSearchCV()`
        - 集成方法
    - 分析最佳模型和它们的误差
        - 特征重要性：`grid_search.best_estimator_.feature_importances_`
        - 误差原因
    - 用测试集评估系统
        - 测试集误差置信区间
    - 启动、监控、维护系统

## 一些技巧
- 简单正则化:`X_train.astype(np.float64)`
- SVM分类器:`SGDClassifier(loss="hinge", alpha=1/(m*C))`
- 数据不能放进内存:`np.memmap`