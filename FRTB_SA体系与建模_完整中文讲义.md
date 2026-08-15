# FRTB 标准法：体系、公式、参数与建模完整中文讲义

## 一、讲义定位

本讲义把 FRTB 标准法（FRTB SA）作为一套完整的监管风险模型来讲解，而不是只介绍监管背景或管理意义。课程主线为：

$$
\text{适用范围与账簿边界}
\rightarrow \text{风险因子}
\rightarrow \text{监管敏感度}
\rightarrow \text{风险权重}
\rightarrow \text{相关性聚合}
\rightarrow \text{SBM、DRC、RRAO}
\rightarrow \text{资本与RWA}
\rightarrow \text{应用和验证}.
$$

每一部分都按六个问题展开：

1. 这部分在整个体系中解决什么风险；
2. 公式是什么；
3. 变量和系数分别代表什么；
4. 参数为什么这样进入模型；
5. 如何在系统中计算；
6. 怎样验证和调试。

本讲义不依赖前面的期权、Swaption 或 CDS 定价案例。所有算例都从监管风险记录开始，因此可单独作为 FRTB SA 主课程使用。

> 监管口径说明：本讲义根据用户提供的巴塞尔框架 PDF 编写。附件生成日期为 2026 年 8 月 15 日，其中 MAR21 已注明纳入 2024 年 7 月及 2026 年 3 月发布的 FAQ。正式报送仍须核对适用司法辖区、当地生效日期、国家选择项和机构批准的方法。

---

## 二、附件与课程范围

### 2.1 主讲章节

- [RBC25.pdf](<C:/Users/jinyu/Downloads/RBC25.pdf>)：交易账簿与银行账簿边界；
- [MAR10.pdf](<C:/Users/jinyu/Downloads/MAR10.pdf>)：市场风险术语；
- [MAR11.pdf](<C:/Users/jinyu/Downloads/MAR11.pdf>)：市场风险适用范围及 SA/IMA 关系；
- [MAR12.pdf](<C:/Users/jinyu/Downloads/MAR12.pdf>)：交易台定义和风险管理要求；
- [MAR20.pdf](<C:/Users/jinyu/Downloads/MAR20.pdf>)：完整 SA 的总体结构；
- [MAR21.pdf](<C:/Users/jinyu/Downloads/MAR21.pdf>)：敏感度法 SBM；
- [MAR22.pdf](<C:/Users/jinyu/Downloads/MAR22.pdf>)：违约风险资本 DRC；
- [MAR23.pdf](<C:/Users/jinyu/Downloads/MAR23.pdf>)：剩余风险附加 RRAO。

### 2.2 比较和体系边界章节

- [MAR40.pdf](<C:/Users/jinyu/Downloads/MAR40.pdf>)：简化标准法，只用于与完整 SA 比较；
- [MAR30.pdf](<C:/Users/jinyu/Downloads/MAR30.pdf>)、[MAR31.pdf](<C:/Users/jinyu/Downloads/MAR31.pdf>)、[MAR32.pdf](<C:/Users/jinyu/Downloads/MAR32.pdf>)、[MAR33.pdf](<C:/Users/jinyu/Downloads/MAR33.pdf>)：内部模型法，只讲与 SA 的关系；
- [MAR50.pdf](<C:/Users/jinyu/Downloads/MAR50.pdf>)：CVA 风险框架，用于解释 CVA 资本为什么不能与交易账簿 SA 混为一体。

---

# 第一模块：FRTB SA 的体系结构

## 三、先讲清四个层次

FRTB SA 可以分为四层：

| 层次 | 监管章节 | 回答的问题 |
|---|---|---|
| 范围层 | RBC25、MAR11、MAR12 | 哪些头寸、哪些交易台进入市场风险框架？ |
| 风险表示层 | MAR10、MAR21 | 交易风险如何表示为风险因子和敏感度？ |
| 资本模型层 | MAR20–MAR23 | 风险如何经过权重、相关性、DRC、RRAO 转成资本？ |
| 应用层 | MAR11、MAR12及机构治理 | 如何用于持续资本、限额、报告和风险行动？ |

如果范围层错误，后面的计算再精确也没有意义；如果敏感度单位错误，资本会相差 100 或 10,000 倍；如果聚合层错误，可能过度承认对冲；如果应用层只报总数而不解释，模型不能发挥风险控制作用。

## 四、总体资本公式

完整标准法资本为三个组件的简单相加：

$$
K_{SA}=K_{SBM}+K_{DRC}+K_{RRAO}.
$$

其中：

- $K_{SBM}$：敏感度法资本，包括 Delta、Vega、Curvature；
- $K_{DRC}$：突然违约的 Jump-to-Default 资本；
- $K_{RRAO}$：规定的奇异标的和其他剩余风险附加。

市场风险 RWA 为：

$$
RWA_{market}=12.5\times K_{market}.
$$

### 4.1 系数 12.5 的含义

$$
12.5=\frac{1}{8\%}.
$$

它是资本要求与风险加权资产之间的监管换算系数，不是市场波动率、置信水平或风险权重。

### 4.2 报告频率与持续管理

- MAR20.2 要求完整 SA 原则上按月计算并向监管者报告；
- MAR11.2 同时要求银行持续满足资本要求，包括每个营业日结束时，并控制日内风险；
- 因此“月度监管报送”不等于“每月才管理一次风险”。

### 4.3 总体数值例子

假设三个相关性情景下的 SBM 分别为：

| 情景 | SBM |
|---|---:|
| Low | 8.0 百万元 |
| Medium | 7.5 百万元 |
| High | 8.4 百万元 |

则：

$$K_{SBM}=\max(8.0,7.5,8.4)=8.4.$$

若 $K_{DRC}=2.1$ 百万元，$K_{RRAO}=0.3$ 百万元，则：

$$K_{SA}=8.4+2.1+0.3=10.8\text{ 百万元},$$

$$RWA=12.5\times10.8=135\text{ 百万元}.$$

### 常见错误

- 错误：对 SBM、DRC、RRAO 取最大值；
- 正确：三项相加；只有 SBM 的三种相关性情景取最大值。

---

# 第二模块：边界、交易台和模型输入

## 五、RBC25：交易账簿边界是模型的第一道公式

严格来说，边界不是数值公式，但它决定资本模型的输入集合：

$$
\mathcal P_{SA}=\{i:\ i\text{属于市场风险资本适用范围}\}.
$$

### 5.1 进入交易账簿的基本逻辑

