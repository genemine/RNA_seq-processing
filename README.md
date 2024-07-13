#  RNA-seq 预处理
RNA-seq是一种分析样本中的转录组，即所有基因的RNA表达情况的数据矩阵。RNA-seq是原始测序数据，为了便后续的分析能够更准确和可靠地进行，我们要对RNA-seq进行预处理。

## 1 RNA-seq预处理

为了提高模型训练的效果，我们需要筛选掉低表达基因，并对筛选后的基因表达数据进行对数转换。以下是具体处理步骤以及处理示例：

### (1) 过滤低表达基因：
   - 对于基因表达量文件（FPKM或TPM，必须是未经对数处理的原始文件），构建一个 P×N 的基因表达矩阵X，其中每一行代表一个基因，每一列代表一个样本。
   - 设基因表达量阈值为t，设基因中表达量大于t的样本比例阈值为r(默认为0.5)。之后对基因表达量矩阵X中的每一个基因计算基因表达指标ER(expression ration)，计算过程如下：
<p align="center">
  <img src="https://latex.codecogs.com/png.latex?ER%20%3D%20\frac{k}{n}" alt="ER = k/N">
</p>
其中k表示该基因中表达量大于t的样本个数，N表示总的样本个数。最终保留ER≥r的基因。

### (2) 数据对数转换：
   - 对筛选后基因表达矩阵中的值进行对数处理。假设第 i 个基因在第 j 个样本的表达量为 $x_{ij}$，则经过处理后的数值为log2($x_{ij}$+1) 。


### (3) 处理示例：
设有一个4×4的基因表达矩阵 X<sub>1</sub>，按照t=1，r=0.5进行数据处理，矩阵如下所示：
<p align="center">
  <img src="https://latex.codecogs.com/png.latex?\begin{bmatrix}1.925&0.629&1.760&0.00\\1.881&0.863&1.886&1.784\\1.317&0.790&0.683&1.750\\0.783&0.039&1.535&0.181\end{bmatrix}" alt="矩阵">
</p>
第一步筛选低表达基因：先计算各个基因的ER指标得到基因ER指标矩阵Y，结果如下：
<p align="center">
  <img src="https://latex.codecogs.com/png.latex?\begin{bmatrix}0.25\\0.75\\0.5\\0.25\end{bmatrix}" alt="矩阵">
</p>
然后根据Y筛选掉低表达基因，筛选后的基因表达矩阵为 X<sub>2</sub>，结果如下：
<p align="center">
  <img src="https://latex.codecogs.com/png.latex?\begin{bmatrix}1.925&0.629&1.760&0.00\\1.881&0.863&1.886&1.784\\1.317&0.790&0.683&1.750\end{bmatrix}" alt="矩阵">
</p>
第二步数据对数转换：对X2进行对数处理后得到最终的表达量矩阵 X<sub>3</sub>，结果如下：
<p align="center">
  <img src="https://latex.codecogs.com/png.latex?\begin{bmatrix}1.549&0.704&1.465&0.00\\1.527&0.897&1.529&1.477\\1.212&0.840&0.751&1.460\end{bmatrix}" alt="矩阵">
</p>


## 2 公共的基因表达数据库

构建基因表达矩阵（其中行代表基因，列代表样本或条件，每个单元格中的值表示相应基因在相应样本中的表达量，通常是原始的非负数值）需要基因表达量数据，基因表达量数据可以从以下渠道获取：
* Genome Expression Omnibus (GEO)：GEO是一个由美国国立生物技术信息中心（NCBI）维护的数据库，提供了大量的基因表达数据，包括微阵列和RNA测序数据。研究人员可以上传和下载基因表达数据，以进行各种分析。

* The Cancer Genome Atlas (TCGA)：TCGA是一个由美国国立癌症研究所（NCI）和国立人类基因组研究所（NHGRI）支持的项目，它提供了多种癌症类型的基因表达数据，包括RNA测序数据。这些数据可用于癌症研究和分析。

* ArrayExpress：ArrayExpress是由欧洲生物信息研究所（EBI）维护的数据库，提供了基因表达数据，包括微阵列和RNA测序数据。它包括了来自各种生物体和实验条件的数据。

* GTEx (Genotype-Tissue Expression)：GTEx项目提供了来自多个组织的正常人体组织的基因表达数据。这对于研究基因在不同组织中的表达模式非常有用。

* Single Cell Expression Atlas：这个资源提供了单细胞RNA测序数据，允许研究人员探索单个细胞水平的基因表达。

* NCI Genomic Data Commons (GDC)：GDC是一个综合性的数据存储和分析平台，包括了多个癌症项目的基因表达数据，如TCGA。

* ENCODE (Encyclopedia of DNA Elements)：ENCODE项目提供了大量的基因调控和表达数据，包括RNA测序数据，有助于理解基因功能和调控。



