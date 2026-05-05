# 怪物结构化索引

> 本文件由上一版结构化怪物索引转换而来，保留原机制、危险点、打法、推荐准备和附录。上一版数据文件已归档，不参与常规 RAG。

## 使用规则

- 优先用 `act + tier + name/id` 检索。
- 若下方“Boss/精英 MCP 映射补充”与原快速索引冲突，优先使用映射补充；原快速索引保留原始迁移状态。
- 原文缺少 MCP 英文名的条目保留 `needs_mcp_english_mapping`，等待后续人工或实战映射。
- hp、意图序列、威胁等级仅在原文明确给出时填写；未给出则显示“未提供”。

## Boss/精英 MCP 映射补充

> 本节用于解决实战 MCP 返回英文 `entity_id/model` 时查不到中文攻略的问题。`confirmed_from_mcp_or_legacy` 可直接用于检索；`likely_translation` 只能作为候选，命中后仍要用当前敌人意图和描述二次确认。

| canonical_id | act | tier | 中文名 | MCP/英文候选 | entity_id 提示 | 映射可信度 | 威胁等级 | 快速打法/机制 |
|---|---:|---|---|---|---|---|---|---|
| `act1_elite_bygone_effigy` | 1 | elite | 旧日雕像 | Bygone Effigy | `BYGONE_EFFIGY_*` | confirmed_from_source_heading | high | 第一幕精英，按当前意图决定攻防；若有召唤/状态压力，优先减少后续回合压力。 |
| `act1_elite_donusi_bird` | 1 | elite | 多尼斯异鸟 | Donusi Bird / Donus Bird | unknown | likely_translation; needs_review | medium_high | 飞行/多段倾向时优先用多段或低费攻击处理机制；血量低时不贪输出。 |
| `act1_elite_frog_parasite` | 1 | elite | 异蛙寄生虫 | Frog Parasite | unknown | likely_translation; needs_review | high | 若出现寄生/状态牌压力，优先缩短战斗；状态牌进入牌组后提高消耗牌价值。 |
| `act1_elite_writhing_worm` | 1 | elite | 扭动虫 | Writhing Worm | unknown | likely_translation; needs_review | high | 按大攻击回合优先防御；若有强化/蓄力，提前准备弱化或斩杀线。 |
| `act1_boss_ritual_beast` | 1 | boss | 仪式兽 | Ritual Beast | unknown | likely_translation; needs_review | high | 第一幕 Boss；若有仪式/强化节奏，越拖越危险。优先建立输出，同时保留关键防御药水。 |
| `act1_boss_clan_priest_followers` | 1 | boss | 同族神官 + 同族信徒 | Clan Priest + Clan Follower / Same-Clan Priest | unknown | likely_translation; needs_review | high | 多目标 Boss。优先识别治疗/增益来源；如果神官会强化随从，先压制支援目标。 |
| `act1_boss_vantom` | 1 | boss | 墨影幻灵 | Vantom | `VANTOM_*` | confirmed_from_source_heading_and_legacy | high | 有滑溜层数时优先用多张低费攻击剥层；不要把前几回合全部能量花在纯防，除非 `death_risk=true`。 |
| `act2_elite_slaughter_centipede` | 2 | elite | 残杀千足虫 | Slaughter Centipede / Centipede | unknown | likely_translation; needs_review | high | 多段/成长型精英按力量和脆弱风险处理；低防牌组避免硬吃连续攻击。 |
| `act2_elite_swarm_warlock` | 2 | elite | 蜂群术士 | Swarm Warlock | unknown | likely_translation; needs_review | high | 若召唤蜂群或叠 debuff，优先处理会放大伤害的来源；AOE 价值上升。 |
| `act2_elite_infected_prism` | 2 | elite | 感染棱柱 | Infected Prism | unknown | likely_translation; needs_review | high | 状态/感染压力会污染牌组；Burning Pact/True Grit/Fiend Fire 等消耗牌优先级上升。 |
| `act2_boss_knowledge_demon` | 2 | boss | 知识恶魔 | Knowledge Demon | `KNOWLEDGE_DEMON_*` | likely_translation_from_legacy; needs_mcp_confirmation | high | 长线 Boss，Act2 后厚牌组需要消耗和抽牌；低血通过时 Act3 路线必须保守。 |
| `act2_boss_crusher_rocket` | 2 | boss | 碾碎爪 + 火箭 | Crusher + Rocket | `CRUSHER_*`, `ROCKET_*` | confirmed_from_legacy_mcp_entity_ids | extreme | 包围双 Boss，任一死亡会触发另一方蟹之怒：+力量和 99 格挡。不要过早斩单边；每回合先转向最大攻击来源，再算防御。Rocket 强化后可能出现 33-49 单击，缺 30+ 稳定格挡/防御药水时极危险。 |
| `act2_boss_insatiable_sandworm` | 2 | boss | 无厌沙虫 | Insatiable Sandworm / Sandworm | unknown | likely_translation; needs_review | high | 开局会加入逃离/沙坑倒计时。若斩杀远，适量打逃离延长倒计时；若斩杀近，不要过度浪费能量。 |
| `act3_elite_horror_eel` | 3 | elite | 骇鳗 | Horror Eel | unknown | likely_translation; needs_review | high | 高伤精英按最新意图防御；若有电/缠绕类机制，优先避免下回合死亡线。 |
| `act3_elite_sneaky_coral` | 3 | elite | 鬼祟珊瑚群 | Sneaky Coral | unknown | likely_translation; needs_review | high | 多目标/隐藏机制可能拖慢斩杀；AOE 和精准目标切换都要按 MCP 实体确认。 |
| `act3_elite_spectral_knight` | 3 | elite | 幽灵骑士 | Spectral Knight / Ghost Knight | unknown | likely_translation; needs_review | high | 若有灵体/无形或状态惩罚，优先确认当回合实际可造成伤害，避免浪费爆发。 |
| `act3_elite_magic_knight` | 3 | elite | 魔法骑士 | Magic Knight | unknown | likely_translation; needs_review | high | debuff/法术压力大时，弱化、防御药水和快速斩杀优先级上升。 |
| `act3_elite_flail_knight` | 3 | elite | 连枷骑士 | Flail Knight | unknown | likely_translation; needs_review | high | 多段攻击常见时，力量降低/虚弱和高格挡优先。 |
| `act3_elite_garden_ghost_eel` | 3 | elite | 花园幽灵鳗 | Garden Ghost Eel | unknown | likely_translation; needs_review | high | 若出现隐匿/规避机制，先用低成本牌测试与剥机制，再交高伤牌。 |
| `act3_elite_mech_knight` | 3 | elite | 机甲骑士 | Mech Knight | unknown | likely_translation; needs_review | high | 护甲/蓄力回合需要提前准备易伤和爆发；大攻击回合优先活过当回合。 |
| `act3_elite_soul_nexus` | 3 | elite | 灵魂枢纽 | Soul Nexus | unknown | likely_translation; needs_review | high | 若有召唤/链接/灵魂机制，优先确认击杀顺序，避免杀错目标触发惩罚。 |
| `act3_boss_door_doormaker` | 3 | boss | 门扉 + 门扉缔造者 | Door + Doormaker | `DOOR_*`, `DOORMAKER_*` | confirmed_from_source_heading_and_legacy | extreme | 紧握回合每出牌额外失能量，先打灵体/放血/祭品等关键资源，再交高价值攻击；饥饿回合会让牌消耗，反而可配合 Feel No Pain/Dark Embrace。 |
| `act3_boss_queen` | 3 | boss | 女王 | Queen | `QUEEN_*` | likely_translation_from_legacy; needs_mcp_confirmation | extreme | 关注召唤、链结和 debuff；Boss 战药水不要省，先保证不进入下回合死亡线。 |

## 快速索引