根据 RBC25，持有目的包括短期转售、从短期价格变动获利、锁定套利利润，或对冲上述活动风险的工具，原则上应进入交易账簿。交易账簿工具还应不存在阻碍出售或完全对冲的法律障碍，并按日公允价值计量、将价值变化计入 P&L。

### 5.2 边界控制的实际意义

- 防止在交易账簿和银行账簿之间选择资本较低的一方；
- 保证相同经济风险受到一致计量；
- 决定哪些头寸进入 SBM、DRC 和 RRAO；
- 决定内部风险转移是否获得监管资本承认。

### 5.3 账簿转移

RBC25 对账簿转移采取严格限制。市场事件、流动性变化或交易意图变化本身通常不是有效转移理由；转移也不得产生资本利益。系统必须保留转移前后资本、批准、监管沟通和公开披露记录。

## 六、MAR11：完整 SA、简化法和 IMA 的关系

市场风险主要有三种监管路径：

| 路径 | 章节 | 特征 |
|---|---|---|
| 完整标准法 | MAR20–23 | 本课程主线，风险敏感的 SBM+DRC+RRAO |
| 简化标准法 | MAR40 | 适用于经监管批准的较小或较简单交易账簿 |
| 内部模型法 | MAR30–33 | 需要监管批准，包含 ES、NMRF、回测、PLA 等 |

即使银行部分交易台获批使用 IMA，MAR11.8 仍要求：

1. 对全部交易台和全部工具计算全行 SA；
2. 对每个 IMA 合格交易台按独立组合计算 SA，不能跨交易台抵销。

这使 SA 同时具有最低资本方法、IMA 回退、跨银行基准和监管比较工具的作用。

## 七、MAR12：交易台是风险管理和资本的组织单位

交易台需要明确：

- 交易员或交易账户范围；
- 负责人和报告线；
- 允许产品和风险因子；
- 业务和对冲策略；
- 收入、成本和 RWA 预算；
- 风险限额、日内限额及超限处理；
- P&L、风险、流动性和库存账龄报告。

模型实现中，`desk_id` 不是普通标签。它决定独立 SA 计算、限额、资本归因和 IMA 回退范围。

---

## 八、核心数据对象

建议把 SA 输入拆为五张逻辑表。

### 8.1 头寸表

```text
as_of_date, legal_entity, desk_id, trade_id, product_type,
regulatory_book, direction, quantity, notional, market_value,
currency, maturity, issuer, seniority, rating, sector
```

### 8.2 敏感度表

```text
trade_id, risk_class, measure, bucket, risk_factor,
curve_or_name, tenor_1, tenor_2, sensitivity,
reporting_currency, bump_size, bump_unit, model_version
```

### 8.3 Curvature 重估表

```text
trade_id, curvature_factor, V_base, V_up, V_down,
curvature_RW, delta_used, sticky_convention
```

### 8.4 DRC 表

```text
trade_id, obligor, seniority, maturity, LGD, notional,
cumulative_PnL, gross_JTD, net_JTD, DRC_bucket, rating_RW
```

### 8.5 监管参数表

```text
jurisdiction, rule_version, effective_date, risk_class,
measure, bucket, tenor, RW, rho, gamma, scalar, source_paragraph
```

每个字段都要有单位、数据所有人、来源系统和质量规则。

---

# 第三模块：SBM 的通用数学结构

## 九、从敏感度到加权敏感度

令 $s_{ik}$ 表示交易 $i$ 对监管风险因子 $k$ 的敏感度。首先对完全相同的风险因子净额：

$$
s_k=\sum_i s_{ik}.
$$

再乘风险权重：

$$
WS_k=RW_k\,s_k.
$$

### 9.1 三个变量的意义

| 符号 | 含义 | 决定因素 |
|---|---|---|
| $s_k$ | 净监管敏感度 | 头寸、定价模型、bump、映射 |
| $RW_k$ | 监管风险权重 | 风险类别、期限、桶、流动性 |
| $WS_k$ | 加权敏感度 | 风险规模与监管冲击强度的结合 |

风险权重越高，表示在标准法校准中该风险被认为波动更大、流动性更差或更难管理。

## 十、桶内聚合

对桶 $b$：

$$
K_b=
\sqrt{
\max\left(
0,
\sum_{k\in b}WS_k^2+
\sum_{k\in b}\sum_{l\ne k}\rho_{kl}WS_kWS_l
\right)}.
$$

其中：

- $K_b$：桶级资本；
- $\rho_{kl}$：同一桶内风险因子 $k,l$ 的监管相关性；
- 第一项：各风险自身贡献；
- 第二项：风险之间的对冲或同向叠加。

如果程序使用无序组合 $k<l$，则应写为：

$$
K_b=\sqrt{\max\left(0,\sum_kWS_k^2+2\sum_{k<l}\rho_{kl}WS_kWS_l\right)}.
$$

不能同时使用双重有序求和和额外的系数 2。

### 10.1 相关性的风险含义

- $WS_k$ 与 $WS_l$ 同号：相关性越高，资本越高；
- 两者异号：相关性越高，承认的对冲越多，资本越低；
- $\rho=1$ 不等于一定完全抵销，只有风险因子相同才先在 $s_k$ 层完全净额。

### 10.2 桶内数字例子

假设：

$$WS_1=100,\qquad WS_2=-80,\qquad \rho_{12}=50\%.$$

则：

$$
K_b=\sqrt{100^2+80^2+2\times0.5\times100\times(-80)}
=\sqrt{8400}=91.65.
$$

直接净额为 20，但资本为 91.65，因为两个头寸并非同一个风险因子。

## 十一、跨桶聚合

先定义：

$$S_b=\sum_{k\in b}WS_k.$$

风险类别资本为：

$$
K=
\sqrt{
\max\left(
0,
\sum_bK_b^2+
\sum_b\sum_{c\ne b}\gamma_{bc}S_bS_c
\right)}.
$$

其中 $\gamma_{bc}$ 是同一风险类别内桶 $b,c$ 的跨桶相关性。

### 11.1 负根号的替代规则

如果使用原始 $S_b$ 后整体根号内为负，MAR21.4 要求采用受限桶敏感度重新计算：

$$
S_b^{alt}=\max\left[\min\left(\sum_{k\in b}WS_k,K_b\right),-K_b\right].
$$

它把 $S_b$ 限制在 $[-K_b,K_b]$ 内。生产代码不能只用 `max(0, radicand)` 后结束，否则会遗漏替代计算。

## 十二、三种相关性情景

### 12.1 Medium

使用各风险类别规定的基础 $\rho$ 和 $\gamma$。

### 12.2 High

