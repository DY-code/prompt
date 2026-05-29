# 思维流与逻辑链
```
- 研究主线：基于 RL 探索学习的视频任务跟踪模仿 [进行中] (更新时间：2026-05-29)
    # 参考【ai research 功能界定】【思维流&逻辑链：记录研究思维过程】
    - 核心边界 [已确认] (更新时间：2026-05-22)
        # 不是直接模仿学习；视频用于引导 RL 探索，最终策略由环境反馈和任务对象状态成功定义。
        - 人类操作视频提供任务状态序列、接触区域、动作方向和阶段顺序 [已确认]
        - 机器人通过探索学习自身控制策略，而不是复现人类低层动作 [已确认]
        - 研究重点从“奖励设计”扩展为“探索行为和过程的引导” [已确认]
    - 统一问题抽象 [已确认] (更新时间：2026-05-29)
        # 参考【参考依据和支撑】；该抽象为 agent 推理/假设。
        - 视频结构化引导：
            $$
            \text{human video}\rightarrow \mathcal{G}(V)
            $$
        - guided RL exploration：
            $$
            \mathcal{G}(V)\rightarrow \text{guided RL exploration}\rightarrow \pi(a|h_t)
            $$
        - 可部署策略：
            $$
            h_t=(o_{t-H:t},a_{t-H:t})
            $$

- 研究任务 1：视频参考 RL 的探索引导 [已完成] (更新时间：2026-05-22)
    # 参考【研究目标与细节澄清】【研究过程要求】【研究成果交付】【论文检索】【论文阅读】
    - 任务固化 [已完成] (更新时间：2026-05-21)
        - 任务文档：`task/2026-05-21_video_guided_exploration.md`
        - 研究目标：探索如何利用人类操作视频引导 RL 采样任务相关 transition。
    - 核心问题 [已完成] (更新时间：2026-05-21)
        - 视频奖励是否足够 [已完成]
        - 如何从奖励设计扩展到探索过程引导 [已完成]
        - 如何纳入动作序列提取与重定向 [已完成]
    - 论文与知识整理 [已完成] (更新时间：2026-05-21)
        # 参考【论文阅读】；A/B/C 分类已写入 `notes/papers/`。
        - 视频表征与奖励：TCN、TCC、XIRL、VIP、Rank2Reward、Diffusion Reward [已完成]
        - 探索与 relabeling：HER、ICM、RND [已完成]
        - 表征与动作先验：R3M、SFV、GenMimic、HOLD [已完成]
        - 知识笔记：`notes/knowledge/` 与 `notes/routes/video_guided_exploration_routes.md` [已完成]
    - 当前结论 [已完成] (更新时间：2026-05-22)
        # 参考【参考依据和支撑】；以下为 agent 推理/假设，已在 submit 中标注依据。
        - 最有价值的方向不是单一视觉相似度 reward，而是视频结构化探索引导器 [已完成]
        - 推荐结构：
            $$
            \mathcal{G}(V)=\{G_V,\Phi,c,\tau^{retarget}\}
            $$
        - 组合机制：视频进度势能差分、video-HER、门控内在探索、动作/轨迹重定向先验、稀疏成功奖励 [已完成]
    - 交付成果 [已完成] (更新时间：2026-05-22)
        # 参考【研究成果交付】。
        - `submit/video_guided_exploration_2026_05_21/technical_review.md`
        - `submit/video_guided_exploration_2026_05_21/innovation_ideas.md`
        - `submit/video_guided_exploration_2026_05_21/references.md`

- 研究任务 2：Teacher-Student / Privileged RL 辅助视频引导探索 [已完成] (更新时间：2026-05-22)
    # 参考【研究目标与细节澄清】【研究过程要求】【研究成果交付】【论文检索】【论文阅读】
    - 任务固化 [已完成] (更新时间：2026-05-21)
        - 任务文档：`task/2026-05-21_teacher_student_privileged_rl.md`
        - 研究目标：研究如何把视频解析信号、对象真值、接触状态等训练期强先验迁移到可部署策略。
    - 核心问题 [已完成] (更新时间：2026-05-21)
        - teacher-student PPO / privileged learning 的基本路线 [已完成]
        - asymmetric critic、DAgger、RMA、CTS 之间的关系 [已完成]
        - 视频先验如何作为 training privilege [已完成]
        - 如何避免退化为纯 imitation [已完成]
    - 论文与知识整理 [已完成] (更新时间：2026-05-21)
        # 参考【论文阅读】；核心论文已按 A/B 类整理。
        - A 类：Lee et al. 2020、Ross et al. 2011、Pinto et al. 2017、Learning by Cheating、Privileged Action 2025 [已完成]
        - B 类：CTS、Hwangbo 2019、Teacher Motion Priors、RMA [已完成]
        - 路线笔记：`notes/routes/teacher_student_privileged_rl_routes.md` [已完成]
        - 知识笔记：`notes/knowledge/privileged_teacher_student_rl.md`、`notes/knowledge/teacher_student_for_video_guided_exploration.md` [已完成]
    - 当前结论 [已完成] (更新时间：2026-05-22)
        # 参考【参考依据和支撑】；以下为 agent 推理/假设，已在 submit 中标注依据。
        - 视频解析信号不一定进入最终策略，可先作为训练期 privilege [已完成]
        - 最佳方向：Video-Privileged Teacher + Deployable Student [已完成]
        - 总结构：
            $$
            \text{human video}\rightarrow p_t^V\rightarrow \pi_T(a_t|o_t,p_t^V)\rightarrow \pi_S(a_t|h_t)
            $$
        - student 训练应结合 DAgger、latent distillation 和 RL fine-tuning [已完成]
    - 交付成果 [已完成] (更新时间：2026-05-22)
        # 参考【研究成果交付】。
        - `submit/teacher_student_privileged_rl_2026_05_21/technical_review.md`
        - `submit/teacher_student_privileged_rl_2026_05_21/innovation_ideas.md`
        - `submit/teacher_student_privileged_rl_2026_05_21/references.md`
    - 待补事项 [待开始] (更新时间：2026-05-22)
        # 参考【参考依据和支撑】；不确定信息需显式标注。
        - RMA 本地 PDF 当前不完整，需补齐后更新页码级引用 [待开始]

- 研究任务 3：Action-Effect / Skill-Effect 因果表示用于视频引导 RL 探索 [暂停] (更新时间：2026-05-29)
    # 参考【研究目标与细节澄清】【思维流】【参考依据和支撑】；按 dy 要求，当前只固化任务，不启动系统研究。
    - 问题来源 [已确认] (更新时间：2026-05-29)
        - dy 关注：机器人如何理解自身动作/行为对环境的影响，并在新场景中想到可用动作/技能 [已确认]
        - 研究定位：action-effect / skill-effect causal representation [已确认]
    - 任务固化 [已完成] (更新时间：2026-05-29)
        - 任务文档：`task/2026-05-29_action_effect_skill_effect_causal_representation.md`
        - 当前要求：不要立刻开始研究，等待后续问题讨论 [已确认]
    - 核心抽象 [已确认] (更新时间：2026-05-29)
        # 参考【参考依据和支撑】；以下为 agent 推理/假设。
        - 技能效果模型：
            $$
            p(\Delta s^{obj}\mid s^{obj},z^{scene},k)
            $$
        - 新场景技能选择：
            $$
            k^*=\arg\max_k P(\Delta s^{goal}\mid s^{obj},z^{scene},k)
            $$
    - 待研究内容 [待开始] (更新时间：2026-05-29)
        # 参考【研究过程要求】【论文检索】【论文阅读】；尚未启动系统检索。
        - 动作效果表示：低层 action、动作原语、高层 skill 与 effect 表示 [待开始]
        - 从机器人交互中学习 action-effect：object-centric forward model、skill effect model、causal discovery [待开始]
        - 从人类视频提出 action-effect 假设：接触点、动作方向、对象变化、重定向 [待开始]
        - 技能库与 skill-effect memory：precondition、effect、confidence、failure memory [待开始]
        - 新场景技能选择与规划：goal-conditioned retrieval、effect-based planning、skill chaining [待开始]
        - 与视频引导 RL 主线结合：视频子目标选择技能，RL 优化技能参数 [待开始]
    - 已快速核查的强相关方向 [已完成] (更新时间：2026-05-29)
        # 参考【参考依据和支撑】；快速核查不替代后续系统论文阅读。
        - affordance / action-effect learning [已核查]
        - VRB / Vision-Robotics Bridge [已核查]
        - VAL / What Can I Do Here? [已核查]
        - object-centric forward model [已核查]
        - learned skill effect model [已核查]
        - causal robotics / CausalWorld [已核查]
    - 预期后续交付 [待开始] (更新时间：2026-05-29)
        # 参考【研究成果交付】；需 dy 确认启动后执行。
        - 强相关论文综述与 A/B/C 分类 [待开始]
        - action-effect / skill-effect 技术路线图 [待开始]
        - 与 video-guided exploration 和 privileged teacher-student 的整合框架 [待开始]
        - 可能创新点排序 [待开始]
        - 理论分析：效果模型如何降低探索复杂度 [待开始]
        - 可行验证方案和消融设计 [待开始]

- 仓库结构与记忆支线 [进行中] (更新时间：2026-05-29)
    # 参考【笔记仓库结构组织】【研究成果交付】。
    - 已有结构整理 [已完成] (更新时间：2026-05-22)
        - `task/`：研究任务文档 [已完成]
        - `notes/`：论文、知识点、路线笔记 [已完成]
        - `draft/`：理论草稿 [已完成]
        - `source/`：论文原文资料，PDF 默认不提交 Git [已完成]
        - `tools/`：工具说明 [已完成]
        - `submit/`：研究交付成果 [已完成]
    - 最新 agent.md 新增结构要求 [待处理] (更新时间：2026-05-29)
        # 参考【笔记仓库结构组织】。
        - `memory/`：用于保存新会话继承信息 [待开始]
        - `experiment/`：只读，用于保存实验反馈结果 [待开始]
        - 各子目录 README 中补充是否满足要求的简短检查报告 [待开始]
    - 仓库索引与阶段总结 [已完成] (更新时间：2026-05-29)
        - `notes/research_map.md`：任务、笔记、交付关系索引 [已完成]
        - `submit/repository_check_2026_05_22.md`：仓库自检记录 [已完成]
        - `submit/current_research_summary_2026_05_29.md`：当前研究阶段总结 [已完成]

- 执行规则状态 [进行中] (更新时间：2026-05-29)
    # 参考【研究目标与细节澄清】【思维流】【研究过程要求】。
    - 每轮研究前读取 `user/` [持续执行]
    - 每轮研究前澄清目标并等待 dy 确认 [持续执行]
    - 确认后将任务固化到 `task/` [持续执行]
    - 未确认前不擅自开展系统研究 [持续执行]
    - 关键结论标注论文依据或“推理/假设/待验证” [持续执行]
    - 完成任务节点后立即更新 `flow.md` 并写明时间戳 [持续执行]
```

---
时间戳：2026-05-29