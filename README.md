## Response to Reviewer zT6P
Thank you for recognizing the contributions of our work and for your detailed review. We are pleased that you find the proposed two-stage commonsense reasoning method based on retrieval-augmented generation (TSCR) to be an innovative solution. We greatly appreciate your comments regarding the “concise implementation details of the TSCR method” and the “lack of necessary technical specifications in Knowledge Base Construction.” We take these concerns seriously and will make the following revisions in the updated manuscript:

### Q1: the implementation details of TSCR method appear somewhat brief:
If the paper is accepted, we will include the following implementation details of the TSCR method in the revised version, we will include more comprehensive implementation details of TSCR in the revised version. As illustrated in Figure 1, the TSCR method comprises three main steps: relevant context retrieval, small model reasoning, and large model reasoning.In the context retrieval stage, TSCR constructs a knowledge base from Wikipedia consisting of 6.28 million articles, using a dual-level index structure at both the article and sentence levels. Specifically, the system first performs article-level semantic retrieval using Faiss, leveraging the lead sentence (summary) of each article to accelerate matching. Then, it splits the relevant articles into sentences and builds a sentence-level index to extract the most relevant context for either answering the question directly or serving as a prompt for the large language model.In the reasoning stage, TSCR adopts a two-stage strategy to enhance efficiency and introduces a confidence threshold ϵ to dynamically determine whether to invoke the large model. In the first stage, a small model attempts to answer the question; if its maximum confidence exceeds ϵ, the result is returned directly. Otherwise, the instance is marked as a “difficult case” and passed to the large model, which outputs only the index of the selected option as the final answer. This strategy ensures high accuracy while significantly reducing the usage of the large model and saving inference costs.

### Q2: Lack of Technical Specifications in Knowledge Base Construction:
We acknowledge that this aspect was not clearly explained in the original manuscript, and we will supplement it as follows:
For data sources, we utilize the open-source Wikipedia data dump as the base corpus. During preprocessing and normalization, we use WikiExtractor to clean the raw data, removing non-content elements such as hyperlinks, templates, and footnotes, while preserving the structured paragraphs to ensure corpus quality.For vector representation of the knowledge base, we compute embeddings using Sentence Transformers and construct dual-level indices (article-level and sentence-level) with Faiss. We also incorporate summary sentences (i.e., the first sentence of each article) during indexing to improve retrieval efficiency.For context selection, we specify Top-k articles and Top-k sentences for each query, and rank them based on semantic similarity to ensure that the retrieved context is both relevant and comprehensive for model input.


## 回复Reviewer bQED

感谢您对本工作的审阅并提出宝贵意见，您所提出的问题非常有见解，为本论文的完整呈现提供了切实帮助，如果论文被录用，我们将在修订稿中进行如下修改：

### 问题1：本文所提出的TSCR方法为：先用小预训练模型优先进行推理，再对置信度不足的样本用大语言模型兜底。整体实现方案本质上更像是对现有技术的组合优化，且基于置信度进行样本划分也已是工业界常见做法，TSCR 并没有体现出太大的创新性；

本文所提出的TSCR方法不仅仅是将“基于置信度划分样本”作为一种常见思路直接应用，而是在此基础上结合了检索增强生成的策略，利用上下文信息的语义相关性补全推理链路，并在多数据集实验中展示了其即插即用的适配能力以及大模型和小模型的合理配合，在问答场景中极大地提升了推理的准确性，我们将在修订版中更详细地阐述这一部分的创新点和差异化贡献。

### 问题2：在“实验结果与分析”部分，作者选取传统方法和小型预训练模型作为基线；而在推理效率分析时，却仅对比大语言模型，这种不一致的基线选择可能影响读者对TSCR方法优势的客观判断，建议采用统一的对比基准以增强结论的可信度；