$$
\rho^{high}=\min(1.25\rho,1),
\qquad
\gamma^{high}=\min(1.25\gamma,1).
$$

系数 1.25 用于模拟压力期相关性上升，相关性上限为 100%。

### 12.3 Low

$$
\rho^{low}=\max(2\rho-1,0.75\rho),
$$

$$
\gamma^{low}=\max(2\gamma-1,0.75\gamma).
$$

这里 $1$ 代表 100%。低相关性情景主要限制依赖反向相关对冲的组合。

### 12.4 为什么不能直接假设 High 最大

对同号风险，高相关性通常增加资本；对异号风险，高相关性反而增加对冲收益。因此三种情景必须从桶内开始完整重算，再取：

$$
K_{SBM}=\max\left(K_{SBM}^{low},K_{SBM}^{medium},K_{SBM}^{high}\right).
$$

---

## 十三、SBM 的最终聚合

对相关性情景 $c$：

$$
K_{SBM}^{(c)}=
\sum_{r\in\mathcal R}
\left(K_{\Delta,r}^{(c)}+K_{Vega,r}^{(c)}+K_{Curv,r}^{(c)}\right),
$$

其中七个风险类别为：

$$
\mathcal R=\{GIRR,CSR_{nonsec},CSR_{nonCTP},CSR_{CTP},Equity,Commodity,FX\}.
$$

风险类别之间不再使用相关性分散，而是简单相加。

### SBM 伪代码

```python
for scenario in ["low", "medium", "high"]:
    total = 0
    for risk_class in RISK_CLASSES:
        k_delta = aggregate_delta(risk_class, scenario)
        k_vega = aggregate_vega(risk_class, scenario)
        k_curv = aggregate_curvature(risk_class, scenario)
        total += k_delta + k_vega + k_curv
    sbm_by_scenario[scenario] = total

K_SBM = max(sbm_by_scenario.values())
```

---

# 第四模块：监管敏感度模型

## 十四、敏感度统一原则

所有敏感度必须：

- 以银行报告币种表达；
- 每次只改变一个规定风险因子，其他风险因子保持当前水平；
- 使用独立风险控制部门用于高级管理层风险或实际 P&L 报告的定价模型；
- 能证明替代敏感度公式与监管规定结果非常接近；
- 记录 bump、单位、sticky 约定和模型版本。

## 十五、GIRR Delta：PV01

对币种 $c$、曲线 $q$、期限 $t$ 的利率风险：

$$
s_{c,q,t}^{GIRR}=
\frac{V(r_{c,q,t}+0.0001)-V(r_{c,q,t})}{0.0001}.
$$

### 参数解释

- $0.0001$：1 个基点；
- 分子：真实 +1bp 重估损益；
- 除以 $0.0001$：把损益转换为每 1.00 绝对利率变化的斜率型敏感度。

如果内部系统的 `PV01` 已定义为“+1bp 的货币损益”，则进入 MAR21 权重公式前必须确认是否需要除以 $0.0001$。同名字段可能使用不同单位，必须以数据字典为准。

## 十六、CSR Delta：CS01

$$
s_{name,curve,t}^{CSR}=
\frac{V(cs_t+0.0001)-V(cs_t)}{0.0001}.
$$

CSR 使用发行人或分层价差曲线和规定期限点。非证券化 CSR 的 bond spread 和 CDS spread 在 Delta 中是不同曲线风险因子；Curvature 中相关发行人的 bond/CDS spread curve 按规则合并为一个曲率因子。

## 十七、Equity、Commodity 和 FX Delta

股票现货：

$$
s_k^{EQ}=
\frac{V(1.01EQ_k)-V(EQ_k)}{0.01}.
$$

商品：

$$
s_k^{COM}=
\frac{V(1.01CTY_k)-V(CTY_k)}{0.01}.
$$

外汇：

$$
s_k^{FX}=
\frac{V(1.01FX_k)-V(FX_k)}{0.01}.
$$

股票回购率使用 1bp 平行移动并除以 $0.0001$。

### 相对变化与绝对变化

- 利率和信用价差：绝对增加 1bp；
- 股票、商品、FX：相对增加 1%；
- 把 1% 写成 1 而不是 0.01，会造成 100 倍错误；
- 把 1bp 写成 1 而不是 0.0001，会造成 10,000 倍错误。

## 十八、Vega 监管敏感度

MAR21 定义期权层面的 Vega 风险敏感度为：

$$
s_k^{Vega}=Vega_k\times\sigma_k,
$$

其中：

- $Vega_k=\partial V/\partial\sigma_k$；
- $\sigma_k$：用于独立风险管理定价的隐含波动率；
- 结果不是单纯“波动率上升一个百分点的 P&L”。

### 18.1 分布假设

- GIRR、CSR：允许 normal 或 lognormal；
- Equity、Commodity、FX：必须使用 lognormal；
- Vega 计算忽略 CVA 影响；
- 一阶期权敏感度应使用一致的 sticky-strike 或 sticky-delta 假设。

### 18.2 Vega 到期期限

期权到期维度：

$$0.5,\ 1,\ 3,\ 5,\ 10\text{ 年}.$$

GIRR Vega 还有第二维：期权到期时标的的剩余期限，也映射到相同五个期限点。

## 十九、Curvature：大冲击下超出 Delta 的损失

对曲率风险因子 $k$：

$$
CVR_k^+=-
\sum_i
\left[
V_i(x_k^{up})-V_i(x_k)-RW_k^{curv}s_{ik}
\right],
$$

$$
CVR_k^-=-
\sum_i
\left[
V_i(x_k^{down})-V_i(x_k)+RW_k^{curv}s_{ik}
\right].
$$

### 19.1 每一项的含义

- $V_i(x_k^{up/down})-V_i(x_k)$：规定大冲击的完整重估；
- $\pm RW_k^{curv}s_{ik}$：扣除 Delta 已覆盖的一阶部分；
- 外部负号：把价值下降转换为正的损失量；
- $CVR^+$ 与 $CVR^-$：上、下两个方向的增量非线性损失。

Curvature 不是普通 Gamma。Gamma 是局部二阶导数；Curvature 使用监管规定的大幅冲击和完整重估。

### 19.2 桶内曲率聚合

定义：如果 $x,y$ 同为负数，则 $\psi(x,y)=0$；其他情况 $\psi(x,y)=1$。

$$
K_b^+=
\sqrt{
\max\left(
0,
\sum_k\max(CVR_k^+,0)^2+
\sum_k\sum_{l\ne k}
\rho_{kl}^{curv}CVR_k^+CVR_l^+
\psi(CVR_k^+,CVR_l^+)
\right)},
$$