| id | act | tier | 中文名 | 英文名 | 映射状态 |
|---|---:|---|---|---|---|
| `act1_normal_needs_mcp_mapping_001` | 1 | normal | 蛮兽 | 未提供 | needs_mcp_english_mapping |
| `act1_normal_needs_mcp_mapping_002` | 1 | normal | 墨宝 | 未提供 | needs_mcp_english_mapping |
| `act1_normal_needs_mcp_mapping_003` | 1 | normal | 劫掠者刺客 | 未提供 | needs_mcp_english_mapping |
| `act1_normal_needs_mcp_mapping_004` | 1 | normal | 劫掠者弩手 | 未提供 | needs_mcp_english_mapping |
| `act1_normal_needs_mcp_mapping_005` | 1 | normal | 劫掠者斧手 | 未提供 | needs_mcp_english_mapping |
| `act1_normal_needs_mcp_mapping_006` | 1 | normal | 小啃兽 | 未提供 | needs_mcp_english_mapping |
| `act1_normal_needs_mcp_mapping_007` | 1 | normal | 雾菇 | 未提供 | needs_mcp_english_mapping |
| `act1_normal_needs_mcp_mapping_008` | 1 | normal | 毛绒伏地虫 | 未提供 | needs_mcp_english_mapping |
| `act1_normal_needs_mcp_mapping_009` | 1 | normal | 藤蔓蹒跚者 | 未提供 | needs_mcp_english_mapping |
| `act1_normal_needs_mcp_mapping_010` | 1 | normal | 飞蝇菌子 | 未提供 | needs_mcp_english_mapping |
| `act1_normal_needs_mcp_mapping_011` | 1 | normal | 缩小甲虫 | 未提供 | needs_mcp_english_mapping |
| `act1_normal_needs_mcp_mapping_012` | 1 | normal | 树枝史莱姆（小） | 未提供 | needs_mcp_english_mapping |
| `act1_elite_bygone_effigy` | 1 | elite | 旧日雕像 | Bygone Effigy | mapped_from_source_heading |
| `act1_elite_needs_mcp_mapping_013` | 1 | elite | 多尼斯异鸟 | 未提供 | needs_mcp_english_mapping |
| `act1_elite_needs_mcp_mapping_014` | 1 | elite | 异蛙寄生虫 | 未提供 | needs_mcp_english_mapping |
| `act1_elite_needs_mcp_mapping_015` | 1 | elite | 扭动虫 | 未提供 | needs_mcp_english_mapping |
| `act1_boss_needs_mcp_mapping_016` | 1 | boss | 仪式兽 | 未提供 | needs_mcp_english_mapping |
| `act1_boss_needs_mcp_mapping_017` | 1 | boss | 同族神官 + 同族信徒 | 未提供 | needs_mcp_english_mapping |
| `act1_boss_vantom` | 1 | boss | 墨影幻灵 | Vantom | mapped_from_source_heading |
| `act2_normal_needs_mcp_mapping_018` | 2 | normal | 猎人杀手 | 未提供 | needs_mcp_english_mapping |
| `act2_normal_needs_mcp_mapping_019` | 2 | normal | 地道虫 | 未提供 | needs_mcp_english_mapping |
| `act2_normal_needs_mcp_mapping_020` | 2 | normal | 啃咬机 | 未提供 | needs_mcp_english_mapping |
| `act2_normal_needs_mcp_mapping_021` | 2 | normal | 结实的卵 | 未提供 | needs_mcp_english_mapping |
| `act2_normal_needs_mcp_mapping_022` | 2 | normal | 虱虫之祖 | 未提供 | needs_mcp_english_mapping |
| `act2_normal_needs_mcp_mapping_023` | 2 | normal | 熟睡甲虫 | 未提供 | needs_mcp_english_mapping |
| `act2_normal_needs_mcp_mapping_024` | 2 | normal | 棘刺蟾蜍 | 未提供 | needs_mcp_english_mapping |
| `act2_normal_needs_mcp_mapping_025` | 2 | normal | 盛碗虫系列 | 未提供 | needs_mcp_english_mapping |
| `act2_normal_needs_mcp_mapping_026` | 2 | normal | 偷窃草蜢 | 未提供 | needs_mcp_english_mapping |
| `act2_normal_needs_mcp_mapping_027` | 2 | normal | 外骨骼虫 | 未提供 | needs_mcp_english_mapping |
| `act2_normal_needs_mcp_mapping_028` | 2 | normal | 异螨 | 未提供 | needs_mcp_english_mapping |
| `act2_normal_needs_mcp_mapping_029` | 2 | normal | 直飞产卵虫 | 未提供 | needs_mcp_english_mapping |
| `act2_elite_needs_mcp_mapping_030` | 2 | elite | 残杀千足虫 | 未提供 | needs_mcp_english_mapping |
| `act2_elite_needs_mcp_mapping_031` | 2 | elite | 蜂群术士 | 未提供 | needs_mcp_english_mapping |
| `act2_elite_needs_mcp_mapping_032` | 2 | elite | 感染棱柱 | 未提供 | needs_mcp_english_mapping |
| `act2_boss_needs_mcp_mapping_033` | 2 | boss | 知识恶魔 | 未提供 | needs_mcp_english_mapping |
| `act2_boss_needs_mcp_mapping_034` | 2 | boss | 碾碎爪 | 未提供 | needs_mcp_english_mapping |
| `act2_boss_needs_mcp_mapping_035` | 2 | boss | 无厌沙虫 | 未提供 | needs_mcp_english_mapping |
| `act3_normal_needs_mcp_mapping_036` | 3 | normal | 失落之物 | 未提供 | needs_mcp_english_mapping |
| `act3_normal_needs_mcp_mapping_037` | 3 | normal | 活雾 | 未提供 | needs_mcp_english_mapping |
| `act3_normal_needs_mcp_mapping_038` | 3 | normal | 猫头鹰法官 | 未提供 | needs_mcp_english_mapping |
| `act3_normal_needs_mcp_mapping_039` | 3 | normal | 巨斧机器人 | 未提供 | needs_mcp_english_mapping |
| `act3_normal_needs_mcp_mapping_040` | 3 | normal | 幽灵船 | 未提供 | needs_mcp_english_mapping |
| `act3_normal_needs_mcp_mapping_041` | 3 | normal | 高塔炮手 | 未提供 | needs_mcp_english_mapping |
| `act3_normal_needs_mcp_mapping_042` | 3 | normal | 地精佣兵 | 未提供 | needs_mcp_english_mapping |
| `act3_normal_needs_mcp_mapping_043` | 3 | normal | 组装师 | 未提供 | needs_mcp_english_mapping |
| `act3_normal_needs_mcp_mapping_044` | 3 | normal | 双尾鼠 | 未提供 | needs_mcp_english_mapping |
| `act3_normal_needs_mcp_mapping_045` | 3 | normal | 活体盾 | 未提供 | needs_mcp_english_mapping |
| `act3_normal_needs_mcp_mapping_046` | 3 | normal | 海洋混混 | 未提供 | needs_mcp_english_mapping |
| `act3_normal_needs_mcp_mapping_047` | 3 | normal | 化石追踪者 | 未提供 | needs_mcp_english_mapping |
| `act3_normal_needs_mcp_mapping_048` | 3 | normal | 淤泥旋螺 | 未提供 | needs_mcp_english_mapping |
| `act3_normal_needs_mcp_mapping_049` | 3 | normal | 蟾蜍蝌蚪 | 未提供 | needs_mcp_english_mapping |
| `act3_normal_needs_mcp_mapping_050` | 3 | normal | 青蛙骑士 | 未提供 | needs_mcp_english_mapping |
| `act3_normal_needs_mcp_mapping_051` | 3 | normal | 电球头 | 未提供 | needs_mcp_english_mapping |
| `act3_normal_needs_mcp_mapping_052` | 3 | normal | 拳击构装体 | 未提供 | needs_mcp_english_mapping |
| `act3_normal_needs_mcp_mapping_053` | 3 | normal | 咬人卷轴 | 未提供 | needs_mcp_english_mapping |
| `act3_normal_needs_mcp_mapping_054` | 3 | normal | 虔诚雕刻师 | 未提供 | needs_mcp_english_mapping |
| `act3_normal_needs_mcp_mapping_055` | 3 | normal | 史莱姆狂战士 | 未提供 | needs_mcp_english_mapping |
| `act3_elite_needs_mcp_mapping_056` | 3 | elite | 骇鳗 | 未提供 | needs_mcp_english_mapping |
| `act3_elite_needs_mcp_mapping_057` | 3 | elite | 鬼祟珊瑚群 | 未提供 | needs_mcp_english_mapping |
| `act3_elite_needs_mcp_mapping_058` | 3 | elite | 幽灵骑士 | 未提供 | needs_mcp_english_mapping |
| `act3_elite_needs_mcp_mapping_059` | 3 | elite | 魔法骑士 | 未提供 | needs_mcp_english_mapping |
| `act3_elite_needs_mcp_mapping_060` | 3 | elite | 连枷骑士 | 未提供 | needs_mcp_english_mapping |
| `act3_elite_needs_mcp_mapping_061` | 3 | elite | 花园幽灵鳗 | 未提供 | needs_mcp_english_mapping |
| `act3_elite_needs_mcp_mapping_062` | 3 | elite | 机甲骑士 | 未提供 | needs_mcp_english_mapping |
| `act3_elite_needs_mcp_mapping_063` | 3 | elite | 灵魂枢纽 | 未提供 | needs_mcp_english_mapping |
| `act3_boss_door_doormaker` | 3 | boss | 门扉 + 门扉缔造者 | Door + Doormaker | mapped_from_source_heading |
| `act3_boss_needs_mcp_mapping_064` | 3 | boss | 灵魂异鱼 | 未提供 | needs_mcp_english_mapping |
| `act3_boss_needs_mcp_mapping_065` | 3 | boss | 火炬头聚合体 | 未提供 | needs_mcp_english_mapping |
| `act3_boss_needs_mcp_mapping_066` | 3 | boss | 实验体系列 | 未提供 | needs_mcp_english_mapping |
| `act3_boss_needs_mcp_mapping_067` | 3 | boss | 乐加维林族母 | 未提供 | needs_mcp_english_mapping |
| `act3_boss_needs_mcp_mapping_068` | 3 | boss | 瀑布巨兽 | 未提供 | needs_mcp_english_mapping |
| `act3_boss_needs_mcp_mapping_069` | 3 | boss | 女王 | 未提供 | needs_mcp_english_mapping |

## 第 1 幕

### 普通怪

#### 蛮兽

- id: `act1_normal_needs_mcp_mapping_001`
- raw_heading: 蛮兽
- act: 1
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

高基础攻击，常见行动包括高伤害撕扯、易伤、普通攻击。

**危险点**

易伤后接攻击会造成明显掉血。

**打法**

第一幕典型输出检查。前两轮应优先打伤害，不要过度攒慢速能力。被上易伤后，下一回合优先防御。

**推荐准备**

早期拿 1-2 张高效攻击牌，例如破击、重锤、剑柄打击、踩踏。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 高基础攻击，常见行动包括高伤害撕扯、易伤、普通攻击。 |

| 危险点 | 易伤后接攻击会造成明显掉血。 |

| 打法 | 第一幕典型输出检查。前两轮应优先打伤害，不要过度攒慢速能力。被上易伤后，下一回合优先防御。 |

| 推荐准备 | 早期拿 1-2 张高效攻击牌，例如破击、重锤、剑柄打击、踩踏。 |

</details>

#### 墨宝

- id: `act1_normal_needs_mcp_mapping_002`
- raw_heading: 墨宝
- act: 1
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

小体型敌人，行动包括低伤害、多段攻击和中等伤害。

**危险点**

多只同时出现时，总伤害比单只看起来更高。

**打法**

优先点杀血量低、即将攻击的个体。不要为了完美防御拖太久。

**推荐准备**

群伤或低费攻击很有价值。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 小体型敌人，行动包括低伤害、多段攻击和中等伤害。 |

| 危险点 | 多只同时出现时，总伤害比单只看起来更高。 |

