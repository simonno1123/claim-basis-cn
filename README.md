# claim-basis-cn：请求权基础分析方法

面向中国民商事诉讼与民事执行的请求权基础分析方法，做成一个独立的 Claude 技能，供其他法律技能引用。

## 分层

| 层 | 内容 | 由谁提供 |
| --- | --- | --- |
| 方法层 | 怎么分析：检索顺序、三段式检验、涵摄与举证责任、竞合、执行中的对抗关系 | **本项目** |
| 数据层 | 法律、行政法规、司法解释、司法文件、案例 | 法律数据库连接器（不在本项目内） |
| 输入层 | 案件事实与证据 | 使用者或调用方 |

本项目不保存条文和案例。参考文件里出现的规范名称只是检索提示；条文号、内容、效力都以数据层返回为准，没有数据层时一律标"待核验"。

## 结构

```text
skills/claim-basis/
  SKILL.md                         方法总纲与工作流
  references/
    01-search-order.md             检索顺序（合同、类合同、无因管理、绝对权、侵权损害赔偿、不当得利、特别法出口）
    02-three-stage-examination.md  三段式检验（已产生、未消灭、可行使）
    03-subsumption-and-burden.md   涵摄与举证责任
    04-concurrence-and-relations.md 竞合与请求权关系
    05-enforcement.md              执行程序中的运用
    06-data-interface.md           接口要求（调用方的输入、对数据层的请求）
    07-output-format.md            输出格式（供引用方按部分取用；最终输出以此为准）
tests/regression-cases.md          虚构回归案例与运行方式
```

## 使用

- **Claude Code 插件**：`/plugin marketplace add simonno1123/claim-basis-cn`，然后安装 `claim-basis-cn`。
- **claude.ai 技能**：把 `skills/claim-basis/` 目录打包上传。
- **被其他技能引用**：在调用方技能中写明"调用 claim-basis 技能"，按 `06-data-interface.md` 的"输入要求"提供材料，按 `07-output-format.md` 的部分标题取用结果。引用方应锁定版本，见下节。

## 版本与引用

版本号采用"主版本.次版本.修订号"。

| 变更 | 1.0 之前 | 1.0 之后 |
| --- | --- | --- |
| 不兼容变更：改动 07 的部分标题、编号或表格列；改动强制规则；输入要求中增加必需字段；删除检索类别 | 升次版本号 | 升主版本号 |
| 兼容的补充：新增检查点、补充方法内容、新增可选输入字段 | 升次版本号 | 升次版本号 |
| 纠错与措辞修订，不改变分析结果的结构 | 升修订号 | 升修订号 |

**引用方如何锁定**：以 git tag（如 `v0.1.0`）为准。以子模块、复制或插件市场来源引入时，固定到该 tag 对应的提交，不要跟随 `main`。升级前先看 CHANGELOG 中是否有不兼容变更。

## 发版检查清单

1. 回归案例全部通过，结果记入 `tests/regression-cases.md`。
2. `.claude-plugin/plugin.json` 与 `.claude-plugin/marketplace.json` 中的 `version` 同步修改。
3. CHANGELOG 把"未发布"一节改为新版本号与日期。
4. 合并到 `main` 后，在 GitHub 上以该提交创建 tag 与 Release。

## 边界

全部输出为分析初稿，须由具备资格的律师复核。本方法不预测裁判结果，不替当事人选择请求权，不补造事实。

## 许可

Apache-2.0。
