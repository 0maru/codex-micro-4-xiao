# ファームウェア

このディレクトリには、Seeed Studio XIAO nRF52840向けに独自実装するZephyrアプリケーションを格納します。

最初の目標は、観察されたベンダー定義BLE HIDレポートを実装し、完成版PCBがなくても操作できるプロトコルPoCを作ることです。ChatGPT Desktopからの認識を確認した後、キーマトリクス、ロータリーエンコーダー、ジョイスティック、LED制御を追加します。

## 予定している構成

```text
firmware/
  CMakeLists.txt
  prj.conf
  app.overlay
  src/
    main.c
    protocol.c
    protocol.h
    transport.c
    transport.h
    controls.c
    controls.h
```

使用するZephyrまたはnRF Connect SDKのバージョンは、まだ固定していません。XIAO実機で最初のビルドに成功した時点でバージョンを固定し、同じツールチェーンを使うCIを追加します。

確認されている互換プロファイルとPoCの完了条件は、[`../docs/protocol.md`](../docs/protocol.md)を参照してください。