| 打法 | 优先点杀血量低、即将攻击的个体。不要为了完美防御拖太久。 |

| 推荐准备 | 群伤或低费攻击很有价值。 |

</details>

#### 劫掠者刺客

- id: `act1_normal_needs_mcp_mapping_003`
- raw_heading: 劫掠者刺客
- act: 1
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

第一幕普通敌人，常见高单段射击。

**危险点**

如果同时有其他敌人攻击，刺客的单段伤害会形成爆发回合。

**打法**

尽量在其高伤害回合前压低血量。没有足够防御时，优先杀刺客。

**推荐准备**

单体爆发、易伤、药水。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 第一幕普通敌人，常见高单段射击。 |

| 危险点 | 如果同时有其他敌人攻击，刺客的单段伤害会形成爆发回合。 |

| 打法 | 尽量在其高伤害回合前压低血量。没有足够防御时，优先杀刺客。 |

| 推荐准备 | 单体爆发、易伤、药水。 |

</details>

#### 劫掠者弩手

- id: `act1_normal_needs_mcp_mapping_004`
- raw_heading: 劫掠者弩手
- act: 1
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

射击与装填循环。装填回合压力较低。

**危险点**

攻击回合伤害高，装填后往往接高压回合。

**打法**

装填回合是输出窗口；攻击回合优先防御。

**推荐准备**

保留高伤害牌在装填回合打出。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 射击与装填循环。装填回合压力较低。 |

| 危险点 | 攻击回合伤害高，装填后往往接高压回合。 |

| 打法 | 装填回合是输出窗口；攻击回合优先防御。 |

| 推荐准备 | 保留高伤害牌在装填回合打出。 |

</details>

#### 劫掠者斧手

- id: `act1_normal_needs_mcp_mapping_005`
- raw_heading: 劫掠者斧手
- act: 1
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

攻击并加格挡，也有较高单段攻击。

**危险点**

防御型行动会拖长战斗。

**打法**

不要把伤害分散在多个敌人身上；集中火力避免它反复叠格挡。

**推荐准备**

易伤、穿透性高的爆发伤害。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 攻击并加格挡，也有较高单段攻击。 |

| 危险点 | 防御型行动会拖长战斗。 |

| 打法 | 不要把伤害分散在多个敌人身上；集中火力避免它反复叠格挡。 |

| 推荐准备 | 易伤、穿透性高的爆发伤害。 |

</details>

#### 小啃兽

- id: `act1_normal_needs_mcp_mapping_006`
- raw_heading: 小啃兽
- act: 1
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

攻击、攻击并格挡、加力量。

**危险点**

加力量后拖久会失控。

**打法**

如果它开始成长，优先击杀。可以接受前期少量掉血换快速斩杀。

**推荐准备**

第一幕前置伤害。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 攻击、攻击并格挡、加力量。 |

| 危险点 | 加力量后拖久会失控。 |

| 打法 | 如果它开始成长，优先击杀。可以接受前期少量掉血换快速斩杀。 |

| 推荐准备 | 第一幕前置伤害。 |

</details>

#### 雾菇

- id: `act1_normal_needs_mcp_mapping_007`
- raw_heading: 雾菇
- act: 1
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

可制造幻象/召唤物，同时攻击并成长。

**危险点**

幻象拖延目标选择，成长后伤害升高。

**打法**

如果召唤物本身压力不大，优先本体；若召唤物即将攻击且能一击清掉，则先清。

**推荐准备**

群伤、随机目标攻击、多段攻击。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 可制造幻象/召唤物，同时攻击并成长。 |

| 危险点 | 幻象拖延目标选择，成长后伤害升高。 |

| 打法 | 如果召唤物本身压力不大，优先本体；若召唤物即将攻击且能一击清掉，则先清。 |

| 推荐准备 | 群伤、随机目标攻击、多段攻击。 |

</details>

#### 毛绒伏地虫

- id: `act1_normal_needs_mcp_mapping_008`
- raw_heading: 毛绒伏地虫
- act: 1
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

低伤害行动与大幅力量成长。

**危险点**

“吸气/蓄力”后接下来的攻击会显著变危险。

**打法**

看到成长回合后，要把它当作倒计时敌人处理。

**推荐准备**

快速击杀能力。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 低伤害行动与大幅力量成长。 |

| 危险点 | “吸气/蓄力”后接下来的攻击会显著变危险。 |

| 打法 | 看到成长回合后，要把它当作倒计时敌人处理。 |

| 推荐准备 | 快速击杀能力。 |

</details>

#### 藤蔓蹒跚者

- id: `act1_normal_needs_mcp_mapping_009`
- raw_heading: 藤蔓蹒跚者
- act: 1
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

会施加缠绕类限制，也有多段和大单段攻击。

**危险点**

被限制后无法按计划防御或输出。

**打法**

负面状态回合要预判下一轮，避免手牌被限制后裸吃大伤害。

**推荐准备**

稳定防御、解场伤害、耗竭状态牌能力。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 会施加缠绕类限制，也有多段和大单段攻击。 |

| 危险点 | 被限制后无法按计划防御或输出。 |

| 打法 | 负面状态回合要预判下一轮，避免手牌被限制后裸吃大伤害。 |

| 推荐准备 | 稳定防御、解场伤害、耗竭状态牌能力。 |

</details>

#### 飞蝇菌子

- id: `act1_normal_needs_mcp_mapping_010`
- raw_heading: 飞蝇菌子
- act: 1
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

会施加易伤/脆弱，并有中等攻击。

**危险点**

脆弱降低防御质量，易伤放大后续伤害。

**打法**

能快速杀就快速杀；不能杀时，负面状态前后要留足防御。

**推荐准备**

单体爆发、药水。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 会施加易伤/脆弱，并有中等攻击。 |

| 危险点 | 脆弱降低防御质量，易伤放大后续伤害。 |

| 打法 | 能快速杀就快速杀；不能杀时，负面状态前后要留足防御。 |

| 推荐准备 | 单体爆发、药水。 |

</details>

#### 缩小甲虫

- id: `act1_normal_needs_mcp_mapping_011`
- raw_heading: 缩小甲虫
- act: 1
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

有“缩小”类特殊行动，并有啃咬/践踏等攻击。公开页面显示机制不完全，具体数值需以游戏内为准。

**危险点**

体型/状态变化可能改变后续伤害节奏。

**打法**

按成长型普通怪处理，不要拖。

**推荐准备**

早期稳定输出。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 有“缩小”类特殊行动，并有啃咬/践踏等攻击。公开页面显示机制不完全，具体数值需以游戏内为准。 |

| 危险点 | 体型/状态变化可能改变后续伤害节奏。 |

| 打法 | 按成长型普通怪处理，不要拖。 |

| 推荐准备 | 早期稳定输出。 |

</details>

#### 树枝史莱姆（小）

- id: `act1_normal_needs_mcp_mapping_012`
- raw_heading: 树枝史莱姆（小）
- act: 1
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

小型史莱姆类敌人。

**危险点**

多只出现时会放大低伤害频率。

**打法**

优先清理正在攻击的低血量单位。

**推荐准备**

群伤、低费攻击。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 小型史莱姆类敌人。 |

| 危险点 | 多只出现时会放大低伤害频率。 |

| 打法 | 优先清理正在攻击的低血量单位。 |

| 推荐准备 | 群伤、低费攻击。 |



---

</details>

### 精英

#### 旧日雕像 / Bygone Effigy

- id: `act1_elite_bygone_effigy`
- raw_heading: 旧日雕像 / Bygone Effigy
- act: 1
- tier: elite
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: mapped_from_source_heading
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

初始沉睡，随后醒来并获得大量力量，之后进入高伤害斩击循环。

**危险点**

醒来后的力量加成会让每回合伤害变得很高。

**打法**

不要过早无意义刮痧唤醒。准备好爆发、易伤和防御后再打醒；唤醒后应尽快斩杀。

**推荐准备**

爆发伤害、易伤、防御药水、力量药。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 初始沉睡，随后醒来并获得大量力量，之后进入高伤害斩击循环。 |

| 危险点 | 醒来后的力量加成会让每回合伤害变得很高。 |

| 打法 | 不要过早无意义刮痧唤醒。准备好爆发、易伤和防御后再打醒；唤醒后应尽快斩杀。 |

| 推荐准备 | 爆发伤害、易伤、防御药水、力量药。 |

</details>

#### 多尼斯异鸟

- id: `act1_elite_needs_mcp_mapping_013`
- raw_heading: 多尼斯异鸟
- act: 1
- tier: elite
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

低伤害啄击与高伤害俯冲。

**危险点**

俯冲回合若没有足够防御，会损失大量生命。

**打法**

低压回合输出，高压回合防御。若已经能斩杀，不要保守。

**推荐准备**

可靠防御、爆发伤害。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 低伤害啄击与高伤害俯冲。 |

| 危险点 | 俯冲回合若没有足够防御，会损失大量生命。 |

| 打法 | 低压回合输出，高压回合防御。若已经能斩杀，不要保守。 |

| 推荐准备 | 可靠防御、爆发伤害。 |

</details>

#### 异蛙寄生虫

- id: `act1_elite_needs_mcp_mapping_014`
- raw_heading: 异蛙寄生虫
- act: 1
- tier: elite
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

会施加感染/状态，并有多段攻击。

**危险点**

状态牌污染抽牌，多段攻击压低生命。

**打法**

牌组缺抽牌/耗竭时尤其要快速杀。不要让状态牌堆积到第二轮循环。

**推荐准备**

耗竭、抽牌、前置伤害。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 会施加感染/状态，并有多段攻击。 |

| 危险点 | 状态牌污染抽牌，多段攻击压低生命。 |

| 打法 | 牌组缺抽牌/耗竭时尤其要快速杀。不要让状态牌堆积到第二轮循环。 |

| 推荐准备 | 耗竭、抽牌、前置伤害。 |

</details>

#### 扭动虫

- id: `act1_elite_needs_mcp_mapping_015`
- raw_heading: 扭动虫
- act: 1
- tier: elite
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

