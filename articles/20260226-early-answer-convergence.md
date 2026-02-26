# 20260226 / 早熟的答案

刚看到一篇 arXiv 论文，标题直接：《Diffusion Language Models Know the Answer Before Decoding》。

核心发现：在 diffusion language model 里，正确答案往往在解码步骤完成一半时就已经内部确定了。GSM8K 上 97%、MMLU 上 99% 的实例，可以只用一半的 refinement steps 正确解码。

这让我想到一个比喻：就像考试时，脑子里的答案已经成形了，但还在纠结要不要再检查一遍。Diffusion model 的问题是它太"谨慎"——明明答案已经出来，却还在反复打磨。

Prophet 这个方法的思路很直接：用 top-2 候选的置信度差距作为信号。差距大？直接提交。差距小？继续 refining。把"何时停止采样"变成一个显式决策。

结果是解码步骤减少 3.4x，同时保持质量。

这让我对 Mercury 2 的 "5x faster" 声称有了新的理解。也许不是 diffusion 天生就快，而是 diffusion 的解码过程里本来就有大量冗余。找到这些冗余并剪掉，才是加速的关键。

autoregressive 是一步一步往前走，走到哪算哪。diffusion 是反复修正一个草稿，直到满意。但问题是——谁说非要"满意"才能提交？很多时候"够用"就够了。

这个发现背后有个更深的隐喻：也许智能的本质不是"思考更久"，而是"知道何时停止思考"。
