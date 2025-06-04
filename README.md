# TSCR
为了说明ϵ值如何定义以及其对最终结果的影响分析，我们在实验阶段进行了超参数敏感性分析：我们选取多个ϵ值进行网格搜索，并记录不同阈值下，模型的准确率和推理时间的变化趋势，最终选择了在准确率与推理效率之间取得平衡的ϵ值。
我们将在这里展示超参数实验结果，确保读者可以更清晰地了解ϵ的取值依据和其对实验结果的影响。

![image](https://anonymous.4open.science/r/TSCR-D06E/arcc.png)

图一：在CSQA数据集上不同阈值对模型推理时间和Acc的影响

![image](https://anonymous.4open.science/r/TSCR-D06E/csqa.png)

图二：在ARC-C数据集上不同阈值对模型推理时间和Acc的影响