啃咬、扭动成长、状态牌；召唤/生成时可能有眩晕。

**危险点**

力量成长加状态牌，会拖垮慢速牌组。

**打法**

优先处理成长个体；如果被召唤物只是眩晕一回合，可先打主威胁。

**推荐准备**

群伤、快速击杀。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 啃咬、扭动成长、状态牌；召唤/生成时可能有眩晕。 |

| 危险点 | 力量成长加状态牌，会拖垮慢速牌组。 |

| 打法 | 优先处理成长个体；如果被召唤物只是眩晕一回合，可先打主威胁。 |

| 推荐准备 | 群伤、快速击杀。 |



---

</details>

### Boss

#### 仪式兽

- id: `act1_boss_needs_mcp_mapping_016`
- raw_heading: 仪式兽
- act: 1
- tier: boss
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

高伤害攻击、力量成长、咆哮/特殊状态。

**危险点**

18+、17+ 等高伤害回合叠力量后会越来越难挡。

**打法**

不要用纯防御拖。第一幕 Boss 需要稳定输出和一点缩放；被成长后尽快结束。

**推荐准备**

易伤、力量、持续输出、防御药。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 高伤害攻击、力量成长、咆哮/特殊状态。 |

| 危险点 | 18+、17+ 等高伤害回合叠力量后会越来越难挡。 |

| 打法 | 不要用纯防御拖。第一幕 Boss 需要稳定输出和一点缩放；被成长后尽快结束。 |

| 推荐准备 | 易伤、力量、持续输出、防御药。 |

</details>

#### 同族神官 + 同族信徒

- id: `act1_boss_needs_mcp_mapping_017`
- raw_heading: 同族神官 + 同族信徒
- act: 1
- tier: boss
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

神官会用脆弱、虚弱、光束攻击和仪式类力量成长；信徒会攻击并获得力量。

**危险点**

多目标 + 负面状态 + 力量成长。

**打法**

通常先处理信徒，减少攻击来源；如果神官即将成长且信徒血量较高，视牌组伤害决定是否先压神官。

**推荐准备**

群伤、单体爆发、解除状态压力的抽牌/耗竭。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 神官会用脆弱、虚弱、光束攻击和仪式类力量成长；信徒会攻击并获得力量。 |

| 危险点 | 多目标 + 负面状态 + 力量成长。 |

| 打法 | 通常先处理信徒，减少攻击来源；如果神官即将成长且信徒血量较高，视牌组伤害决定是否先压神官。 |

| 推荐准备 | 群伤、单体爆发、解除状态压力的抽牌/耗竭。 |

</details>

#### 墨影幻灵 / Vantom

- id: `act1_boss_vantom`
- raw_heading: 墨影幻灵 / Vantom
- act: 1
- tier: boss
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: mapped_from_source_heading
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

具有滑溜/减伤类机制；部分资料显示前若干次受击只造成极低伤害。还会进行墨迹、长枪、多段/大伤害、准备成长等行动。

**危险点**

单发大伤害会被滑溜机制浪费；拖久后成长和大攻击造成压力。

**打法**

多段攻击和低伤害攻击更适合破滑溜。不要把所有爆发伤害打在减伤层数仍高时。先用低费/多段消耗机制，再打关键爆发。

**推荐准备**

多段攻击、低费攻击、稳定防御。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 具有滑溜/减伤类机制；部分资料显示前若干次受击只造成极低伤害。还会进行墨迹、长枪、多段/大伤害、准备成长等行动。 |

| 危险点 | 单发大伤害会被滑溜机制浪费；拖久后成长和大攻击造成压力。 |

| 打法 | 多段攻击和低伤害攻击更适合破滑溜。不要把所有爆发伤害打在减伤层数仍高时。先用低费/多段消耗机制，再打关键爆发。 |

| 推荐准备 | 多段攻击、低费攻击、稳定防御。 |



---

</details>

## 第 2 幕

### 普通怪

#### 猎人杀手

- id: `act2_normal_needs_mcp_mapping_018`
- raw_heading: 猎人杀手
- act: 2
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

会施加“嫩化/易伤类”负面，并有高单段或多段攻击。

**危险点**

负面后吃多段攻击很疼。

**打法**

负面回合后优先防御；能快速杀则不用保留资源。

**推荐准备**

防御密度、爆发。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 会施加“嫩化/易伤类”负面，并有高单段或多段攻击。 |

| 危险点 | 负面后吃多段攻击很疼。 |

| 打法 | 负面回合后优先防御；能快速杀则不用保留资源。 |

| 推荐准备 | 防御密度、爆发。 |

</details>

#### 地道虫

- id: `act2_normal_needs_mcp_mapping_019`
- raw_heading: 地道虫
- act: 2
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

会啃咬、潜地获得大量格挡，潜地后可能打出高伤害。

**危险点**

潜地回合打它效率低，下一轮伤害高。

**打法**

潜地前尽量输出；潜地后优先防御和准备下一轮，不要浪费爆发。

**推荐准备**

爆发窗口判断、格挡。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 会啃咬、潜地获得大量格挡，潜地后可能打出高伤害。 |

| 危险点 | 潜地回合打它效率低，下一轮伤害高。 |

| 打法 | 潜地前尽量输出；潜地后优先防御和准备下一轮，不要浪费爆发。 |

| 推荐准备 | 爆发窗口判断、格挡。 |

</details>

#### 啃咬机

- id: `act2_normal_needs_mcp_mapping_020`
- raw_heading: 啃咬机
- act: 2
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

多段夹击和尖叫/特殊行动。

**危险点**

多段攻击会吃掉防御不足的牌组。

**打法**

按中等威胁处理。若同场有成长型敌人，先处理成长型。

**推荐准备**

中等防御、群伤。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 多段夹击和尖叫/特殊行动。 |

| 危险点 | 多段攻击会吃掉防御不足的牌组。 |

| 打法 | 按中等威胁处理。若同场有成长型敌人，先处理成长型。 |

| 推荐准备 | 中等防御、群伤。 |

</details>

#### 结实的卵

- id: `act2_normal_needs_mcp_mapping_021`
- raw_heading: 结实的卵
- act: 2
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

可孵化/召唤，低伤害啃咬。

**危险点**

放任孵化会增加敌人数量。

**打法**

能在孵化前打掉就优先打掉；若本体敌人压力更高，则用群伤顺带处理。

**推荐准备**

群伤、前置输出。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 可孵化/召唤，低伤害啃咬。 |

| 危险点 | 放任孵化会增加敌人数量。 |

| 打法 | 能在孵化前打掉就优先打掉；若本体敌人压力更高，则用群伤顺带处理。 |

| 推荐准备 | 群伤、前置输出。 |

</details>

#### 虱虫之祖

- id: `act2_normal_needs_mcp_mapping_022`
- raw_heading: 虱虫之祖
- act: 2
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

会施加脆弱，攻击，蜷缩获得格挡并加力量。

**危险点**

格挡 + 力量成长会拖长战斗并增加后续伤害。

**打法**

看到成长/格挡行动后，要优先考虑斩杀窗口。不要让它多次成长。

**推荐准备**

爆发、易伤。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 会施加脆弱，攻击，蜷缩获得格挡并加力量。 |

| 危险点 | 格挡 + 力量成长会拖长战斗并增加后续伤害。 |

| 打法 | 看到成长/格挡行动后，要优先考虑斩杀窗口。不要让它多次成长。 |

| 推荐准备 | 爆发、易伤。 |

</details>

#### 熟睡甲虫

- id: `act2_normal_needs_mcp_mapping_023`
- raw_heading: 熟睡甲虫
- act: 2
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

初始睡眠/打鼾，醒来后攻击并增益。

**危险点**

和其他敌人同场时，唤醒时机决定压力。

**打法**

先清其他威胁或准备爆发后再唤醒；不要无计划地群伤唤醒。

**推荐准备**

可控攻击、爆发。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 初始睡眠/打鼾，醒来后攻击并增益。 |

| 危险点 | 和其他敌人同场时，唤醒时机决定压力。 |

| 打法 | 先清其他威胁或准备爆发后再唤醒；不要无计划地群伤唤醒。 |

| 推荐准备 | 可控攻击、爆发。 |

</details>

#### 棘刺蟾蜍

- id: `act2_normal_needs_mcp_mapping_024`
- raw_heading: 棘刺蟾蜍
- act: 2
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

获得尖刺，进行高伤害攻击或舌击。

**危险点**

尖刺惩罚多段攻击。

**打法**

尖刺高时避免无意义多段；用单段高伤害或非攻击伤害处理。

**推荐准备**

单段高伤害、防御。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 获得尖刺，进行高伤害攻击或舌击。 |

| 危险点 | 尖刺惩罚多段攻击。 |

| 打法 | 尖刺高时避免无意义多段；用单段高伤害或非攻击伤害处理。 |

| 推荐准备 | 单段高伤害、防御。 |

</details>

#### 盛碗虫系列

- id: `act2_normal_needs_mcp_mapping_025`
- raw_heading: 盛碗虫系列
- act: 2
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

未提供

**危险点**

未提供

**打法**

未提供

**推荐准备**

未提供

<details><summary>原始记录</summary>

| 变体 | 机制 | 打法 |

|---|---|---|

| 盛碗虫（卵） | 低血量，攻击并格挡。 | 尽早清掉，避免被格挡拖回合。 |

| 盛碗虫（石） | 中血量，有头槌/眩晕相关行动。 | 注意高伤害和控制；不要拖。 |

| 盛碗虫（丝） | 多段攻击、虚弱类负面。 | 被虚弱时输出变差，优先处理。 |

| 盛碗虫（蜜） | 公开资料显示有撕扯和强化类行动，具体数值需核对。 | 按成长型小怪处理，优先击杀。 |

</details>

#### 偷窃草蜢

- id: `act2_normal_needs_mcp_mapping_026`
- raw_heading: 偷窃草蜢
- act: 2
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