我们确实在推理效率分析部分仅与LLM进行了对比，而未与小模型等基线统一进行对比。对此部分，本工作的主要目标之一是通过阈值决定是否调用大模型，以此在保持准确率的前提下减少对大模型的依赖，从而达到推理效率的提升。由于小模型推理部分本身在绝大多数问题中可直接完成推理，在效率分析中如果纳入小模型单独推理的对比，可能会掩盖我们想强调的“大模型使用频率降低”这一核心创新点。换句话说，我们希望突出展示本文提出的阈值机制在大模型层面如何有效降低整体推理时间及计算资源消耗，而不仅仅是和小模型的直接速度对比。
在修订版中，我们会更明确地解释效率分析的这一立意，确保读者能够理解这种单一对比选择的合理性。

### 问题3：如文章所述，阈值ϵ是一个重要的超参，但作者在汇报结果时并没有阐述如何定义ϵ，也未分析ϵ对实验结果的影响，这降低了实验的可信度。

感谢审稿人指出阈值ϵ的设定问题。阈值ϵ的设定确实是TSCR方法中影响模型推理性能和效率平衡的关键因素。关于这一点，我们在文章中已在4.4节通过“相同难度阈值下不同数据集上的难题数量对比”实验做了部分讨论，以验证阈值在不同数据集下划分难易题目的可行性。

同时，我们在实验阶段进行了超参数敏感性分析：我们选取多个ϵ值进行网格搜索，并记录不同阈值下，模型的准确率和推理时间的变化趋势，最终选择了在准确率与推理效率之间取得平衡的ϵ值（CSQA选取0.35，ARC-C选取0.55）。我们将在修订版中系统性地补充这一部分超参数实验结果，确保读者可以更清晰地了解ϵ的取值依据和其对实验结果的影响，进一步增强实验的可信度和可复现性。

![image](https://anonymous.4open.science/r/TSCR-E723/csqa.png)

图一：在CSQA数据集上不同阈值对模型推理时间和Acc的影响

![image](https://anonymous.4open.science/r/TSCR-E723/arcc.png)

图二：在ARC-C数据集上不同阈值对模型推理时间和Acc的影响

## 回复Reviewer k7GK

感谢您对我们工作的肯定，并提出了建设性的宝贵意见，如果论文被录用，我们将在修订稿中进行如下修改：

### 问题1：TSCR方法中，不同步骤之间的关系应该阐述。

感谢您提出的宝贵意见。我们已根据您的建议，对 TSCR 方法的各个步骤之间的逻辑关系进行了更为清晰的阐述，具体如下：TSCR方法包括三个步骤，如图1所示：相关上下文检索、小模型推理和大模型推理。在相关上下文检索部分，TSCR基于Wikipedia构建了一个包含6.28M篇文章的知识库，采用文章级与句子级双重索引结构。具体而言，系统先通过Faiss完成文章级语义检索，利用首句摘要加速匹配；再对相关文章分句，构建句级索引，提取与问题最相关的上下文，用于回答或作为Prompt输入大语言模型。在推理部分，TSCR采用两阶段策略提升效率，并设置置信度阈值ϵ用于动态决策模型调用。第一阶段由小模型进行推理，若其最大置信度超过ϵ，则直接输出结果；否则将其标记为“困难问题”，由大模型接续处理，仅输出选项索引作为答案。该策略在保证准确率的同时，有效降低大模型使用频率，节省推理成本。

### 问题2：大模型与小模型的区别缺少解释。

我们将在修订版中对两者的区别进行补充说明。具体而言，我们以参数规模是否超过 1B（10亿）作为划分标准，将参数量低于1B的模型定义为“小模型”；而参数量超过1B的模型定义为“大模型”。

### 问题3：数据集统计中的数值缺少单位。

感谢您指出我们在数据集统计表中数值缺乏单位的疏漏。我们将在修订稿中对“表2.数据集详细信息表”进行补充，明确每一项统计数据的单位。
具体修改包括：“训练集大小”、“验证集大小”、“测试集大小”单位为“条”（即样本条数）；“选项数量”单位为“个”（每个问题的候选选项数）；“问题平均长度”、“选项平均长度”单位为“词”。

### 问题4：实验表中，/表示什么？缺少相应的解释。

感谢您指出我们未对实验表中“/”未作说明的疏忽，对此，我们将在修改稿中进行改正，详细说明在本论文中，“/”表示原论文未在该数据集上进行实验。
