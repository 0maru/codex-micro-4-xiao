# codex-micro-4-xiao

Seeed Studio XIAO nRF52840を使った、ChatGPT Desktop向けの非公式BLEコントローラーです。

13個のロープロファイルキー、ジョイスティック、ロータリーエンコーダーを備えた小型デバイスを自作します。ファームウェアと基板は独自に設計し、公開されている互換実装からは通信時に観察された情報だけを参照します。

> [!IMPORTANT]
> このプロジェクトはOpenAIおよびWork Louderによる公式製品ではなく、両社から承認・支援を受けていません。非公開の互換プロトコルに依存するため、ChatGPT Desktopの更新によって動作しなくなる可能性があります。

## 現在の仕様

| 項目 | 内容 | 状態 |
| --- | --- | --- |
| MCU | Seeed Studio XIAO nRF52840（Plusではないモデル） | 確定・所有済み |
| MCU実装 | PCBへ直接はんだ付け・背面GPIO使用 | 方針確定・接続設計未検証 |
| 接続 | Bluetooth Low Energy HID over GATT | 確定 |
| キー | 13キー | 確定 |
| スイッチ実装 | Kailh CPG135001S30 ×13 | 採用確定・適合検証待ち |
| スイッチ | JezailFunder 霧、Kailh Choc V2互換 | 購入済み |
| キーキャップ | LCK AIキーキャップセット | 購入済み |
| 左上 | Bourns PEC11R-4215F-S0024（押込み付き） | 採用方針確定・実装依頼条件確認待ち |
| 右上 | Alps Alpine SKRHABE010（4方向＋中央押込み） | 確定・要調達 |
| キー照明 | OPSCO SK6803MINI-E-001 ×13 | 採用確定・電源／配置検証待ち |
| 実装依頼 | LED・エンコーダー等の難しい部品はJLCPCBへ依頼 | 方針確定・部品別可否確認待ち |
| ファームウェア | Zephyrベースの専用実装 | 方針確定 |
| PCB | KiCad | 未着手 |

## レイアウト

通常キーは13個すべてを独立入力として扱います。右利きで操作しやすいように、左上へロータリーエンコーダー、右上へジョイスティックを置きます。

| 段 | 左端 | 左中央 | 右中央 | 右端 |
| --- | --- | --- | --- | --- |
| 1 | ロータリーエンコーダー | K01 | K02 | ジョイスティック |
| 2 | K03 | K04 | K05 | K06 |
| 3 | K07 | K08 | K09 | K10 |
| 4 | 空き | K11 | K12 | K13 |

KLEの生データは[`layout/keyboard-layout.json`](layout/keyboard-layout.json)に保存しています。KLEには通常キーだけを記録し、ジョイスティックとエンコーダーはKiCadで配置します。

## 進め方

1. XIAO単体でBLE Vendor HIDを実装する
2. ChatGPT Desktopによる検出と双方向通信を確認する
3. 13個目のCommand Key候補を検証する
4. 選定済み部品の適合・実装条件を確認し、電源部品とバッテリーを選定する
5. GPIO割り当てと回路図を確定する
6. PCB、プレート、ケースを設計する

仕様は[`docs/`](docs/)にまとめています。未確認事項は推測と区別し、実機で確認できた内容だけを「検証済み」へ移します。

PCB発注までの具体的な作業と完了条件は、[`docs/pcb-order-checklist.md`](docs/pcb-order-checklist.md)で管理します。

## 参考資料

- [OpenAI: Codex Micro](https://learn.chatgpt.com/docs/features/codex-micro)
- [Seeed Studio: XIAO nRF52840](https://wiki.seeedstudio.com/XIAO_BLE/)
- [imliubo/codex-micro-4-core2](https://github.com/imliubo/codex-micro-4-core2) — 通信観察結果の参考

## ライセンス

このリポジトリで独自に作成したソースコードとドキュメントは[MIT License](LICENSE)で公開します。第三者の名称、商標、機器識別子およびプロトコルに対する権利は含まれません。詳細は[`NOTICE.md`](NOTICE.md)を参照してください。