偷窃/抢夺/逃跑，可能获得机动增益。

**危险点**

拖久可能逃走，造成资源损失。

**打法**

有斩杀窗口时优先杀它；必要时用药水保收益。

**推荐准备**

爆发、易伤。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 偷窃/抢夺/逃跑，可能获得机动增益。 |

| 危险点 | 拖久可能逃走，造成资源损失。 |

| 打法 | 有斩杀窗口时优先杀它；必要时用药水保收益。 |

| 推荐准备 | 爆发、易伤。 |

</details>

#### 外骨骼虫

- id: `act2_normal_needs_mcp_mapping_027`
- raw_heading: 外骨骼虫
- act: 2
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

小血量，但可能有单次伤害上限或特殊减伤；还会攻击、激怒成长。

**危险点**

单发大伤害效率低；拖久成长。

**打法**

多段攻击、低费攻击、反复触发伤害更好。

**推荐准备**

多段攻击、群伤。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 小血量，但可能有单次伤害上限或特殊减伤；还会攻击、激怒成长。 |

| 危险点 | 单发大伤害效率低；拖久成长。 |

| 打法 | 多段攻击、低费攻击、反复触发伤害更好。 |

| 推荐准备 | 多段攻击、群伤。 |

</details>

#### 异螨

- id: `act2_normal_needs_mcp_mapping_028`
- raw_heading: 异螨
- act: 2
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

施加中毒/状态，啃咬，吸取并成长。

**危险点**

状态和成长叠加，拖久危险。

**打法**

优先击杀；如果已经中毒/状态多，要用抽牌或耗竭稳定循环。

**推荐准备**

前置伤害、耗竭。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 施加中毒/状态，啃咬，吸取并成长。 |

| 危险点 | 状态和成长叠加，拖久危险。 |

| 打法 | 优先击杀；如果已经中毒/状态多，要用抽牌或耗竭稳定循环。 |

| 推荐准备 | 前置伤害、耗竭。 |

</details>

#### 直飞产卵虫

- id: `act2_normal_needs_mcp_mapping_029`
- raw_heading: 直飞产卵虫
- act: 2
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

召唤、攻击、施加易伤、获得力量。

**危险点**

召唤 + 易伤 + 力量成长，典型不能拖。

**打法**

本体优先级很高。若召唤物即将大量攻击，用群伤清。

**推荐准备**

群伤、爆发、防御。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 召唤、攻击、施加易伤、获得力量。 |

| 危险点 | 召唤 + 易伤 + 力量成长，典型不能拖。 |

| 打法 | 本体优先级很高。若召唤物即将大量攻击，用群伤清。 |

| 推荐准备 | 群伤、爆发、防御。 |



---

</details>

### 精英

#### 残杀千足虫

- id: `act2_elite_needs_mcp_mapping_030`
- raw_heading: 残杀千足虫
- act: 2
- tier: elite
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

多节身体，头/中节/尾节可能分别行动。中节有多段攻击、成长、虚弱、死亡/重连治疗等机制。

**危险点**

只杀一段可能被重连或治疗；多段同时攻击压力大。

**打法**

尽量把各节血量压低后连续击杀。不要长期只打一个部位而放任其他部位成长/攻击。

**推荐准备**

群伤、可控斩杀、AOE 药水。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 多节身体，头/中节/尾节可能分别行动。中节有多段攻击、成长、虚弱、死亡/重连治疗等机制。 |

| 危险点 | 只杀一段可能被重连或治疗；多段同时攻击压力大。 |

| 打法 | 尽量把各节血量压低后连续击杀。不要长期只打一个部位而放任其他部位成长/攻击。 |

| 推荐准备 | 群伤、可控斩杀、AOE 药水。 |

</details>

#### 蜂群术士

- id: `act2_elite_needs_mcp_mapping_031`
- raw_heading: 蜂群术士
- act: 2
- tier: elite
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

召唤/蜂群类机制，喷洒信息素，攻击，并可能强化蜂群。

**危险点**

蜂群和本体同时施压；部分机制可能惩罚高频攻击。

**打法**

如果蜂群数量上升过快，优先清小怪；如果本体进入成长循环，直接压本体。

**推荐准备**

群伤、稳定格挡、爆发。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 召唤/蜂群类机制，喷洒信息素，攻击，并可能强化蜂群。 |

| 危险点 | 蜂群和本体同时施压；部分机制可能惩罚高频攻击。 |

| 打法 | 如果蜂群数量上升过快，优先清小怪；如果本体进入成长循环，直接压本体。 |

| 推荐准备 | 群伤、稳定格挡、爆发。 |

</details>

#### 感染棱柱

- id: `act2_elite_needs_mcp_mapping_032`
- raw_heading: 感染棱柱
- act: 2
- tier: elite
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

高单段攻击、辐射式攻击并格挡、多段旋风、脉动获得格挡和力量。

**危险点**

攻防一体，拖久力量成长很难处理。

**打法**

低压回合输出，高压回合防御。看到格挡+力量时要准备斩杀。

**推荐准备**

易伤、爆发、持续防御。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 高单段攻击、辐射式攻击并格挡、多段旋风、脉动获得格挡和力量。 |

| 危险点 | 攻防一体，拖久力量成长很难处理。 |

| 打法 | 低压回合输出，高压回合防御。看到格挡+力量时要准备斩杀。 |

| 推荐准备 | 易伤、爆发、持续防御。 |



---

</details>

### Boss

#### 知识恶魔

- id: `act2_boss_needs_mcp_mapping_033`
- raw_heading: 知识恶魔
- act: 2
- tier: boss
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

诅咒/知识超载类行动，多段攻击，思考时治疗并成长。

**危险点**

诅咒污染牌组，治疗成长拖长战斗。

**打法**

牌组越依赖循环，越要防止诅咒堆积。高伤害窗口要集中输出，减少治疗轮数。

**推荐准备**

耗竭、抽牌、爆发、力量缩放。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 诅咒/知识超载类行动，多段攻击，思考时治疗并成长。 |

| 危险点 | 诅咒污染牌组，治疗成长拖长战斗。 |

| 打法 | 牌组越依赖循环，越要防止诅咒堆积。高伤害窗口要集中输出，减少治疗轮数。 |

| 推荐准备 | 耗竭、抽牌、爆发、力量缩放。 |

</details>

#### 碾碎爪

- id: `act2_boss_needs_mcp_mapping_034`
- raw_heading: 碾碎爪
- act: 2
- tier: boss
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

多种攻击、弱化/脆弱、适应成长、攻击并格挡。

**危险点**

防御行动让战斗拖长，负面状态降低防御与输出。

**打法**

抓住未格挡或低防回合输出；负面状态回合不要贪。

**推荐准备**

稳定防御、易伤、缩放。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 多种攻击、弱化/脆弱、适应成长、攻击并格挡。 |

| 危险点 | 防御行动让战斗拖长，负面状态降低防御与输出。 |

| 打法 | 抓住未格挡或低防回合输出；负面状态回合不要贪。 |

| 推荐准备 | 稳定防御、易伤、缩放。 |

</details>

#### 无厌沙虫

- id: `act2_boss_needs_mcp_mapping_035`
- raw_heading: 无厌沙虫
- act: 2
- tier: boss
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

液化地面/状态、双段攻击、高伤害啃咬、唾液成长。

**危险点**

高伤害回合明显，状态牌会拖慢牌组。

**打法**

识别高伤害轮，提前留防御。不要让状态牌塞满第二轮循环。

**推荐准备**

抽牌、耗竭、防御、爆发。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 液化地面/状态、双段攻击、高伤害啃咬、唾液成长。 |

| 危险点 | 高伤害回合明显，状态牌会拖慢牌组。 |

| 打法 | 识别高伤害轮，提前留防御。不要让状态牌塞满第二轮循环。 |

| 推荐准备 | 抽牌、耗竭、防御、爆发。 |



---

</details>

## 第 3 幕

### 普通怪

#### 失落之物

- id: `act3_normal_needs_mcp_mapping_036`
- raw_heading: 失落之物
- act: 3
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

会削弱/偷取类机制并自我成长，也有多段激光。

**危险点**

如果它偷走力量或资源，战斗会变慢。

**打法**

通常优先击杀；被偷取的收益如果会返还，则更要确保能杀死它。

**推荐准备**

单体爆发、多段防御。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 会削弱/偷取类机制并自我成长，也有多段激光。 |

| 危险点 | 如果它偷走力量或资源，战斗会变慢。 |

| 打法 | 通常优先击杀；被偷取的收益如果会返还，则更要确保能杀死它。 |

| 推荐准备 | 单体爆发、多段防御。 |

</details>

#### 活雾

- id: `act3_normal_needs_mcp_mapping_037`
- raw_heading: 活雾
- act: 3
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

雾气/烟雾负面，召唤或膨胀，攻击。

**危险点**

召唤与状态叠加会拖垮牌组。

**打法**

看是否能快速打本体；否则用群伤同时处理召唤物。

**推荐准备**

群伤、耗竭、抽牌。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 雾气/烟雾负面，召唤或膨胀，攻击。 |

| 危险点 | 召唤与状态叠加会拖垮牌组。 |

| 打法 | 看是否能快速打本体；否则用群伤同时处理召唤物。 |

| 推荐准备 | 群伤、耗竭、抽牌。 |

</details>

#### 猫头鹰法官

- id: `act3_normal_needs_mcp_mapping_038`
- raw_heading: 猫头鹰法官
- act: 3
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

常规攻击、多段攻击、飞升/成长、审判大伤害并施加易伤。

**危险点**

审判类高伤害回合非常危险。

**打法**

预留防御应对大招；成长后要尽快结束。

**推荐准备**

大格挡、爆发。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 常规攻击、多段攻击、飞升/成长、审判大伤害并施加易伤。 |

| 危险点 | 审判类高伤害回合非常危险。 |

| 打法 | 预留防御应对大招；成长后要尽快结束。 |

| 推荐准备 | 大格挡、爆发。 |

