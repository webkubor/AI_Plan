## 轻量扩展（不改模型、低风险）

为“提升命中率”，我建议用 轻量扩展（不改模型、低风险），有三种方案可选：

  1. 同义词/别名扩展（最稳）

  - 规则：对短 query 自动加别名
  - 例：Satori → Satori OR Vercel Satori OR OG Image OR Open Graph OR SVG

  2. 关键词增强模板（最简单）

  - 规则：把 query 包进一个“场景句”
  - 例：Satori → Satori 用于生成 OG 图片的库

  3. 标签回填（最准但需维护词表）

  - 规则：从 snippets/ 里维护 关键词： 行，检索时把关键词也拼进查询