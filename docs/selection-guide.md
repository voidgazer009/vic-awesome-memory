# 选型指南

```
需要人能直接读/改记忆吗？
├─ 是 → 文件优先：ai-memory / Basic Memory / GBrain / vic-memory
└─ 否
   ├─ 事实经常变化 / 需要审计时间线？ → Graphiti (Zep)
   ├─ 主要是"记住用户偏好"的聊天应用？ → Mem0 / Memobase / Supermemory
   ├─ 长期自主运行、要自我管理记忆？ → Letta / MemOS
   ├─ 编码 agent？ → claude-mem / agentmemory / ByteRover
   ├─ 大量文档 + 记忆一体？ → Cognee / OpenViking
   └─ 多跳关联推理？ → HippoRAG
```

## 五个必问问题
1. **许可证**：AGPL（OpenViking、Honcho、Basic Memory）用于商业 SaaS 要谨慎
2. **开源版 ≠ 托管版**：确认你要的功能在开源版里（如 Mem0 图记忆已移出 OSS）
3. **写入成本**：每轮几次 LLM 调用？延迟能接受吗？
4. **删除能力**：能否精确删除某个用户的全部记忆（GDPR）？
5. **可迁移**：能否导出成通用格式？换系统会不会全部丢失？