</details>

#### 巨斧机器人

- id: `act3_normal_needs_mcp_mapping_039`
- raw_heading: 巨斧机器人
- act: 3
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

启动获得格挡与力量，多段攻击，磨斧成长，弱化/脆弱攻击。

**危险点**

力量成长后多段攻击伤害上升。

**打法**

不要让它安全启动多轮。能在成长后立刻压死最好。

**推荐准备**

穿透格挡的高伤害、稳定防御。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 启动获得格挡与力量，多段攻击，磨斧成长，弱化/脆弱攻击。 |

| 危险点 | 力量成长后多段攻击伤害上升。 |

| 打法 | 不要让它安全启动多轮。能在成长后立刻压死最好。 |

| 推荐准备 | 穿透格挡的高伤害、稳定防御。 |

</details>

#### 幽灵船

- id: `act3_normal_needs_mcp_mapping_040`
- raw_heading: 幽灵船
- act: 3
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

状态牌、普通攻击、多段攻击、虚弱/脆弱/易伤。

**危险点**

多种负面状态叠加后，下一轮很难完整防御。

**打法**

不要贪慢速能力。优先维持手牌质量，必要时耗竭状态。

**推荐准备**

抽牌、耗竭、前置伤害。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 状态牌、普通攻击、多段攻击、虚弱/脆弱/易伤。 |

| 危险点 | 多种负面状态叠加后，下一轮很难完整防御。 |

| 打法 | 不要贪慢速能力。优先维持手牌质量，必要时耗竭状态。 |

| 推荐准备 | 抽牌、耗竭、前置伤害。 |

</details>

#### 高塔炮手

- id: `act3_normal_needs_mcp_mapping_041`
- raw_heading: 高塔炮手
- act: 3
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

多段射击，装填/成长。

**危险点**

多段攻击在成长后很痛。

**打法**

装填回合输出，攻击回合防御。

**推荐准备**

大格挡、爆发。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 多段射击，装填/成长。 |

| 危险点 | 多段攻击在成长后很痛。 |

| 打法 | 装填回合输出，攻击回合防御。 |

| 推荐准备 | 大格挡、爆发。 |

</details>

#### 地精佣兵

- id: `act3_normal_needs_mcp_mapping_042`
- raw_heading: 地精佣兵
- act: 3
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

多段攻击、虚弱、力量成长。

**危险点**

成长后多段攻击伤害上升。

**打法**

被虚弱时输出差，尽量在非虚弱窗口打爆发。

**推荐准备**

稳定防御、爆发。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 多段攻击、虚弱、力量成长。 |

| 危险点 | 成长后多段攻击伤害上升。 |

| 打法 | 被虚弱时输出差，尽量在非虚弱窗口打爆发。 |

| 推荐准备 | 稳定防御、爆发。 |

</details>

#### 组装师

- id: `act3_normal_needs_mcp_mapping_043`
- raw_heading: 组装师
- act: 3
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

组装/召唤，攻击并召唤，分解攻击。

**危险点**

放任召唤会导致目标过多。

**打法**

如果本体血量可控，优先本体；否则用群伤清召唤物。

**推荐准备**

群伤、爆发。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 组装/召唤，攻击并召唤，分解攻击。 |

| 危险点 | 放任召唤会导致目标过多。 |

| 打法 | 如果本体血量可控，优先本体；否则用群伤清召唤物。 |

| 推荐准备 | 群伤、爆发。 |

</details>

#### 双尾鼠

- id: `act3_normal_needs_mcp_mapping_044`
- raw_heading: 双尾鼠
- act: 3
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

抓挠、疾病啃咬、尖叫施加脆弱、呼叫支援。

**危险点**

召唤 + 脆弱会造成后续高压。

**打法**

尽量在呼叫支援前击杀；已经召唤时先清即将攻击的小怪。

**推荐准备**

群伤、低费攻击。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 抓挠、疾病啃咬、尖叫施加脆弱、呼叫支援。 |

| 危险点 | 召唤 + 脆弱会造成后续高压。 |

| 打法 | 尽量在呼叫支援前击杀；已经召唤时先清即将攻击的小怪。 |

| 推荐准备 | 群伤、低费攻击。 |

</details>

#### 活体盾

- id: `act3_normal_needs_mcp_mapping_045`
- raw_heading: 活体盾
- act: 3
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

盾击、重击并成长。

**危险点**

成长后伤害上升。

**打法**

普通成长型敌人，尽快处理。

**推荐准备**

单体伤害。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 盾击、重击并成长。 |

| 危险点 | 成长后伤害上升。 |

| 打法 | 普通成长型敌人，尽快处理。 |

| 推荐准备 | 单体伤害。 |

</details>

#### 海洋混混

- id: `act3_normal_needs_mcp_mapping_046`
- raw_heading: 海洋混混
- act: 3
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

踢击、多段回旋踢、获得格挡和力量。

**危险点**

格挡+成长拖长战斗。

**打法**

在其低防窗口输出，避免被成长拖进高伤害。

**推荐准备**

爆发、易伤。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 踢击、多段回旋踢、获得格挡和力量。 |

| 危险点 | 格挡+成长拖长战斗。 |

| 打法 | 在其低防窗口输出，避免被成长拖进高伤害。 |

| 推荐准备 | 爆发、易伤。 |

</details>

#### 化石追踪者

- id: `act3_normal_needs_mcp_mapping_047`
- raw_heading: 化石追踪者
- act: 3
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

冲锋施加脆弱，缠绕/攻击，多段挥击。

**危险点**

脆弱后接多段攻击。

**打法**

被脆弱后优先防御；能打断/斩杀时不要拖。

**推荐准备**

防御、爆发。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 冲锋施加脆弱，缠绕/攻击，多段挥击。 |

| 危险点 | 脆弱后接多段攻击。 |

| 打法 | 被脆弱后优先防御；能打断/斩杀时不要拖。 |

| 推荐准备 | 防御、爆发。 |

</details>

#### 淤泥旋螺

- id: `act3_normal_needs_mcp_mapping_048`
- raw_heading: 淤泥旋螺
- act: 3
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

喷油虚弱、重击、狂怒成长。

**危险点**

虚弱降低输出，成长后伤害提升。

**打法**

尽量在虚弱前输出；虚弱后不要强行打低效爆发。

**推荐准备**

易伤、稳定伤害。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 喷油虚弱、重击、狂怒成长。 |

| 危险点 | 虚弱降低输出，成长后伤害提升。 |

| 打法 | 尽量在虚弱前输出；虚弱后不要强行打低效爆发。 |

| 推荐准备 | 易伤、稳定伤害。 |

</details>

#### 蟾蜍蝌蚪

- id: `act3_normal_needs_mcp_mapping_049`
- raw_heading: 蟾蜍蝌蚪
- act: 3
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

多段尖刺喷吐、旋转攻击、获得尖刺。

**危险点**

尖刺惩罚多段攻击。

**打法**

尖刺高时用少次数高伤害或技能伤害，不要乱打多段。

**推荐准备**

单段高伤害、防御。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 多段尖刺喷吐、旋转攻击、获得尖刺。 |

| 危险点 | 尖刺惩罚多段攻击。 |

| 打法 | 尖刺高时用少次数高伤害或技能伤害，不要乱打多段。 |

| 推荐准备 | 单段高伤害、防御。 |

</details>

#### 青蛙骑士

- id: `act3_normal_needs_mcp_mapping_050`
- raw_heading: 青蛙骑士
- act: 3
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

为女王获得力量，高伤害打击，舌击施加脆弱，甲虫冲锋大伤害。

**危险点**

35 左右的大攻击很危险。

**打法**

看到冲锋前后要留足防御；若它成长，应尽快击杀。

**推荐准备**

大格挡、爆发。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 为女王获得力量，高伤害打击，舌击施加脆弱，甲虫冲锋大伤害。 |

| 危险点 | 35 左右的大攻击很危险。 |

| 打法 | 看到冲锋前后要留足防御；若它成长，应尽快击杀。 |

| 推荐准备 | 大格挡、爆发。 |

</details>

#### 电球头

- id: `act3_normal_needs_mcp_mapping_051`
- raw_heading: 电球头
- act: 3
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

多段雷击、脆弱攻击、爆发攻击并成长。

**危险点**

成长 + 多段攻击。

**打法**

不要让它安全成长；多段回合优先防御。

**推荐准备**

稳定防御、快速输出。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 多段雷击、脆弱攻击、爆发攻击并成长。 |

| 危险点 | 成长 + 多段攻击。 |

| 打法 | 不要让它安全成长；多段回合优先防御。 |

| 推荐准备 | 稳定防御、快速输出。 |

</details>

#### 拳击构装体

- id: `act3_normal_needs_mcp_mapping_052`
- raw_heading: 拳击构装体
- act: 3
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

准备并格挡，强力拳击，快速拳击并施加虚弱。

**危险点**

虚弱让斩杀窗口变差。

**打法**

在它准备/格挡少的窗口输出；被虚弱时优先防御和过牌。

**推荐准备**

爆发、抽牌。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 准备并格挡，强力拳击，快速拳击并施加虚弱。 |

| 危险点 | 虚弱让斩杀窗口变差。 |

| 打法 | 在它准备/格挡少的窗口输出；被虚弱时优先防御和过牌。 |

| 推荐准备 | 爆发、抽牌。 |

</details>

#### 咬人卷轴

- id: `act3_normal_needs_mcp_mapping_053`
- raw_heading: 咬人卷轴
- act: 3
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

大啃、多段啃咬、长牙成长。

**危险点**

成长型敌人，拖久伤害高。

**打法**

优先击杀，不要让它多次成长。

**推荐准备**

前置伤害。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 大啃、多段啃咬、长牙成长。 |

| 危险点 | 成长型敌人，拖久伤害高。 |

| 打法 | 优先击杀，不要让它多次成长。 |

| 推荐准备 | 前置伤害。 |

</details>

#### 虔诚雕刻师