$$
K_b^-=
\sqrt{
\max\left(
0,
\sum_k\max(CVR_k^-,0)^2+
\sum_k\sum_{l\ne k}
\rho_{kl}^{curv}CVR_k^-CVR_l^-
\psi(CVR_k^-,CVR_l^-)
\right)},
$$

$$K_b^{curv}=\max(K_b^+,K_b^-).$$

注意：每个相关性情景下选中的上/下方向可能不同。

### 19.3 跨桶曲率聚合

如果桶 $b$ 选择向上方向：

$$S_b=\sum_kCVR_k^+,$$

否则：

$$S_b=\sum_kCVR_k^-.$$

风险类别曲率资本为：

$$
K_{curv}=
\sqrt{
\max\left(
0,
\sum_bK_b^2+
\sum_b\sum_{c\ne b}
\gamma_{bc}^{curv}S_bS_c\psi(S_b,S_c)
\right)}.
$$

### 19.4 曲率相关性

$$
\rho_{kl}^{curv}=(\rho_{kl}^{delta})^2,
\qquad
\gamma_{bc}^{curv}=(\gamma_{bc}^{delta})^2.
$$

“平方一次”是关键控制点。三情景调整作用于曲率相关性参数，不能在多个步骤重复平方。

---

# 第五模块：风险因子、风险权重和相关性参数

## 二十、参数的经济角色

| 参数 | 数学角色 | 风险意义 |
|---|---|---|
| $RW$ | 将敏感度转成加权敏感度 | 冲击强度、波动性和流动性校准 |
| $\rho$ | 桶内交叉项 | 相近风险因子间允许的对冲/叠加 |
| $\gamma$ | 跨桶交叉项 | 更远风险之间允许的分散 |
| LH | 进入 Vega RW | 压力市场退出或对冲所需时间 |
| 1.25 | High 相关性倍数 | 相关性上升压力 |
| 0.75及$2\rho-1$ | Low 相关性变换 | 相关性下降压力 |
| $\sqrt 2$ | 指定币种/币对权重优惠 | 监管允许的流动性和波动差异 |

## 二十一、GIRR 参数

### 21.1 风险因子维度

$$\text{币种}\times\text{曲线}\times\text{期限}.$$

规定期限：

$$0.25,0.5,1,2,3,5,10,15,20,30\text{ 年}.$$

OIS、不同期限 BOR、在岸/离岸曲线视为不同曲线。通胀和跨币种基差为额外风险因子。

### 21.2 GIRR Delta 风险权重

| 期限 | 0.25Y | 0.5Y | 1Y | 2Y | 3Y | 5Y | 10Y | 15Y | 20Y | 30Y |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| RW | 1.7% | 1.7% | 1.6% | 1.3% | 1.2% | 1.1% | 1.1% | 1.1% | 1.1% | 1.1% |

通胀和跨币种基差 RW 均为 1.6%。EUR、USD、GBP、AUD、JPY、SEK、CAD 以及银行本国报告币种，银行可选择将上述权重除以 $\sqrt2$。

### 21.3 GIRR 期限相关性

同一曲线不同期限：

$$
\rho_{kl}^{tenor}=
\max\left(
\exp\left[-0.03\frac{|T_k-T_l|}{\min(T_k,T_l)}\right],
40\%
\right).
$$

系数 $0.03$ 控制相关性随相对期限差衰减；40% 是下限。

- 同期限、不同曲线：99.90%；
- 不同期限、不同曲线：期限相关性 ×99.90%；
- 通胀与同币种收益率曲线：40%；
- 跨币种基差与收益率/通胀/另一基差：0%；
- 不同币种桶间 $\gamma=50\%$。

### 21.4 两期限算例

假设 1Y 敏感度 $s_1=10,000,000$，5Y 敏感度 $s_5=-8,000,000$：

$$WS_1=1.6\%\times10,000,000=160,000,$$

$$WS_5=1.1\%\times(-8,000,000)=-88,000.$$

1Y 与 5Y 的期限相关性为：

$$\rho=\max(e^{-0.03\times4/1},0.4)\approx88.7\%.$$

桶资本：

$$
K_b=\sqrt{160000^2+88000^2+2\times0.887\times160000\times(-88000)}
\approx91,400.
$$

这说明期限相近或曲线相关的反向风险可以获得部分对冲，但不能当作同一风险因子完全净额。

## 二十二、CSR non-securitisation 参数

### 22.1 风险因子与期限

Delta CSR 风险因子：

$$\text{发行人}\times\text{bond/CDS曲线}\times\{0.5,1,3,5,10\}Y.$$

桶由信用质量和行业决定。

### 22.2 18 个桶的 Delta RW

| 桶 | 类型概述 | RW |
|---:|---|---:|
| 1 | IG 主权/央行/MDB | 0.5% |
| 2 | IG 地方政府、政府支持非金融等 | 1.0% |
| 3 | IG 金融 | 5.0% |
| 4 | IG 原材料、能源、工业等 | 3.0% |
| 5 | IG 消费、运输、服务等 | 3.0% |
| 6 | IG 科技、电信 | 2.0% |
| 7 | IG 医疗、公用、专业活动 | 1.5% |
| 8 | Covered bonds | 2.5% |
| 9 | HY/NR 主权/央行/MDB | 2.0% |
| 10 | HY/NR 地方政府等 | 4.0% |
| 11 | HY/NR 金融 | 12.0% |
| 12 | HY/NR 原材料、能源、工业等 | 7.0% |
| 13 | HY/NR 消费、运输、服务等 | 8.5% |
| 14 | HY/NR 科技、电信 | 5.5% |
| 15 | HY/NR 医疗、公用、专业活动 | 5.0% |
| 16 | Other sector | 12.0% |
| 17 | IG 指数 | 1.5% |
| 18 | HY 指数 | 5.0% |

符合条件且 AA- 或以上的 covered bond，可按规则选择 1.5%。

### 22.3 桶内相关性分解

桶 1–15：

$$
\rho_{kl}=\rho^{name}_{kl}\rho^{tenor}_{kl}\rho^{basis}_{kl},
$$

其中：

- 同一名称：$\rho^{name}=1$；不同名称：35%；
- 同一期限：$\rho^{tenor}=1$；不同期限：65%；
- 同一曲线：$\rho^{basis}=1$；bond/CDS 等不同曲线：99.90%。

桶 17–18 的不同名称相关性提高为 80%。桶 16 不使用上述分散，Delta/Vega 按净加权敏感度绝对值简单相加。

