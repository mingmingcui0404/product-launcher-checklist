# 产品上线流程执行清单（简化版）

适用范围：Android 新产品从后台创建、归因配置、广告变现最小单元、首版过审到启动投放。  
目标：先保证产品可提交首版审核；过审后再补齐完整广告与合规配置，最后启动买量。

## 0. 参考配置文档

执行过程中可直接参考下午整理的配置文档：

| 平台 / 场景 | 参考文档 | 用途 |
| --- | --- | --- |
| AppsFlyer - Fruit Mania | [appsflyer_fruit_mania_config.md](appsflyer_fruit_mania_config.md) | 参考 AppsFlyer 产品参数、事件、渠道配置 |
| AppsFlyer - Cluck Carnival | [appsflyer_cluck_carnival_config.md](appsflyer_cluck_carnival_config.md) | 参考不同产品的 AppsFlyer 配置差异 |
| AppsFlyer - Android 创建模板 | [appsflyer_android_app_setup_template.md](appsflyer_android_app_setup_template.md) | 创建新 Android App 时直接照表执行 |
| Adjust - Fruit Mania | [adjust_fruit_mania_config.md](adjust_fruit_mania_config.md) | 参考 Adjust 产品参数、events、partner-level data sharing、w2a 链接 |
| MAX - FruitGarden | [max_fruitgarden_mediation_config.md](max_fruitgarden_mediation_config.md) | 参考 MAX 聚合广告位、waterfall、network、默认开关 |

## 1. 准备产品信息

上线前先确认以下基础信息：

| 信息项 | 说明 |
| --- | --- |
| 产品名称 | 用于 AdMob、Adjust / AppsFlyer、MAX、LevelPlay 等后台 |
| Android 包名 | Google Play package name |
| Google Play 开发者账号 | 确认提交主体 |
| 提现类型 | 真提现 / 假提现 |
| 应用图标、商店素材 | 用于各后台识别与商店提交 |
| 隐私政策 URL | 用于商店、SDK 合规、GDPR |
| app-ads.txt 域名 | 用于 AdMob 审核和广告变现 |

## 2. 创建 AdMob 产品

1. 在 AdMob 创建新 App。
2. 平台选择 Android。
3. 填写产品名称和包名。
4. 记录 AdMob App ID。
5. 暂不强依赖完整广告位配置，先保证产品实体存在。

完成标准：

- AdMob 中已存在该产品；
- App ID 已同步给客户端或配置表；
- app-ads.txt 域名已准备好。

## 3. 配置归因平台

根据提现类型选择归因平台：

| 提现类型 | 归因平台 | 执行动作 |
| --- | --- | --- |
| 真提现 | Adjust | 创建 Android App，配置事件、渠道、链接、必要 partner data sharing |
| 假提现 | AppsFlyer | 创建 Android App，配置事件、渠道、OneLink / 链接模板 |

### 真提现：Adjust

参考：[adjust_fruit_mania_config.md](adjust_fruit_mania_config.md)

1. 创建 Adjust Android App。
2. 配置包名、币种、基础归因窗口。
3. 配置核心 events。
4. 配置渠道链接和必要 partner-level data sharing。
5. 记录 App Token、Event Token、关键 link token。

### 假提现：AppsFlyer

参考：

- [appsflyer_android_app_setup_template.md](appsflyer_android_app_setup_template.md)
- [appsflyer_fruit_mania_config.md](appsflyer_fruit_mania_config.md)
- [appsflyer_cluck_carnival_config.md](appsflyer_cluck_carnival_config.md)

1. 创建 AppsFlyer Android App。
2. 配置包名、时区、币种等基础参数。
3. 配置 in-app events。
4. 配置渠道模板、OneLink 或自定义链接。
5. 配置渠道 postback：
   - `ads_revenue`：All media sources, including organic；Values & revenue。
   - `af_app_opened`：All media sources, including organic；Values & no revenue。
6. 配置 Mintegral / MTG Cost：
   - Partner Integrations > Mintegral > Cost 创建 cost integration。
   - 选择当前 App，Test integration 通过后保存。
   - 新建后可能先显示 `Waiting for sync` / `Never`，后续到 Cost & Ad Revenue Status 复查。
7. 记录 Dev Key、App ID、事件名、关键链接参数。

完成标准：

- 客户端可拿到归因初始化参数；
- 核心事件命名和 token / event name 已确认；
- 渠道链接或模板已能用于测试。

## 4. 创建广告变现最小单元

目标：首版提审前只创建能支撑 SDK 初始化和基础广告验证的最小配置，不一次性铺满全部广告策略。

### MAX

参考：[max_fruitgarden_mediation_config.md](max_fruitgarden_mediation_config.md)

1. 创建 MAX App。
2. 创建最小广告位：
   - Interstitial；
   - Rewarded。
