# my-skills

各项目共用的 skill 上游。目前还没有启用，只记录方案。

## 现在的做法

- 每个项目在自己的 `skills/` 里维护全部 skill，跟着项目一起提交，公开与否跟项目一致。
- 新项目需要 skill 时，让 agent 从最接近的已有项目里抽出对应的 skill，再按新项目改。
- 已有的相近 skill：
  - code-author：rise-origin、neutrino、my-public-image、my-devices（repo-author）
  - doc-author：neutrino、my-devices、my-public-image
  - 写作句式规则：my-fellowship 的 thesis-author

## 以后的方案（真的觉得方便再做）

- 本仓库的 main 只放能公开的通用 role，例如 code-author、doc-author、common。
- 项目的 `skills/` 仍然是普通文件，不用 submodule，不用 subtree。
- 项目的 `skills/upstream.json` 记录：
  - 上游地址和分支（`main`）
  - `base`：上次合并到的上游 commit
  - `roles`：项目文件夹名对应到上游文件夹名，例如 `core-code-author -> code-author`
- `tools/sync.py pull` 对选中的 role 逐个文件做三方合并（`git merge-file`，base、项目、上游最新），合并完更新 `base`，不提交。
  - 两边都有：合并，冲突写入冲突标记
  - 上游新增：复制进项目
  - 上游删除：项目没改过就删，改过就保留并报出
  - 只有项目有：不动
- `tools/sync.py diff` 列出项目相对 `base` 的改动，挑出通用的部分手动提交到 main。
- 私有 role（thesis-author 这类）不进本仓库的 main。只一个项目用就放在项目里；几个私有项目共用，就另开一个私有上游，`upstream.json` 写多个上游，各记各的 `base`。

## TODO

- [ ] 确定哪些 role 公开，哪些私有
- [ ] 把 rise-origin 的 code-author 去掉项目内容后放进 main，作为第一版
- [ ] 写 `tools/sync.py`（只用 `git fetch`、`git show`、`git merge-file`）
- [ ] 用 neutrino 试第一次 pull：`base` 设为上游第一个 commit，所有差异以冲突出现，逐个决定
- [ ] 把 thesis-author 里的三条句式禁令（会做事的主语、逗号加 so、句尾挂 which）放进 common，作为写作规则的第一份内容