### 22.4 参数意义

- 名称相关性 35%：同一行业不代表不同发行人会完全同步；
- 期限相关性 65%：同一信用曲线的不同期限存在不完全期限对冲；
- basis 99.90%：bond/CDS 高度相关但仍保留很小基差风险；
- Other sector 不给分散：无法可靠分类时采用更保守处理。

## 二十三、证券化 CSR 参数

### 23.1 CTP

CTP 使用与非证券化类似的行业桶，但风险权重更高、basis 相关性更低。16 个桶的 RW 为：

$$
4,4,8,5,4,3,2,6,13,13,16,10,12,12,12,13\%.
$$

不同曲线的相关性因子为 99.00%，反映更大的基差风险。

### 23.2 Non-CTP

按信用质量和证券化类型形成 25 个桶。Senior IG 基础权重为：

| 类型桶 1–8 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| RW | 0.9% | 1.5% | 2.0% | 2.0% | 0.8% | 1.2% | 1.2% | 1.4% |

- Non-senior IG 对应权重 ×1.25；
- HY/NR 对应权重 ×1.75；
- Other sector 桶 25：3.5%；
- 桶 1–24 跨桶 $\gamma=0$，即不承认跨桶分散；
- 桶 25 与其他桶资本直接相加。

## 二十四、Equity 参数

股票桶按市值、经济体和行业形成 13 个桶。大市值门槛为 20 亿美元。

| 桶 | Spot RW | Repo RW |
|---:|---:|---:|
| 1 | 55% | 0.55% |
| 2 | 60% | 0.60% |
| 3 | 45% | 0.45% |
| 4 | 55% | 0.55% |
| 5 | 30% | 0.30% |
| 6 | 35% | 0.35% |
| 7 | 40% | 0.40% |
| 8 | 50% | 0.50% |
| 9 | 70% | 0.70% |
| 10 | 50% | 0.50% |
| 11 | 70% | 0.70% |
| 12 | 15% | 0.15% |
| 13 | 25% | 0.25% |

桶内不同名称相关性：

- 大市值新兴市场桶：15%；
- 大市值发达市场桶：25%；
- 小市值新兴市场桶：7.5%；
- 小市值发达市场桶：12.5%；
- 指数桶：80%；
- 同一发行人的 spot 与 repo：99.90%。

跨桶 $\gamma$：

- 桶 1–10 之间：15%；
- 任一桶为 Other sector 11：0%；
- 指数桶 12 与 13：75%；
- 其他组合：45%。

## 二十五、Commodity 参数

| 桶 | 商品类别 | Delta RW | 桶内不同商品基础相关性 |
|---:|---|---:|---:|
| 1 | 固体燃料 | 30% | 55% |
| 2 | 液体燃料 | 35% | 95% |
| 3 | 电力和碳交易 | 60% | 40% |
| 4 | 货运 | 80% | 80% |
| 5 | 非贵金属 | 40% | 60% |
| 6 | 气体燃料 | 45% | 65% |
| 7 | 贵金属 | 20% | 55% |
| 8 | 谷物和油籽 | 35% | 45% |
| 9 | 牲畜和乳制品 | 25% | 15% |
| 10 | 软商品及其他农产品 | 35% | 40% |
| 11 | 其他商品 | 50% | 15% |

桶内相关性为商品、期限和交割地点三个因子的乘积：

- 相同商品为 1，否则使用表中基础相关性；
- 相同期限为 1，否则 99.00%；
- 相同交割地点为 1，否则 99.90%。

桶 1–10 跨桶 $\gamma=20\%$；任一桶为 11 时为 0%。

## 二十六、FX 参数

每个“外币相对报告币种的汇率”构成一个桶：

$$RW_{FX}=15\%,$$

$$\gamma_{FX}=60\%.$$

对巴塞尔指定币对及其一阶交叉币对，银行可选择将 RW 除以 $\sqrt2$。该选择属于监管参数处理，必须保存适用币对名单、审批和一致性规则。

## 二十七、Vega 参数

Vega 风险权重由基础波动率冲击和流动性期限决定：

$$
RW_k^{Vega}=\min\left(55\%\sqrt{\frac{LH_k}{10}},100\%\right).
$$

| 风险类别 | LH（日） | Vega RW |
|---|---:|---:|
| GIRR | 60 | 100% |
| CSR non-securitisation | 120 | 100% |
| CSR CTP | 120 | 100% |
| CSR non-CTP | 120 | 100% |
| Equity 大市值和指数 | 20 | 77.78% |
| Equity 小市值和 Other | 60 | 100% |
| Commodity | 120 | 100% |
| FX | 40 | 100% |

### 27.1 Vega 期限相关性

对两个期权期限 $T_k,T_l$：

$$
\rho_{kl}^{option}=
\exp\left[-0.01\frac{|T_k-T_l|}{\min(T_k,T_l)}\right].
$$

GIRR Vega 桶内相关性为期权到期相关性与标的剩余期限相关性的乘积。非 GIRR Vega 通常为对应 Delta 风险因子相关性与期权到期相关性的乘积。跨桶沿用对应 Delta 的 $\gamma$。

## 二十八、Curvature 冲击参数

- Equity、FX：冲击幅度等于相应 Delta RW；
- GIRR、CSR、Commodity：对曲线全部期限平行冲击，幅度采用相应桶中最高 Delta RW；
- GIRR 通胀和跨币种基差不计算 Curvature；
- CSR 向下冲击导致负价差时，满足 MAR21.99 FAQ 条件可把风险因子下限设为 0，并相应限制向下冲击；
- FX 的某些非报告币种交叉期权还涉及 1.5 标量处理，必须按 MAR21.98 的一致性和监管批准条件实施。

---

# 第六模块：DRC 模型

## 二十九、DRC 为什么独立于 CSR

CSR 用小幅价差变化描述迁移风险；DRC 描述突然违约造成的离散损失。一个连续变化模型不能充分覆盖瞬间从“未违约”跳到“违约”的损失，因此：

$$K_{credit}=K_{CSR}+K_{DRC}$$

并不构成简单重复计量。

## 三十、Gross JTD

对非证券化风险：

$$
JTD_{long}=\max(LGD\times N+P\&L,0),
$$

$$
JTD_{short}=\min(LGD\times N+P\&L,0).
$$

### 30.1 变量含义

- $N$：债券等价名义本金；long 为正，short 为负；
- $LGD$：违约损失率；
- $P\&L$：已经进入市值的累计损益；亏损为负，盈利为正；
- long/short：按违约时亏损还是盈利判断，不按“买入/卖出”标签判断。