- id: `act3_normal_needs_mcp_mapping_054`
- raw_heading: 虔诚雕刻师
- act: 3
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

禁忌咒语类强化，凶猛攻击。

**危险点**

强化后伤害提高。

**打法**

按成长型敌人处理，尽快击杀。

**推荐准备**

单体爆发。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 禁忌咒语类强化，凶猛攻击。 |

| 危险点 | 强化后伤害提高。 |

| 打法 | 按成长型敌人处理，尽快击杀。 |

| 推荐准备 | 单体爆发。 |

</details>

#### 史莱姆狂战士

- id: `act3_normal_needs_mcp_mapping_055`
- raw_heading: 史莱姆狂战士
- act: 3
- tier: normal
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

公开资料显示其为第三幕普通怪且血量很高；完整行动细节需以游戏内或更新数据库为准。

**危险点**

高血量意味着需要缩放，不能只靠前置伤害。

**打法**

保留缩放能力，避免把关键爆发浪费在低收益回合。

**推荐准备**

力量、毒/持续伤害、耗竭循环或大输出引擎。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 公开资料显示其为第三幕普通怪且血量很高；完整行动细节需以游戏内或更新数据库为准。 |

| 危险点 | 高血量意味着需要缩放，不能只靠前置伤害。 |

| 打法 | 保留缩放能力，避免把关键爆发浪费在低收益回合。 |

| 推荐准备 | 力量、毒/持续伤害、耗竭循环或大输出引擎。 |



---

</details>

### 精英

#### 骇鳗

- id: `act3_elite_needs_mcp_mapping_056`
- raw_heading: 骇鳗
- act: 3
- tier: elite
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

冲撞、乱击并获得活力、眩晕/恐惧，可能施加极高易伤。

**危险点**

高易伤后任何攻击都可能致命。

**打法**

恐惧/易伤回合优先防御或直接斩杀。不要在易伤 99 之类极端负面下裸吃攻击。

**推荐准备**

大格挡、无实体、爆发药水。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 冲撞、乱击并获得活力、眩晕/恐惧，可能施加极高易伤。 |

| 危险点 | 高易伤后任何攻击都可能致命。 |

| 打法 | 恐惧/易伤回合优先防御或直接斩杀。不要在易伤 99 之类极端负面下裸吃攻击。 |

| 推荐准备 | 大格挡、无实体、爆发药水。 |

</details>

#### 鬼祟珊瑚群

- id: `act3_elite_needs_mcp_mapping_057`
- raw_heading: 鬼祟珊瑚群
- act: 3
- tier: elite
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

惯性获得格挡和力量，冲刺攻击，多段攻击，重击并塞状态。

**危险点**

力量成长 + 状态污染。

**打法**

状态多的牌组很怕它，优先压血。尽量在格挡低时打爆发。

**推荐准备**

耗竭、爆发、持续防御。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 惯性获得格挡和力量，冲刺攻击，多段攻击，重击并塞状态。 |

| 危险点 | 力量成长 + 状态污染。 |

| 打法 | 状态多的牌组很怕它，优先压血。尽量在格挡低时打爆发。 |

| 推荐准备 | 耗竭、爆发、持续防御。 |

</details>

#### 幽灵骑士

- id: `act3_elite_needs_mcp_mapping_058`
- raw_heading: 幽灵骑士
- act: 3
- tier: elite
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

诅咒/Hex，斩击，多段灵魂火。

**危险点**

诅咒类效果会破坏牌组运转。

**打法**

在三骑士遭遇中通常优先处理幽灵骑士，减少长期干扰。

**推荐准备**

爆发、抽牌、耗竭。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 诅咒/Hex，斩击，多段灵魂火。 |

| 危险点 | 诅咒类效果会破坏牌组运转。 |

| 打法 | 在三骑士遭遇中通常优先处理幽灵骑士，减少长期干扰。 |

| 推荐准备 | 爆发、抽牌、耗竭。 |

</details>

#### 魔法骑士

- id: `act3_elite_needs_mcp_mapping_059`
- raw_heading: 魔法骑士
- act: 3
- tier: elite
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

攻击并格挡，削弱/抑制类负面，准备，魔法炸弹大伤害，冲撞。

**危险点**

魔法炸弹伤害极高。

**打法**

大招前必须留防御；若能提前击杀则优先压它。

**推荐准备**

大格挡、控制节奏的爆发。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 攻击并格挡，削弱/抑制类负面，准备，魔法炸弹大伤害，冲撞。 |

| 危险点 | 魔法炸弹伤害极高。 |

| 打法 | 大招前必须留防御；若能提前击杀则优先压它。 |

| 推荐准备 | 大格挡、控制节奏的爆发。 |

</details>

#### 连枷骑士

- id: `act3_elite_needs_mcp_mapping_060`
- raw_heading: 连枷骑士
- act: 3
- tier: elite
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

战吼加力量，多段连枷，冲撞。

**危险点**

力量成长后多段攻击危险。

**打法**

三骑士中可视情况第二或第三处理；如果它已经成长多次，优先级上升。

**推荐准备**

群伤、单体爆发。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 战吼加力量，多段连枷，冲撞。 |

| 危险点 | 力量成长后多段攻击危险。 |

| 打法 | 三骑士中可视情况第二或第三处理；如果它已经成长多次，优先级上升。 |

| 推荐准备 | 群伤、单体爆发。 |

</details>

#### 花园幽灵鳗

- id: `act3_elite_needs_mcp_mapping_061`
- raw_heading: 花园幽灵鳗
- act: 3
- tier: elite
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

低中等攻击、成长。

**危险点**

多次成长后伤害提升。

**打法**

按成长型精英处理。

**推荐准备**

快速击杀。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 低中等攻击、成长。 |

| 危险点 | 多次成长后伤害提升。 |

| 打法 | 按成长型精英处理。 |

| 推荐准备 | 快速击杀。 |

</details>

#### 机甲骑士

- id: `act3_elite_needs_mcp_mapping_062`
- raw_heading: 机甲骑士
- act: 3
- tier: elite
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

固定模式倾向明显：蓄力/充能、喷火、蓄势、重劈。重劈伤害很高，蓄势会获得格挡和力量。

**危险点**

35-40 左右的大攻击，成长后更危险。

**打法**

看到蓄势后准备大防御或斩杀。不要在它高格挡回合浪费爆发。

**推荐准备**

大格挡、爆发、力量缩放。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 固定模式倾向明显：蓄力/充能、喷火、蓄势、重劈。重劈伤害很高，蓄势会获得格挡和力量。 |

| 危险点 | 35-40 左右的大攻击，成长后更危险。 |

| 打法 | 看到蓄势后准备大防御或斩杀。不要在它高格挡回合浪费爆发。 |

| 推荐准备 | 大格挡、爆发、力量缩放。 |

</details>

#### 灵魂枢纽

- id: `act3_elite_needs_mcp_mapping_063`
- raw_heading: 灵魂枢纽
- act: 3
- tier: elite
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

公开资料显示其为第三幕精英，拥有多个意图；完整行动细节需要进一步核对。

**危险点**

第三幕精英通常会同时考验输出、防御和牌组稳定性。

**打法**

按高威胁精英处理：优先识别成长/召唤/状态来源，必要时使用药水。

**推荐准备**

成型牌组、药水、稳定循环。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 公开资料显示其为第三幕精英，拥有多个意图；完整行动细节需要进一步核对。 |

| 危险点 | 第三幕精英通常会同时考验输出、防御和牌组稳定性。 |

| 打法 | 按高威胁精英处理：优先识别成长/召唤/状态来源，必要时使用药水。 |

| 推荐准备 | 成型牌组、药水、稳定循环。 |



---

</details>

### Boss

#### 门扉 + 门扉缔造者 / Door + Doormaker

- id: `act3_boss_door_doormaker`
- raw_heading: 门扉 + 门扉缔造者 / Door + Doormaker
- act: 3
- tier: boss
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: mapped_from_source_heading
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

先打门扉，门扉死亡后门扉缔造者出现并眩晕。门扉缔造者有高伤害光束、回门/逃离式机制、力量成长和阶段限制。部分资料显示它会围绕“消耗已打出卡牌、限制抽牌、增加能量惩罚”等机制轮转。

**危险点**

硬反制某些慢速或依赖特定类型牌的构筑；阶段机制会让牌组突然失效。

**打法**

第一阶段尽量低损耗破门；缔造者眩晕回合是输出窗口。不要只依赖单一牌型。准备好跨阶段输出、防御和抽牌。

**推荐准备**

多路线输出、抽牌、能量、防御药水、终局缩放。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 先打门扉，门扉死亡后门扉缔造者出现并眩晕。门扉缔造者有高伤害光束、回门/逃离式机制、力量成长和阶段限制。部分资料显示它会围绕“消耗已打出卡牌、限制抽牌、增加能量惩罚”等机制轮转。 |

| 危险点 | 硬反制某些慢速或依赖特定类型牌的构筑；阶段机制会让牌组突然失效。 |

| 打法 | 第一阶段尽量低损耗破门；缔造者眩晕回合是输出窗口。不要只依赖单一牌型。准备好跨阶段输出、防御和抽牌。 |

| 推荐准备 | 多路线输出、抽牌、能量、防御药水、终局缩放。 |

</details>

#### 灵魂异鱼

- id: `act3_boss_needs_mcp_mapping_064`
- raw_heading: 灵魂异鱼
- act: 3
- tier: boss
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

召唤/呼唤状态、排气攻击、凝视塞状态、淡出获得无实体、尖叫施加易伤。

**危险点**

无实体回合会降低输出效率；状态和易伤制造爆发风险。

**打法**

无实体回合少浪费爆发，转为防御/过牌/准备；非无实体窗口集中输出。

**推荐准备**

状态处理、爆发窗口判断。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 召唤/呼唤状态、排气攻击、凝视塞状态、淡出获得无实体、尖叫施加易伤。 |

| 危险点 | 无实体回合会降低输出效率；状态和易伤制造爆发风险。 |

