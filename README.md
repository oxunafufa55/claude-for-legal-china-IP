# claude-for-legal-china-IP: 中国知识产权法律工作流插件

> 基于 Anthropic [`claude-for-legal`](https://github.com/anthropics/claude-for-legal)（Apache 2.0）进行中国法本土化改造，适配中国大陆高科技企业知识产权管理实践。

## 适用对象

- 中国企业知识产权部 / 法务部
- 专利代理事务所
- 高校/科研院所技术转移办公室
- 尤其适合：有专利组合管理需求、涉及软件与硬件交叉领域、有海外布局（PCT）需求的技术企业

## 核心改造差异（vs 原版 ip-legal）

| 维度 | 原版（US-based） | 本版（PRC-based） |
|:---|:---|:---|
| **法律体系** | 美国普通法、判例法、USPTO 行政实践 | 中国大陆成文法、CNIPA 审查实践、最高法司法解释 |
| **专利类型** | 发明（Utility）、植物专利、外观设计 | 发明、实用新型、外观设计（三分法，审查标准不同） |
| **专利性标准** | §101 主题适格、§102 新颖性、§103 非显而易见性 | 《专利法》第22条三性、第25条排除主题 |
| **侵权判定** | Claim construction、DOE、File wrapper estoppel | 全面覆盖原则、等同原则、禁止反悔、现有技术抗辩 |
| **商业秘密** | State Uniform Trade Secrets Act | 《反不正当竞争法》第9条、第32条举证责任转移 |
| **开源合规** | DMCA、OSI 许可证 | 叠加《著作权法》《计算机软件保护条例》、GPL 传染性中国司法认定 |
| **网络侵权** | DMCA §512 通知-删除 | 《民法典》第1195条通知-必要措施、《电子商务法》第42条 |
| **合同规则** | Assignment、License、Indemnity（普通法） | 登记生效主义（专利转让）、职务发明（《专利法》第6条）、技术合同编（《民法典》第859-860条） |
| **期限规则** | USPTO 维持费、Trademark 10年续展+使用声明 | CNIPA 年费（授权后每年递增）、商标10年续展、软著50年 |
| **数据合规** | 默认云端（Claude API） | 涉密信息强制私有化部署、数据出境安全评估、保密办主任审核 |

## 包含技能（Skills）

| 命令 | 技能 | 说明 |
|:---|:---|:---|
| `/claude-for-legal-china-IP:cold-start-interview` | 冷启动面试 | 首次使用必做：交互式问答生成企业实践画像 `CLAUDE.md` |
| `/claude-for-legal-china-IP:invention-intake` | 交底书三性初评 | 新颖性、创造性、实用性初筛；区分发明/实用新型；识别主题适格性障碍；评估宽限期与保密审查义务 |
| `/claude-for-legal-china-IP:fto-triage` | FTO 初筛 | 中国专利侵权风险初筛；全面覆盖/等同原则初评；现有技术抗辩/先用权线索；规避设计建议 |
| `/claude-for-legal-china-IP:trademark-clearance` | 商标清查 | 在先权利冲突检索策略；绝对/相对驳回风险评估；恶意注册排查；跨类驰名商标保护风险 |
| `/claude-for-legal-china-IP:infringement-triage` | 侵权初筛 | 商标/专利/著作权/商业秘密四权侵权初步判定；抗辩/反诉风险；行动建议（警告函/诉讼/无效/行政投诉） |
| `/claude-for-legal-china-IP:ip-clause-review` | IP 合同条款审查 | 技术合同权属、许可、转让、保证、违约责任审查；职务发明/委托开发/合作开发；反垄断合规 |
| `/claude-for-legal-china-IP:oss-review` | 开源合规检查 | 许可证分类与传染性评估；专利报复条款；中国司法判例（GPL 传染性）；木兰许可证 |
| `/claude-for-legal-china-IP:portfolio` | 组合期限管理 | 专利年费、商标续展、软著、集成电路布图设计全生命周期监控；滞纳期/宽展期/恢复权利提醒 |
| `/claude-for-legal-china-IP:claim-chart-builder` | 权利要求比对表 | 专利侵权技术特征比对表；无效宣告证据对照表；功能性特征分解 |
| `/claude-for-legal-china-IP:cease-desist` | 律师函/警告函 | 侵权警告函（公司名义）/律师函（律所名义）；诉前调解意向；行为保全警告；反向风险提示 |
| `/claude-for-legal-china-IP:takedown` | 网络侵权投诉 | 电商平台投诉书（阿里/京东/拼多多/微信/抖音）；ISP 通知函；恶意通知赔偿风险提示 |

## 包含智能体（Agents）

| 智能体 | 说明 |
|:---|:---|
| `ip-renewal-watcher` | 周期性扫描专利年费、商标续展、软著期限；红灯（已过期）/黄灯（30天内）/蓝灯（90天内）三级预警 |

## 快速开始

### 1. 环境要求

- **Claude Code** 或 **Claude Desktop**（需支持插件系统）
- **模型接入**：
  - 一般场景：国内备案大模型 API（通义千问 / 文心一言 / 智谱 GLM）
  - 涉密场景：私有化部署模型（DeepSeek-V3/R1 / Qwen2.5-72B / 昇腾生态）
- **知识库**：向量数据库（Milvus / pgvector / Elasticsearch）
- **外部数据源**（可选但强烈建议）：
  - 专利检索：incoPat / 智慧芽 / Patentics
  - 法律数据库：北大法宝 / 威科先行 / 知产宝
  - 商标检索：权大师 / 摩知轮 / 白兔

### 2. 安装

```bash
# 添加插件市场
/plugin marketplace add C:\Users\Administrator\claude-for-legal-china-IP

# 安装插件
/plugin install claude-for-legal-china-IP

# 重启 Claude Code
```

### 3. 初始化（关键！）

每个插件首次使用必须运行冷启动面试，建立企业实践画像：

```bash
/claude-for-legal-china-IP:cold-start-interview
```

面试将询问：
- 企业技术领域与 IPC 分类倾向
- 现有知识产权组合规模
- 外部服务机构名录
- 审批权限矩阵
- 保密分级规则

面试结果写入 `~/.claude/plugins/config/claude-for-legal-china-IP/CLAUDE.md`，所有技能执行前都会读取该文件。**跳过此步骤是输出质量差的最常见原因。**

### 4. 使用示例

```bash
# 评估一份专利交底书
/claude-for-legal-china-IP:invention-intake ./交底书-大气监测传感器融合方法.md

# 对新产品进行 FTO 初筛
/claude-for-legal-china-IP:fto-triage ./产品技术白皮书.md --tech-field G01N

# 审查技术合同中的 IP 条款
/claude-for-legal-china-IP:ip-clause-review ./技术开发合同-草案.docx

# 检查开源合规
/claude-for-legal-china-IP:oss-review ./SBOM.json
```

## 企业知识库（Playbook）

本插件的核心定制机制是 `CLAUDE.md`（实践档案）。企业须将以下信息录入：

1. **企业概况**：行业、技术领域、组合规模、布局策略
2. **职务发明制度**：权属、奖励报酬、离职 1 年规则
3. **合同审查红线**：源代码披露、政府项目权属、委托/合作开发规则
4. **外部服务机构**：代理所、律所、检索服务商
5. **审批权限矩阵**：预算阈值、发函权限、诉讼审批、数据出境否决权
6. **侵权维权策略**：维权优先级、发函策略、行政/司法双轨选择
7. **保密分级**：公开/内部/秘密/机密/绝密五级，及对应的 AI 输入规则
8. **开源政策**：允许/审查/禁止使用的许可证清单

详见 [`CLAUDE.md`](./CLAUDE.md) 模板。

## 数据源替换对照表

| 原版数据源 | 中国法替代方案 |
|:---|:---|
| USPTO TESS / Trademarkia | 中国商标网 + 权大师 / 摩知轮 / 白兔 |
| Google Patents / USPTO PAIR | CNIPA 专利检索系统 + incoPat / 智慧芽 / Patentics |
| CourtListener / PACER | 中国裁判文书网 + 知产宝 / 北大法宝 / 理脉 |
| Westlaw / Practical Law | 北大法宝 / 威科先行 / Alpha 法律智能 |
| Docket 系统 | 中国审判流程信息公开网 + 各地法院电子诉讼平台 |
| 无效决定数据库 | CNIPA 复审无效决定数据库 + 知识产权出版社数据库 |

## 风险与责任声明

1. **AI 输出不等于法律意见**：所有技能输出均为初评草稿，必须经具备中国执业资格的专利代理师/律师审核后方可作为决策依据。
2. **禁止直接提交官方文件**：AI 不得直接生成并向 CNIPA、商标局、法院等官方机构提交申请文件或法律文书。
3. **涉密信息处理**：未公开专利交底书、核心技术资料、商业秘密须按企业保密分级输入对应模型；绝密信息禁止输入任何 AI 系统。
4. **法律更新滞后**：知识库更新频率取决于企业维护，模型可能引用已失效的法条，所有引用须人工核实。
5. **开源许可证风险**：AI 对 GPL 传染性的判断仅供参考，最终合规责任由企业承担。

## 贡献与二次开发

本插件基于 Anthropic `claude-for-legal`（Apache 2.0）修改，遵循相同协议。欢迎：

- Fork 并适配其他行业/企业
- 提交新的中国法 skills（如植物新品种、地理标志、集成电路布图设计）
- 补充地方法院裁判规则（如北京知识产权法院、上海知识产权法院、广州知识产权法院、最高人民法院知识产权法庭）

开发规范：
- 所有技能文件为 Markdown（`SKILL.md`）
- 所有配置为 JSON
- 法律引用须标注具体条款号
- 须包含 `[需人工核实]` 门控机制

## 路线图

- [x] v1.0 核心技能：冷启动面试、交底书初筛、FTO、商标清查、侵权初筛、合同审查、开源合规、组合管理、比对表、律师函、网络投诉
- [ ] v1.1 增加：专利无效宣告策略技能、专利侵权诉讼技能、商业秘密刑事报案指引
- [ ] v1.2 增加：标准必要专利（SEP）FRAND 评估、专利质押融资审查、知识产权海关保护备案
- [ ] v2.0 对接：incoPat API、北大法宝 API、智慧芽 API，实现检索自动化

---

*本插件知识产权本土化方案由 Kimi Code CLI 辅助制定，基于《中华人民共和国专利法》《商标法》《著作权法》《反不正当竞争法》及现行司法解释。*