### 30.2 监管 LGD

| 工具层级 | LGD |
|---|---:|
| 股票及非优先债务 | 100% |
| 优先债务 | 75% |
| 符合定义的 covered bond | 25% |

## 三十一、期限缩放

一年的资本期限下：

$$
JTD_i^{scaled}=w_iJTD_i,
$$

$$
w_i=
\begin{cases}
1, & T_i\ge1,\\
\max(T_i,0.25), & T_i<1.
\end{cases}
$$

三个月下限对应 $0.25$。衍生品使用衍生品合同到期，而不是标的到期。现金股票可按规则选择三个月或一年以上的期限处理，但必须一致、受控。

## 三十二、同一债务人净额

同一 obligor 的 long 和 short JTD 只有在优先级条件满足时才可净额。规则的直觉是：short exposure 的优先级必须与 long 相同或更低，才能可靠对冲 long 的违约损失。

净额后分别保留：

$$NetJTD_{long}>0,\qquad NetJTD_{short}<0.$$

不同债务人之间不能先完全净额，只能在桶内通过 HBR 获得有限对冲。

## 三十三、HBR

$$
HBR=
\frac{\sum NetJTD_{long}}
{\sum NetJTD_{long}+\sum|NetJTD_{short}|}.
$$

### 参数意义

- 如果只有 long：$HBR=1$，没有空头对冲；
- 如果 short 很大：HBR 下降，空头能获得的资本抵销被进一步折扣；
- HBR 使用未加权 JTD 计算，然后再作用于风险加权 short JTD。

## 三十四、非证券化 DRC 资本

三个桶：corporates、sovereigns、local governments/municipalities。

每个桶：

$$
DRC_b=
\max\left[
\sum_{i\in long}RW_iNetJTD_i
-HBR_b\sum_{i\in short}RW_i|NetJTD_i|,
0
\right].
$$

总资本：

$$DRC_{nonsec}=\sum_bDRC_b.$$

不同桶之间不承认对冲。

### 34.1 DRC 风险权重

| 评级 | RW |
|---|---:|
| AAA | 0.5% |
| AA | 2% |
| A | 3% |
| BBB | 6% |
| BB | 15% |
| B | 30% |
| CCC | 50% |
| Unrated | 15% |
| Defaulted | 100% |

### 34.2 数字例子

同一 corporate 桶：

- A 级 net long JTD：1,000 万元，RW=3%；
- AA 级 net short JTD：600 万元，RW=2%。

$$HBR=\frac{10}{10+6}=0.625.$$

$$
DRC_b=3\%\times10,000,000
-0.625\times2\%\times6,000,000
=225,000.
$$

若错误地把 short 全额抵销，结果会低估不同债务人之间的 basis/default correlation risk。

## 三十五、证券化 DRC

### 35.1 Non-CTP

- Gross JTD 不再额外乘 LGD，因为 LGD 已体现在证券化风险权重中；
- JTD 通常以证券化敞口市值表示；
- 净额限于特定证券化敞口及严格复制条件；
- 风险权重依据银行账簿证券化框架并作一年的期限调整；
- 不同桶之间不承认对冲。

### 35.2 CTP

- 每个指数定义为一个桶；
- CTP 的 HBR 使用整个 CTP 的 long 和 short；
- 单个指数桶 DRC 可以为负；
- 跨指数汇总时负桶只按 50% 提供抵销：

$$
DRC_{CTP}=
\max\left[
\sum_b\left(\max(DRC_b,0)+0.5\min(DRC_b,0)\right),
0
\right].
$$

系数 0.5 对跨指数空头对冲再次折扣，反映跨指数 basis risk。

---

# 第七模块：RRAO 模型

## 三十六、适用范围

RRAO 独立于 SBM 和 DRC 额外计提。两类工具为：

1. Exotic underlying：主要风险因子不在 SBM 或 DRC 风险因子范围内，例如 longevity、weather、natural disaster、future realised volatility；
2. Other residual risks：收益无法由有限个单一标的 vanilla option 线性复制，或符合 CTP 的相关规定结构，例如 gap、path dependency、某些 correlation 和 behavioural risk。

## 三十七、公式和参数

$$
K_{RRAO}=\sum_iGrossNotional_i\times RW_i.
$$

| 类型 | RW |
|---|---:|
| Exotic underlying | 1.0% |
| Other residual risk | 0.1% |

RRAO 不减少 Delta、Vega、Curvature 或 DRC 的适用范围。除符合条件的完全 back-to-back 交易等规定排除外，不能随意净额。

### 37.1 数字例子

- Exotic underlying gross notional：2,000 万元；
- Other residual risk gross notional：5,000 万元。

$$
K_{RRAO}=20,000,000\times1\%+50,000,000\times0.1\%
=250,000.
$$

### 37.2 系数意义

RRAO 使用 gross notional 而不是模型敏感度，属于简单、保守的附加方法。其目的不是精细估计真实损失分布，而是在不把 SA 变得过度复杂的情况下，为难以由主要风险因子充分描述的结构保留资本。

---

# 第八模块：完整计算引擎

## 三十八、端到端计算顺序

1. 确定法律实体、报告币种和监管规则版本；
2. 确定交易账簿和其他市场风险范围；
3. 将每笔交易分配到唯一交易台；
4. 生成基准价值、Delta、Vega 和 Curvature 重估；
5. 统一敏感度单位和报告币种；
6. 映射七类风险、桶、曲线、名称和期限；
7. 对同一风险因子净额；
8. 加载 RW、$\rho$、$\gamma$、LH 和标量；
9. 在 low/medium/high 下计算 Delta、Vega、Curvature；
10. 三情景取最大得到 SBM；
11. 独立计算 DRC；
12. 独立识别和计算 RRAO；
13. 相加得到 SA 资本并乘 12.5；
14. 完成对账、变化解释、限额和签署。

## 三十九、程序模块设计

```python
def determine_scope(positions, boundary_rules): ...
def generate_regulatory_sensitivities(positions, market, models): ...
def normalize_units(sensitivities, reporting_currency): ...
def map_risk_factors(sensitivities, mapping_tables): ...
def apply_correlation_scenario(correlation, scenario): ...
def aggregate_delta_vega(mapped, parameters, scenario): ...
def calculate_curvature(revaluations, deltas, parameters, scenario): ...
def calculate_sbm(mapped, revaluations, parameters): ...
def calculate_gross_and_net_jtd(positions, credit_data): ...
def calculate_drc(jtd_records, parameters): ...
def calculate_rrao(positions, scope_rules): ...
def calculate_total_sa(sbm, drc, rrao): ...
```

