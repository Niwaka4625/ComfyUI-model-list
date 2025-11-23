# 開発環境記録

## デバイス情報
- デバイス名: DESKTOP-08GVSKP
- プロセッサ: Intel Core i5-4690K CPU @ 3.50GHz (4コア)
- 実装RAM: 16.0 GB
- ストレージ:
  - 466 GB SSD (CT500MX500SSD1)
  - 932 GB HDD (WDC WD1001FALS-55J7B0)
  - 238 GB SSD (TOSHIBA THNSNJ256GCSU)
- グラフィックスカード: NVIDIA GeForce GTX 1660 Ti (6 GB)
- システム種類: 64ビット OS, x64 ベースプロセッサ
- OS: Windows 10 Home 64bit (バージョン2009, ビルド19041)

## この構成でできること
- ComfyUIによる画像生成
- DPM++ 2M Karrasサンプラー適用予定 (2025-11-11)
- VAE, 解像度, Seed固定済み
- アップスケール設定利用可能 (ESRGAN系)

## 設定詳細
- サンプラー: DPM++ 2M Karras
- VAE: 適用済み
- 解像度: 512x512 / 768x768
- Seed: 固定
- アップスケール: R-ESRGAN 4x

## 環境での制約
- デノイズ値は最低0.5以上必要（0.5未満では画像生成不可）
- SDXLはGTX1660Ti(6GB)環境では負荷が大きすぎて実用不可
- アップスケーラーは Ultimate SD Upscale ノードを使用（内部設定で R-ESRGAN 4x を選択）

## 備考
- マザーボードは MSI Z97I GAMING ACK（2014年頃の世代）を使用。古い構成ながら現役で稼働中。