# ハードウェア

このディレクトリには、KiCadの回路図、PCB、プロジェクト固有のシンボル、フットプリント、確認済み3Dモデルを格納します。

商品写真だけを基に製造用フットプリントを作成しません。ジョイスティック、ロータリーエンコーダー、ソケット、コネクター、スイッチの型番を確定してから、メーカーの寸法図と照合してパッドおよび外形寸法を確認します。

予定している構成：

```text
hardware/
  codex-micro-4-xiao.kicad_pro
  codex-micro-4-xiao.kicad_sch
  codex-micro-4-xiao.kicad_pcb
  libraries/
    symbols/
    footprints/
    models/
```

現在の電気的・機械的な制約は、[`../docs/hardware.md`](../docs/hardware.md)を参照してください。
