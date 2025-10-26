無人セルフ決済キオスクシステム (Prototype)
=============================



![Node.js](https://www.notion.so/image/attachment%3A1cab7827-3a8a-403f-b4e3-04256ef50988%3A73139655-C2DB-496C-AAE4-17733BDCACE2_4_5005_c.jpeg?table=block&id=2093114b-8b80-80ef-b42e-fa4eef3573e2&spaceId=882ec991-f563-4ea2-8f84-2f93c01b7280&width=2000&userId=&cache=v2) 

<img src="https://www.notion.so/image/attachment%3A9519bf0d-7794-4e13-bc4f-ff04054a9ea0%3A%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-06-12_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.09.35.png?table=block&id=2103114b-8b80-801e-84b9-e58ecc3fd61d&spaceId=882ec991-f563-4ea2-8f84-2f93c01b7280&width=2000&userId=&cache=v2" title="" alt="WebSocket" style="zoom:33%;"> 





## プロジェクト概要

* * *

コンビニエンスストア/スーパーマーケットの無人決済システムをウェブベースで実装したプロトタイププロジェクトです。RFIDタグベースの入店、AI商品認識、自動返金計算などの機能をシミュレートし、実際のハードウェア連携前にソフトウェアアーキテクチャの検証を目的としています。

### プロジェクト背景

* **事前決済後返金方式**: 入店時に10万ウォン事前決済 → 購入商品自動認識 → 残額返金
* **無人システム**: 店員の介入なしに顧客が自ら決済完了
* **拡張可能な設計**: 今後のAIモデル、DB、実際の決済システム連携に対応

### プロジェクトステータス

* **開発期間**: 2025.05 ~ 2025.06 (約1ヶ月)
* **現在の状態**: プロトタイプ完成 (Mockデータベース)
* **参加形態**: チームプロジェクトにてバックエンドおよびシステム設計担当
* **離脱理由**: 日本就職プログラム参加のため途中離脱 (チームメンバーがその後AI連携など継続開発)

* * *

主な実装機能
------

### セッション管理システム

* UUIDベースの固有セッションID生成
* 10分自動有効期限タイマー (フロント/バックエンド同期)
* 事前決済情報をJSONファイルで保存・管理

### WebSocketリアルタイム通信

* 商品認識結果の即時配信
* クライアント-サーバー双方向メッセージ処理
* HTTPリクエストなしで画面遷移トリガー

### 返金計算ロジック

* 事前決済金額(₩100,000) - 総購入金額 = 返金額
* 返金履歴をJSONファイルに記録
* セッションベースの重複返金防止

### 5段階UIフロー

1. **初期画面**: スタートボタン
2. **カート案内**: 商品を置くよう誘導
3. **スキャン中**: ローディングアニメーション + シミュレーションボタン
4. **レシート出力**: 返金完了フィードバック
5. **終了挨拶**: 自動初期化および復帰

### Mockデータシミュレーション

* 10商品のサンプルデータ (バーコード、価格など)
* ランダムに3商品を選択してスキャン結果を生成
* AIモデルなしでも全プロセステスト可能

* * *

技術スタック
--------

### Backend

    Node.js v14+
    Express.js v4.18
    WebSocket (ws v8.13)
    UUID v9.0

### Frontend

    Vanilla JavaScript (ES6+)
    HTML5 / CSS3
    SessionStorage API

### Architecture

    MVC Pattern
    RESTful API
    WebSocket Protocol
    File-based Storage (JSON)

* * *

プロジェクト構造
----------

 kiosk/
├── app.js                    # メインサーバーエントリーポイント
├── controllers/
│   └── kioskController.js    # ビジネスロジック (セッション、返金、シミュレーション)
├── routes/
│   └── kioskRoutes.js        # APIルーティング
├── websocket/
│   └── kioskSocket.js        # WebSocketサーバー設定
├── models/
│   └── productModel.js       # (予備) 商品データモデル
├── public/                   # フロントエンド静的ファイル
│   ├── index.html            # キオスクメインUI
│   ├── app.js                # クライアントロジック
│   ├── style.css             # スタイルシート
│   └── assets/               # 画像リソース
│       ├── basket.png
│       ├── receipt.png
│       └── greeting.png
├── test/                     # テストおよびMockデータ
│   ├── session-start.html    # 入店シミュレーションページ
│   ├── mock-products.json    # サンプル商品データ
│   ├── paidSessions.json     # 決済セッションログ
│   └── refunds.json          # 返金履歴ログ
├── package.json
└── README.md

* * *

インストールと実行
---------

### 1. リポジトリクローンと依存関係インストール

    git clone [repository-url]
    cd kiosk
    npm install

### 2.サーバー起動

    node app.js

ターミナルに以下のメッセージが表示されれば正常起動です:

Kiosk server running at [http://localhost:3000](http://localhost:3000)

### 3. Webブラウザでアクセス

**① 入店シミュレーション (セッション開始)** http://localhost:3000/test/session-start.html

→ 「入店」ボタンクリック時、セッション生成後キオスク画面へ自動移動

**② キオスクメイン画面 (直接アクセス時)** http://localhost:3000/index.html

セッションIDがない場合は警告メッセージが表示されます

* * *

使用方法
----

<<<<<<< HEAD
### Step 1: セッション開始
=======
### 세션 시작
>>>>>>> 4c3c7c0c0e6fb372c6d1d5712fa480172c7920e5

1. `http://localhost:3000/test/session-start.html` にアクセス
2. **「入店」** ボタンをクリック
3. 自動的にキオスク画面(`index.html`)へ移動
   * URLに `sessionId` と `expireTime` が自動付与

<<<<<<< HEAD
### Step 2: キオスクフロー進行
=======
### 키오스크 플로우 진행
>>>>>>> 4c3c7c0c0e6fb372c6d1d5712fa480172c7920e5

1. **初期画面**: 「スタート」ボタンをクリック
2. **カート画面**: 3秒後に自動遷移
3. **スキャン画面**:
   * **「商品認識シミュレーション」** ボタンをクリック (実際のAIの代わり)
   * WebSocketを通じてサーバーにリクエスト送信
4. **レシート画面**: 自動遷移 (1秒待機)
5. **終了画面**: 5秒後に初期画面へ復帰

<<<<<<< HEAD
### Step 3: 結果確認
=======


* **コンソールログ**: ブラウザ開発者ツールでセッション情報、スキャン結果を確認
* **ファイル確認**:
  * `test/paidSessions.json`: 入店セッション記録
  * `test/refunds.json`: 返金履歴記録

* * *

<<<<<<< HEAD
APIドキュメント
------------
=======



### 1. セッション開始

    POST /api/session/start

**Response:** {"sessionId": "uuid-here","paidAmount": 100000,"entryTime": "2025-06-12T09:00:00.000Z","expireTime": "2025-06-12T09:10:00.000Z"}

### 2. 返金処理

    POST /api/refund
    Content-Type: application/json
    
    {
      "sessionId": "uuid-here",
      "scannedTotal": 8650
    }

**Response:**
    {
      "message": "환불 완료",
      "refundAmount": 91350
    }

### 3. WebSocketイベント

**Client → Server:** {"action": "simulateScan"}

**Server → Client:** {"type": "scanResult","items": [{"상품명": "クリンラップ 50M","판매가격": 2300,...}],"totalPrice": 8650,"refund": 91350}

* * *

主要実装詳細
-----------

### セッション有効期限処理

    // バックエンド: 10分後に有効期限時刻を設定
    const expireTime = new Date(Date.now() + 10 * 60 * 1000);
    
    // フロントエンド: 残り時間を計算後タイマー設定
    const timeRemaining = expireTimestamp - Date.now();
    setTimeout(() => {
      alert("세션이 만료되었습니다.");
      sessionStorage.clear();
      goToScreen("screen-start");
    }, timeRemaining);

### WebSocket リアルタイム通信

    // サーバー: 商品認識結果を送信
    ws.send(JSON.stringify({
      type: "scanResult",
      items: [...],
      totalPrice: 8650,
      refund: 91350
    }));
    
    // クライアント: 受信後画面遷移
    socket.onmessage = (event) => {
      const data = JSON.parse(event.data);
      if (data.type === "scanResult") {
        goToScreen("screen-receipt");
      }
    };

### Mock シミュレーションロジック

    exports.simulateScan = () => {
      const mockProducts = JSON.parse(fs.readFileSync('mock-products.json'));
      const selectedItems = mockProducts.sort(() => 0.5 - Math.random()).slice(0, 3);
      const totalPrice = selectedItems.reduce((sum, item) => sum + item.판매가격, 0);
      return {
        items: selectedItems,
        totalPrice,
        refund: 100000 - totalPrice
      };
    };

* * *

画面構成
--------

| 画面       | ID               | 説明                   | 遷移条件        |
| -------- | ---------------- | -------------------- | ----------- |
| 初期画面     | `screen-start`   | スタートボタン              | ボタンクリック     |
| カート案内画面  | `screen-basket`  | 商品を置くよう誘導            | 3秒自動        |
| スキャン中    | `screen-scan`    | ローディング + シミュレーションボタン | WebSocket受信 |
| レシート出力画面 | `screen-receipt` | 返金完了表示               | 1秒自動        |
| 終了挨拶画面   | `screen-goodbye` | お礼の挨拶                | 5秒後に初期化     |

* * *

プロトタイプの制限と今後の改善
---------------

### 現在の制限事項

* ❌ Mockデータベース (実際のAI認識未連携)
* ❌ JSONファイル保存 (DB未使用)
* ❌ 実際の決済システム未連携
* ❌ RFIDリーダー未連携
* ❌ 超音波センサー未連携
* ❌ プリンター連携未実装

### 今後の改善方向

    ✅ AI商品認識モデル連携 (カメラベース)
    ✅ PostgreSQL/MongoDB導入
    ✅ 実際のPG社連携 (トスペイメンツ、イニシスなど)
    ✅ HTTPS + WSS セキュア通信
    ✅ レシートプリンタードライバー連携
    ✅ 管理者ダッシュボード構築

* * *


テスト環境
=======


### 必須要件

* Node.js v14以上
* Webブラウザ (Chrome、Edge推奨)
* WebSocket対応ブラウザ

### 確認事項

1. **ポート競合確認**: 3000番ポートが使用中の場合は`app.js`で変更const PORT = 3000; // → 別のポートに変更
2. **WebSocket接続失敗時**:// public/app.js 修正socket = new WebSocket("ws://localhost:3000");// → ポート番号確認後修正

* * *

貢献者
---

* **チェ・スンヨン** - バックエンド開発、システムアーキテクチャ設計、WebSocket実装
* **チームメンバー3名** - フロントエンドUI、AIモデル開発 (その後継続)
1. 
   
   