## 四十、参数必须配置化

程序逻辑只实现“如何用参数”，参数表决定“具体使用什么值”。以下内容不能散落硬编码：

- Delta RW；
- Vega LH、RW 和期限参数；
- $\rho$ 和 $\gamma$；
- 三情景变换；
- Curvature shock 和 scalar；
- DRC LGD、评级 RW 和桶；
- RRAO 范围和 RW；
- 指定币种和币对名单；
- 生效日、当地自由裁量和监管批准项。

---

# 第九模块：参数调试与模型验证

## 四十一、单位测试

| 测试 | 正确表现 | 典型问题 |
|---|---|---|
| 1bp 写成 0.0001 | PV01/CS01 与内部风险一致 | 资本差 10,000 倍 |
| 1% 写成 0.01 | 股票/商品/FX 风险一致 | 资本差 100 倍 |
| 报告币种转换 | 所有敏感度同币种 | 混合币种直接相加 |
| Vega×vol | 与 MAR21 定义一致 | 把 1 vol point P&L 直接输入 |
| 数量×2 | 资本近似×2 | 头寸乘数或净额错误 |

## 四十二、相关性测试

1. 单风险因子时，low/medium/high 资本相同；
2. 同号风险下提高相关性，资本一般增加；
3. 异号风险下提高相关性，资本一般降低；
4. High 参数不得超过 100%；
5. Low 参数必须同时满足 $2\rho-1$ 和 $0.75\rho$ 的较大值；
6. Curvature 使用 Delta 相关性平方一次；
7. 不同风险类别之间不得自行增加相关性分散。

## 四十三、聚合测试

- 完全相同风险因子的相反敏感度应在 $s_k$ 层净额；
- 不同风险因子只能通过 $\rho$ 部分抵销；
- Other sector 使用规定的保守聚合；
- 跨桶根号为负时触发受限 $S_b$ 替代计算；
- 双重求和不能重复乘 2；
- 每个场景都必须重新计算桶级和风险类别级资本。

## 四十四、Curvature 测试

- 输出 $V_{base},V_{up},V_{down},Delta\ term,CVR^+,CVR^-$；
- 纯线性工具的 Curvature 应接近零；
- shock 必须落到正确曲线/名称/币种；
- Delta 和 shocked valuation 使用相同 sticky 假设；
- GIRR/CSR/Commodity 的全期限平行冲击不得误成单期限冲击；
- CSR 向下冲击的零下限只在规则允许的场景使用；
- 桶选择的 up/down 方向随相关性情景可改变。

## 四十五、DRC 测试

- long/short 按违约损益判断；
- LGD 与优先级匹配；
- $P\&L$ 符号正确，避免重复计算已确认损失；
- 三个月期限下限正确；
- 只有同一 obligor 且满足优先级条件才完全净额；
- HBR 使用未加权净 JTD；
- 风险权重在 HBR 之后进入桶资本；
- non-CTP、CTP 和 non-securitisation 之间不承认分散。

## 四十六、RRAO 测试

- 逐条记录触发原因；
- 核对 gross notional 定义；
- 验证 back-to-back 是否真的完全匹配；
- listed/central clearing 排除只按 MAR23 规定范围使用；
- RRAO 不能替代 SBM 或 DRC；
- 新产品和条款变更必须重新判断范围。

## 四十七、独立验证证据

至少保存：

- 监管段落到代码和参数的映射矩阵；
- 公式单元测试；
- 监管示例复算；
- 历史日期重现；
- 独立实现或手工基准；
- 前台、风险、财务和监管报送对账；
- 参数变更影响分析；
- 模型限制、保守附加和问题整改。

---

# 第十模块：应用解释

## 四十八、从资本构成判断风险

| 资本构成 | 说明 | 可能的风险行动 |
|---|---|---|
| Delta 高 | 方向、曲线、价差或现货风险高 | 减仓、方向对冲、期限调整 |
| Vega 高 | 隐含波动率暴露高 | 调整期权结构或波动率对冲 |
| Curvature 高 | 大幅行情下一阶对冲失效 | 降低非线性、增加压力缓冲 |
| DRC 高 | 名称集中或违约跳跃风险高 | 降低单名风险、改善信用对冲 |
| RRAO 高 | 奇异/路径/相关性结构多 | 重新评价产品收益与资本成本 |
| Low 情景最大 | 组合依赖反向对冲 | 检查相关性下降和 basis risk |
| High 情景最大 | 同向风险集中 | 检查资产、期限和行业集中度 |

## 四十九、资本不是唯一的风险指标

SA 应与下列指标共同使用：

- 日常 Greeks 和关键期限风险；
- P&L 和 P&L explain；
- 压力测试；
- 流动性和退出期限；
- 单一发行人和行业集中度；
- 止损与风险偏好；
- 估值不确定性和模型风险。

降低 SA 资本不一定降低全部风险。一个资本有效的对冲可能增加流动性、展期、basis 或模型风险。

## 五十、与 MAR40、MAR30–33 和 MAR50 的边界

### 50.1 MAR40

MAR40 是经监管批准、面向较小或较简单交易账簿的简化替代方法，使用传统的利率、股票、FX、商品和期权处理。不能把 MAR40 参数混入 MAR21 的完整 SBM。

### 50.2 MAR30–33

IMA 使用 ES、流动性期限、NMRF、回测、PLA 和内部违约风险模型。SA 与 IMA 是不同资本模型，但 SA 仍作为全行和交易台层面的基准及回退。

### 50.3 MAR50

MAR50 计量 CVA 风险，即交易对手信用价差和衍生品价值变化导致 CVA 改变的风险。它有 BA-CVA 和 SA-CVA，与 MAR20–23 的交易账簿 SA 分开。符合资格的外部 CVA 对冲按 MAR50 规则从市场风险资本中排除；不合格对冲仍按交易账簿资本处理。

---

# 第十一模块：建议授课流程

## 五十一、12 次课安排

