# Cosmos DB (NoSQL) 概要 & Hands On

## 目次

1. Cosmos DB NoSQL 概要 (1hour)
1. Cosmos DB NoSQLの操作 (30min)
1. Cosmos DB Change Feed 概要 (30min)
1. Cosmos DB Change Feedの操作 (1hour)
    1. Azure Functionsの作成
    1. Azure Functionsのコーディング
    1. テスト

## Cosmos DB for NoSQL 概要

### Cosmos DB for NoSQLとは？

- Microsoft Azureで利用できる**NoSQLデータストア**
    - いわゆる**データベース**とは異なる概念と理解するのが良い
    - ファイルの簡便性とRDBMSの高速検索の良いとこどり
    - RDBMSを補完する概念であり、排他的ではない 

- 優位性
  - **表形式でない**ドキュメント(JSON)の取り扱い
  ```JSON
  {
      "name"   : "John Smith",
      "sku"    : "20223",
      "price"  : 23.95,
      "shipTo" : { "name" : "Jane Smith",
                   "address" : "123 Maple Street",
                   "city" : "Pretendville",
                   "state" : "NY",
                   "zip"   : "12345" },
      "billTo" : { "name" : "John Smith",
                   "address" : "123 Maple Street",
                   "city" : "Pretendville",
                   "state" : "NY",
                   "zip"   : "12345" }
  }
  ```
  - 大量データの中から特定の少量データを高速に検索できる
  - データ登録を起点とするイベント起動ができイベント駆動型データ処理を実現
  - 同時アクセスなどによる高負荷時でも処理を分散してレイテンシーを落とさない
  - 地理分散を利用してアクセス元の地域に寄らない高速なアクセス速度を保証
  - マルチマスターによる高速な書き込み
  - 負荷に合わせて性能を自由に変更できる。自動調整も可能。(注:制約はある)
  - SLAで99.999%の可用性を保証
  - 1KB Readに関しては99%の処理を10ms以内で応答

- トレードオフ
  - トランザクション
      - 利用できる状況が限定的
  - 結合・集計処理
      - できないことはないが高コスト
      - 分析用途としてSynapse Linkという連携機能を装備しており、そちらで対応
   
- 優位性とトレードオフを加味すると以下のようなシステムに向いている
    - 利用者や同時アクセス数は多い方が良い
    - 蓄積されるデータは多い方が良い
    - クエリはシンプルなほうが良い
    - 1回に抽出されるデータは少ない方が良い
    - データが"HOT"な期間、蓄積して利用するのが良い
        - 長期的なデータ保存は別の仕組みに任せるほうがよい

- 上記を考慮したユースケース
    - Webアプリ・モバイルアプリ
    - IoTデバイスデータ処理
    - プロダクトカタログ
    - ログ管理
    - エンタープライズ・データ中継 

- 開発者向けのメリット
    - システム変更の効率化に寄与
        - データベース項目の変更があってもスキーマレスのため過去データを直さなくてよい
        - アクセス負荷の急激な変更に強い