| 打法 | 无实体回合少浪费爆发，转为防御/过牌/准备；非无实体窗口集中输出。 |

| 推荐准备 | 状态处理、爆发窗口判断。 |

</details>

#### 火炬头聚合体

- id: `act3_boss_needs_mcp_mapping_065`
- raw_heading: 火炬头聚合体
- act: 3
- tier: boss
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

多段和单段攻击混合，多个火炬头可能形成连续攻击压力。

**危险点**

攻击频率高，防御断档会掉大量血。

**打法**

稳定防御比单回合爆发更重要。能削弱攻击来源时优先处理。

**推荐准备**

持续格挡、AOE、力量缩放。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 多段和单段攻击混合，多个火炬头可能形成连续攻击压力。 |

| 危险点 | 攻击频率高，防御断档会掉大量血。 |

| 打法 | 稳定防御比单回合爆发更重要。能削弱攻击来源时优先处理。 |

| 推荐准备 | 持续格挡、AOE、力量缩放。 |

</details>

#### 实验体系列

- id: `act3_boss_needs_mcp_mapping_066`
- raw_heading: 实验体系列
- act: 3
- tier: boss
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

多阶段/复活式敌人，带痛刺、复仇类能力，攻击模式随阶段变化；后期有高伤害扑击和多段撕裂。

**危险点**

以为击杀后结束，实际进入下一阶段；后期 30-45 伤害回合非常危险。

**打法**

保留资源给后续阶段。不要把所有药水/爆发花在第一条命。

**推荐准备**

持续缩放、大防御、稳定抽牌。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 多阶段/复活式敌人，带痛刺、复仇类能力，攻击模式随阶段变化；后期有高伤害扑击和多段撕裂。 |

| 危险点 | 以为击杀后结束，实际进入下一阶段；后期 30-45 伤害回合非常危险。 |

| 打法 | 保留资源给后续阶段。不要把所有药水/爆发花在第一条命。 |

| 推荐准备 | 持续缩放、大防御、稳定抽牌。 |

</details>

#### 乐加维林族母

- id: `act3_boss_needs_mcp_mapping_067`
- raw_heading: 乐加维林族母
- act: 3
- tier: boss
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

初始睡眠，醒后斩击、攻击并格挡、多段撕裂、灵魂吸取加力量。

**危险点**

和旧日雕像类似，唤醒时机决定战斗难度；成长后压力升高。

**打法**

先准备能力/缩放，再唤醒；醒后集中输出。

**推荐准备**

可控唤醒、爆发、防御。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 初始睡眠，醒后斩击、攻击并格挡、多段撕裂、灵魂吸取加力量。 |

| 危险点 | 和旧日雕像类似，唤醒时机决定战斗难度；成长后压力升高。 |

| 打法 | 先准备能力/缩放，再唤醒；醒后集中输出。 |

| 推荐准备 | 可控唤醒、爆发、防御。 |

</details>

#### 瀑布巨兽

- id: `act3_boss_needs_mcp_mapping_068`
- raw_heading: 瀑布巨兽
- act: 3
- tier: boss
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

蒸汽喷发/叠层、攻击、踩踏、虹吸、压力炮、爆炸类行动。

**危险点**

公开资料显示其有爆炸/压力类大回合，容易突然造成高伤害。

**打法**

注意倒计时式行动。低压回合输出，高压回合防御；若有爆炸前窗口，应尽快斩杀。

**推荐准备**

大格挡、爆发、生命管理。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 蒸汽喷发/叠层、攻击、踩踏、虹吸、压力炮、爆炸类行动。 |

| 危险点 | 公开资料显示其有爆炸/压力类大回合，容易突然造成高伤害。 |

| 打法 | 注意倒计时式行动。低压回合输出，高压回合防御；若有爆炸前窗口，应尽快斩杀。 |

| 推荐准备 | 大格挡、爆发、生命管理。 |

</details>

#### 女王

- id: `act3_boss_needs_mcp_mapping_069`
- raw_heading: 女王
- act: 3
- tier: boss
- hp_range: 未提供
- intent_pattern: 未提供
- threat_level: 未提供
- mapping_status: needs_mcp_english_mapping
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔 2：全怪物机制与打法攻略（中文整理版）.md

**机制**

束缚/操线、极端负面状态、格挡、斩首式多段攻击、处决、成长。

**危险点**

极端虚弱/脆弱/易伤状态会让某些回合几乎不能硬吃。

**打法**

不要在极端负面状态下贪输出。她格挡/准备时可以调整手牌，处决或多段回合要全力防御或斩杀。

**推荐准备**

负面状态应对、强防御、终局输出。

<details><summary>原始记录</summary>

| 项目 | 内容 |

|---|---|

| 机制 | 束缚/操线、极端负面状态、格挡、斩首式多段攻击、处决、成长。 |

| 危险点 | 极端虚弱/脆弱/易伤状态会让某些回合几乎不能硬吃。 |

| 打法 | 不要在极端负面状态下贪输出。她格挡/准备时可以调整手牌，处决或多段回合要全力防御或斩杀。 |

| 推荐准备 | 负面状态应对、强防御、终局输出。 |



---

</details>

## 附录

### 快速阅读指南

### 怪物打法的通用判断顺序



1. **这场是否有成长机制？**

   如果怪物每轮涨力量、护甲、尖刺、召唤或叠状态，优先结束战斗，不要拖。



2. **这场是否惩罚多段攻击？**

   有尖刺、反击或“每次受击触发”的敌人时，多段攻击要谨慎。反之，遇到“单次伤害上限”的敌人，多段攻击反而很强。



3. **这场是否需要清小怪？**

   召唤物持续给压力时，先清召唤物；如果本体会无限召唤且本体血量不高，则优先打本体。



4. **这场是否有大回合？**

   例如 25、30、35、40+ 的大攻击。提前留防御牌、药水、虚无/无实体/护甲累积。



5. **这场是否塞状态或上负面？**

   状态牌会破坏循环牌组。需要抽牌、耗竭、牌组压缩或快速斩杀。



---

### 按机制分类的应对清单

### A. 成长型敌人



代表：小啃兽、毛绒伏地虫、虱虫之祖、直飞产卵虫、巨斧机器人、咬人卷轴、机甲骑士、感染棱柱、女王。



**打法要点：**

- 不要无意义拖回合。

- 看到加力量/强化后，应把下一轮视为危险回合。

- 有易伤或药水时，优先创造斩杀窗口。



### B. 召唤型敌人



代表：雾菇、结实的卵、直飞产卵虫、组装师、双尾鼠、活雾、同族神官遭遇。



**打法要点：**

- 本体血量低：优先本体。

- 召唤物即将造成大量伤害：先清小怪。

- 群伤在这些战斗中价值很高。



### C. 状态/负面型敌人



代表：异蛙寄生虫、幽灵船、知识恶魔、灵魂异鱼、藤蔓蹒跚者、飞蝇菌子、化石追踪者。



**打法要点：**

- 抽牌和耗竭价值上升。

- 不要让状态牌进入第二轮循环。

- 被易伤/脆弱时，优先保血。



### D. 多段攻击型敌人



代表：异蛙寄生虫、同族神官、幽灵船、高塔炮手、电球头、实验体后期、三骑士部分单位。



**打法要点：**

- 力量成长会显著放大多段伤害。

- 防御需要足够厚，不能只靠少量减伤。

- 虚弱敌人或削力量非常有效。



### E. 尖刺/受击惩罚型敌人



代表：棘刺蟾蜍、蟾蜍蝌蚪、可能具有受击惩罚的特殊敌人。



**打法要点：**

- 少用无意义多段攻击。

- 优先单段高伤害、非攻击伤害或格挡后输出。

- 如果必须多段，先确认生命能承受反伤。



### F. 单次伤害上限/滑溜型敌人



代表：墨影幻灵、外骨骼虫。



**打法要点：**

- 多段攻击优先。

- 不要把高倍率爆发浪费在减伤层数仍高时。

- 低费攻击、随机多段、追加伤害更好。



---

### 药水和路线建议

### 第一幕



- 进精英前至少需要两类能力之一：

  1. 稳定前置伤害；

  2. 一瓶能解决大回合的药水。

- 如果牌组只有防御没有输出，旧日雕像、扭动虫、异蛙寄生虫都会很难打。

- 如果牌组只有输出没有防御，Boss 高伤害回合会很危险。



### 第二幕



- 需要群伤和状态处理。

- 召唤、负面、成长明显变多。

- 如果牌组还没有抽牌/能量/耗竭，第二幕 Boss 会暴露循环问题。



### 第三幕



- 需要终局方案。

- 单靠普通攻击和普通格挡通常不够。

- 要有至少一种缩放：力量、格挡转输出、耗竭压缩、无限/近无限循环、毒/持续伤害、强能力牌。

- 药水应优先留给精英和 Boss 的大回合，而不是普通怪的小失误。



---

### 资料来源与更新建议

主要资料来源：



- STS2.GG 简中怪物数据库：

  https://sts2.gg/zh/monsters



- Spire Codex / Community Reference Sheet：

  https://spire-codex.com/monsters

  https://github.com/ptrlrd/spire-codex



- STS2 Info Hub Encounters：

  https://slaythespire2.game-vault.net/wiki/Encounter



- Mobalytics Encounters：

  https://mobalytics.gg/slay-the-spire-2/encounters/monsters



- PC Gamer / GamesRadar 相关 Boss 攻略与机制说明：

  https://www.pcgamer.com/

  https://www.gamesradar.com/



更新建议：



1. 每次大版本更新后，优先核对 Boss 与精英数值。

2. 再核对有明显机制的怪：门扉缔造者、墨影幻灵、女王、实验体、残杀千足虫、蜂群术士。

3. 普通怪数值变动通常不影响大方向打法，但会影响是否能斩杀。

4. 若要做成表格版，可把本攻略拆成：怪物名、幕数、类型、机制、危险回合、打法、推荐牌组能力、资料状态。