| 课次 | 主题 | 重点公式/参数 | 课堂产出 |
|---|---|---|---|
| 1 | FRTB 体系、意义和边界 | $K_{SA}$、12.5 | 画出完整架构 |
| 2 | 交易台、数据和风险因子 | MAR10–12 | 建立输入数据字典 |
| 3 | SBM 通用聚合 | $WS,K_b,S_b,K$ | 手算桶内/跨桶 |
| 4 | 三相关性情景 | high/low 变换 | 分析同号/异号组合 |
| 5 | 监管 Delta | PV01、CS01、1%敏感度 | 完成单位实验 |
| 6 | GIRR 与 CSR 参数 | RW、期限、name/basis 相关性 | 完成参数映射 |
| 7 | Equity、Commodity、FX | 桶、RW、$\rho,\gamma$ | 比较风险类别结构 |
| 8 | Vega | $Vega\times\sigma$、LH | 构建 Vega 网格 |
| 9 | Curvature | $CVR^\pm$、$\psi$、相关性平方 | 完成上下重估聚合 |
| 10 | DRC | JTD、期限、HBR、评级 RW | 完成信用桶算例 |
| 11 | RRAO 与总资本 | gross notional×RW | 完成总资本计算 |
| 12 | 引擎、验证和应用 | 控制矩阵 | 提交端到端项目 |

## 五十二、建议讲解节奏

每个公式都按以下顺序讲：

1. 先画出风险关系；
2. 再说明变量的单位和符号；
3. 代入只有两个风险因子的数字；
4. 改变一个系数观察结果；
5. 解释这个系数限制了哪一种对冲；
6. 最后展示生产系统的输入、输出和控制。

### 可直接使用的讲稿

> FRTB SA 不是把敏感度简单相加。先对完全相同的风险因子净额，再用风险权重决定单个风险的冲击强度，用桶内相关性决定相近风险可以抵销多少，用跨桶相关性决定更远风险可以分散多少。由于压力期相关性可能上升，也可能下降，模型把整个过程计算三次并取最大值。信用突然违约和无法充分拆解的复杂风险，再由 DRC 和 RRAO 补充。

---

# 第十二模块：课程项目

## 五十三、项目输入

教师给出一组已经生成的监管风险记录，不要求学生开发定价模型：

- 七类风险的 Delta/Vega 敏感度；
- Curvature 的基准、向上和向下价值；
- DRC 的名义本金、LGD、P&L、期限、评级和 obligor；
- RRAO 产品范围和 gross notional；
- 监管参数表。

## 五十四、项目任务

1. 完成风险因子净额；
2. 计算加权敏感度；
3. 计算桶内和跨桶资本；
4. 计算三相关性情景；
5. 计算 Vega 和 Curvature；
6. 计算 JTD、HBR 和 DRC；
7. 计算 RRAO；
8. 形成 SA 和 RWA；
9. 完成至少 20 个验证测试；
10. 解释资本驱动并提出风险行动。

## 五十五、评分标准

| 项目 | 权重 |
|---|---:|
| 公式和计算准确性 | 30% |
| 参数、单位和映射 | 25% |
| 模型解释 | 15% |
| 验证和控制 | 20% |
| 管理应用 | 10% |

---

# 第十三模块：一页总复习

## 五十六、八个必须记住的公式

### 1. 总资本

$$K_{SA}=K_{SBM}+K_{DRC}+K_{RRAO}.$$

### 2. RWA

$$RWA=12.5K.$$

### 3. 加权敏感度

$$WS_k=RW_ks_k.$$

### 4. 桶内资本

$$K_b=\sqrt{\max(0,\sum WS_k^2+\sum_{k\ne l}\rho_{kl}WS_kWS_l)}.$$

### 5. 跨桶资本

$$K=\sqrt{\max(0,\sum K_b^2+\sum_{b\ne c}\gamma_{bc}S_bS_c)}.$$

### 6. 相关性情景

$$\rho^{high}=\min(1.25\rho,1),\qquad
\rho^{low}=\max(2\rho-1,0.75\rho).$$

### 7. HBR

$$HBR=\frac{\sum NetJTD_{long}}{\sum NetJTD_{long}+\sum|NetJTD_{short}|}.$$

### 8. RRAO

$$K_{RRAO}=\sum GrossNotional_iRW_i.$$

## 五十七、模型思想总结

FRTB SA 的核心不是某一个风险权重，而是分层限制对冲：

1. 完全相同风险因子可以净额；
2. 同一桶内的不同风险因子按 $\rho$ 部分抵销；
3. 同一风险类别的不同桶按 $\gamma$ 更有限地分散；
4. 不同风险类别不再给予相关性分散；
5. 三相关性情景防止资本依赖单一相关性状态；
6. DRC 限制不同债务人间的违约对冲；
7. RRAO 对难以由主要因子表达的风险采用保守附加。

这就是 FRTB SA 同时具有“模型体系”和“风险控制体系”意义的原因。

## 五十八、本地化前仍需确认

- 适用国家或地区；
- 当地监管生效版本；
- 本国报告币种及指定币种处理；
- 国家自由裁量和过渡安排；
- 机构的评级、行业和产品映射；
- 敏感度单位和模型约定；
- 是否获准使用某些 $\sqrt2$、FX scalar 或其他选项；
- 监管报表、披露和签署流程。

在上述信息确认前，本讲义是基于巴塞尔附件的完整教学模型，不是当地监管报送操作意见。

---

## 五十九、公式与监管段落索引

| 讲义内容 | 主要监管出处 |
|---|---|
| 交易账簿范围、推定分类、转移和内部风险转移 | RBC25.1–25.35 |
| 市场风险、风险因子、敏感度、Delta/Vega/Curvature、JTD | MAR10.1、10.9–10.25 |
| SA/IMA/简化法关系和 SA 独立交易台计算 | MAR11.7–11.9 |
| 交易台结构、限额和风险报告 | MAR12.1–12.6 |
| $K_{SA}$ 三组件及 $12.5$ RWA 转换 | MAR20.1–20.4 |
| Delta/Vega 桶内和跨桶聚合 | MAR21.4 |
| Curvature 定义和聚合 | MAR21.5、21.96–21.101 |
| 三种相关性情景及 SBM 最大值 | MAR21.6–21.7 |
| 七类风险因子 | MAR21.8–21.14 |
| 敏感度定义和模型要求 | MAR21.15–21.30 |
| Delta 桶、RW、$\rho$、$\gamma$ | MAR21.39–21.89 |
| Vega RW 和相关性 | MAR21.90–21.95 |
| DRC、JTD、期限、HBR 和评级 RW | MAR22.1–22.26 |
| 证券化 DRC | MAR22.27–22.45 |
| RRAO 范围、排除和权重 | MAR23.1–23.8 |
| 简化标准法 | MAR40 |
| IMA 比较 | MAR30–MAR33 |
| CVA 风险与市场风险对冲边界 | MAR50.1–50.13 |

课堂展示具体参数时，应同时显示“数值、单位、适用范围、监管段落和规则版本”，避免学生把一张权重表误认为永远不变的数学常数。