3. 真提现产品如需 Prebid，创建对应 Prebid 组或 Prebid 广告位。
4. 假提现产品 MAX 不创建 Prebid 组。
5. 记录 MAX SDK Key、Ad Unit ID。

### LevelPlay

1. 创建 LevelPlay App。
2. 创建最小广告位：
   - Interstitial；
   - Rewarded。
3. 接入最基础 waterfall / bidding 配置。
4. 记录 App Key、Instance / Placement 信息。

完成标准：

- MAX 和 LevelPlay 至少有 inter / reward 最小广告单元；
- 假提现产品 MAX 未配置 Prebid 组；
- 客户端可完成广告 SDK 初始化和测试广告请求。

## 5. 提交产品第一版审核

提交首版前确认：

| 检查项 | 状态 |
| --- | --- |
| 包名与各后台一致 | 待确认 |
| 归因 SDK 初始化参数已配置 | 待确认 |
| 广告 SDK 最小单元已配置 | 待确认 |
| 隐私政策 URL 可访问 | 待确认 |
| 商店素材完整 | 待确认 |
| 测试设备 / 测试模式已关闭或符合审核要求 | 待确认 |

完成标准：

- Google Play 第一版已提交审核；
- 审核期间不大改后台关键配置，避免包内参数与后台不一致。

## 6. 第一版过审后补齐配置

产品第一版过审后，再补齐完整广告与合规配置。

### 补齐广告配置

1. 在 MAX 配置剩余广告位、waterfall、bidding network、国家/分组策略；参考 [max_fruitgarden_mediation_config.md](max_fruitgarden_mediation_config.md)。
2. 在 LevelPlay 配置剩余广告位、instances、waterfall、bidding network。
3. 配置 AdMob 正式广告单元。
4. 核对各广告平台的 app / package / ad unit 是否对应同一产品。
5. 使用测试设备验证广告加载、展示、激励回调。

### 配置 GDPR

1. 配置 CMP / GDPR 弹窗策略。
2. 确认 EEA / UK 用户 consent 流程。
3. 确认广告 SDK consent 透传。
4. 确认归因 SDK 隐私相关开关；Adjust 参考 [adjust_fruit_mania_config.md](adjust_fruit_mania_config.md)，AppsFlyer 参考 [appsflyer_android_app_setup_template.md](appsflyer_android_app_setup_template.md)。

完成标准：

- 广告配置已从最小单元扩展为正式投放配置；
- GDPR 配置已完成并通过测试；
- 激励广告回调、收入上报、归因事件链路正常。

## 7. 触发 AdMob app-ads.txt 审核

1. 确认 app-ads.txt 已部署到指定域名。
2. 确认文件中包含 AdMob 要求的 publisher 记录。
3. 在 AdMob 中手动触发 app-ads.txt 审核。
4. 等待 AdMob 审核通过。

完成标准：

- AdMob app-ads.txt 状态通过；
- 广告请求不再因 app-ads.txt 问题受限。

## 8. 启动投放

启动投放前最后确认：

| 模块 | 必须满足 |
| --- | --- |
| 商店状态 | 第一版已过审并上线 |
| 归因 | Adjust / AppsFlyer 链路测试通过 |
| 广告 | MAX、LevelPlay、AdMob 正式配置可用 |
| GDPR | 弹窗和 consent 透传已验证 |
| app-ads.txt | AdMob 审核通过 |
| 事件 | 安装、打开、注册、关键转化事件可见 |
| 数据 | 广告收入、归因、事件数据能在后台看到 |
| AppsFlyer 渠道 postback | `ads_revenue` 为 Values & revenue；`af_app_opened` 为 Values & no revenue |
| AppsFlyer Cost | Mintegral / MTG cost integration 已创建并测试通过；同步完成后无 warning / action required |

全部通过后，启动买量投放。

## 9. 总流程速览

```text
准备产品信息
  -> 创建 AdMob 产品
  -> 按提现类型配置归因
     -> 真提现：Adjust
     -> 假提现：AppsFlyer
  -> 创建 MAX / LevelPlay 广告变现最小单元
     -> 假提现 MAX 不配 Prebid 组
  -> 提交第一版审核
  -> 第一版过审
  -> 补齐剩余广告配置
  -> 配置 GDPR
  -> 手动触发 AdMob app-ads.txt 审核
  -> app-ads.txt 审核通过
  -> 启动投放
```

## 10. 简化执行口径

- 首版审核前：只做“产品实体 + 归因基础 + 广告最小单元”。
- 首版过审后：再补齐“完整广告策略 + GDPR + app-ads.txt 审核”。
- 启动投放前：必须确认商店、归因、广告、GDPR、app-ads.txt、事件数据全部闭环。
