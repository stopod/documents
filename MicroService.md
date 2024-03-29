### 受講メモ

内容: https://www.udemy.com/course/microservices-architecture-design/

---

## マイクロサービス概要

- モノリスとは  
  必要なコンポーネントがすべて一つのアプリの中に含まれているような構成

  モノリスの利点  
  開発しやすい、大きな変更を加えやすい、テストしやすい、デプロイしやすい、障害調査しやすい  
  ->時間経過で肥大化

- 肥大化した際の課題  
  影響範囲の特定が難しい、デプロイまでの道のりが長い、リリース時の調整が大変、リソース効率が悪い、信頼性が低い、技術スタックが陳腐化しやすい

---

- マイクロサービスとは  
  アプリケーションの構成要素を独立したサービス群へ分割し、それらサービス群を連携させることで一つのアプリケーションとして組み立てるようなアーキテクチャまたは開発手法

  マイクロサービス化する利点  
  修正範囲がサービスに限定される、CL/CD で必要なテストが容易になる、素早くリリースができる、サービスに合わせたスケールができる、障害分離しやすい、新しい技術を試しやすい

  どこまで分割すればマイクロなのか->明確な「解」はない  
  -> 大きすぎるは理解できるので、大きすぎると感じない大きさがちょうどよい大きさ

  マイクロサービス化の課題  
  サービスの適切な分割範囲を見極めるのが難しい、アプリケーションの設計が複雑になる、複数のサービスを同時修正しないと実現できないような機能の場合、デプロイ/リリースの調整が必要、アプリケーション全体のテストが難しくなる

- サービスとは  
  何かしらの機能を実現する単独動作、単独デプロイ/リリースできるコンポーネント  
  API、ロジック、データが一般的に含まれる  
  ->データもサービスごとに分割（ここがマイクロサービスのポイントらしい）

- マイクロサービスと SOA の違い  
   SAO: サービス指向設計（システムの機能単位を業務単位で決めていく設計=業務分析が必須）  
  | 観点 | SOA | マイクロサービス |  
  | :--------------------- | :------- |:------- |
  | アーキテクチャ | リソース共有 | 独立したサービス |
  | コンポーネント共有 | 共有する |共有しない |
  | サービス粒度 | 比較的大きい |非常に小さい |
  | データ | 共有 DB |独立 DB |
  | サービス間通信 | ESB |メッセージブローカー |
  | 通信プロトコル | SOAP |REST, gRPC |

- コンウェイの法則  
  システムを設計するあらゆる組織がその組織構造に倣った構造を持つ設計を生み出す  
  逆にする

---

## 設計・実装・テスト

- 同期  
  クライアントはリアルタイムに処理され、すぐにレスポンスされることを期待、  
  レスポンスがあるまで処理をブロックする

- 非同期  
  クライアントは即時処理を期待せず、レスポンスが時間をあけて返ってくる可能性を考慮する  
  クライアントは処理をブロックしない

- サービス間の通信方法  
  | | 1 対 1 | 1 対多 |  
  | :--------------------- | :------- |:------- |
  | 同期通信 | REST, gRPC | (なし) |
  | 非同期通信 | バッチ | メッセージブローカー, DB 共有 |

- REST とは  
  HTTP を利用したテキストベースの通信  
  「メソッド」と「リソース」で操作方法を表現

  - メリット  
    一般的  
    どの言語でも基本的にサポート  
    ツールが充実
  - デメリット  
    動詞の種類に限りがある  
    1 リクエストで複数リソース操作できない  
    バッファリングなしに直接接続するため可用性が下がる

- gRPC とは  
  HTTP/2 を利用したバイナリベースのプロトコル（RPC フレームワーク）  
  ".proto"ファイルに IF 定義を行い、各言語のひな形を自動作成

  - メリット  
    バイナリ+HTTP/2 で高速通信  
    複数言語に対応（C#, Go, Java, Node.js, Python...）
  - デメリット  
    古い環境だと HTTP/2 に対応していない  
    通信がバイナリなので監視が難しい  
    公式対応していない言語もある

- バッチとは  
  一定量の処理をまとめて実行する  
  一般的にはファイルや DB 参照を介した定期的な連携

- メッセージブローカーとは  
  非同期にメッセージを交換する仕組み  
  確実に後続サービスへ通知を行ってくれる  
  「キュー方式」と「Pub/Sub 方式」の 2 種類がある

  - キュー方式  
    処理量が多くても対応できる  
    （バッチ処理に対応できる、ラグは多少出る）
  - Pub/Sub 方式  
    メッセージブローカーが Consumer のリソースを考慮する必要がある  
    軽い処理に向く  
    リアルタイムに近づけられる
    専用ミドルウェアを介してメッセージを交換する

  - メリット  
    疎結合になる  
    メッセージのバッファリングができる
  - デメリット  
    パフォーマンスのボトルネックになる懸念  
    単一障害点になる懸念  
    運用複雑度の上昇
  - 代表的なメッセージブローカー  
    Apache Kafka, Apache ActiveMQ, RabbitMQ, Redis  
    以下はクラウド  
    MSK, MQ, ElastiCache, SQS, Queue Storage, Service Bus, Pub/Sub, Tasks

- DB 共有とは  
  View やテーブルそのものを公開してデータ連携する方法

  - メリット  
    大量のデータを移す必要がない  
    反映した即時のデータを扱える
  - デメリット  
    データ所有権が元データ側にあるため、データ変更を伴う修正の影響範囲が広がる

- マネージドサービスとは  
  通常サービスを稼働させる際、システム以外にも必要となる運用、保守、  
  障害対応などの業務もあわせて請け負ってくれるサービス  
  IasS, PaaS, SaaS(責任共有モデルというらしい、SaaS はアプリの使用に責任がある)

- サーキットブレーカー  
  一定回数通信に失敗した際、遮断するような仕組みを入れるやつ  
  何度かエラーになっているサービスに対しては即時エラーを返す  
  （= エラーだとしても応答を早くする）

  - 代表的なサーキットブレーカー  
    Istio(Kubernetes の拡張), Hystrix, resilience4j, App Mesh

- サービスディスカバリ  
  インスタンスが増える->アクセス先の IP アドレス/ポート番号がわからない

  - 「オートスケール」+ 「ロードバランサー」
  - サービスディスカバリ（マイクロサービスでよく言われるらしい）  
    数が増えると手動だと管理が大変になるのでこれを使うらしい  
    DNS っぽい
  - 代表的なサービスディスカバリ  
    Zookeper, Consul, Eureka, etcd

- API ゲートウェイ  
  マイクロサービス化を進めると、クライアントアプリからのアクセス先が増える  
  ->どのアドレスを呼び出せば良いか分からない  
  ->認証認可をそれぞれ実装すると冗長  
   = 入り口を一元化し、共通処理を吐き出す（API ゲートウェイ）

  - 代表的な API ゲートウェイ  
    Kong, Zuul, Tyk, Apigee, API Gateway

- ドメイン駆動設計  
  マイクロサービス化を進めた -> サービスに対して頻繁に変更が加わる  
  変化に弱い設計だと頻繁にリファクタが発生、工数ばかりかかるようになる、  
  これまでの手続き型の実装では変更箇所が多くなってしまう。  
  ↓  
  対策：オブジェクト指向設計（OOD）の原則に従って実装する  
  = ドメイン駆動設計（ドメイン知識を中心に据えた設計手法）

- サーガ  
  DB が分散した状態でシステム全体としてデータ整合性は担保できるのか？
  - 2 層コミットを使って担保する  
    通信が同期になり、可用性が下がるためマイクロサービスでは使わない
  - サーガを使って担保する
    - サーガは 3 フェーズに分割される  
      補償可能トランザクション（失敗する可能性がある）  
      　このトランザクションは API 呼び出し、成功もロールバックも作る必要がある  
      ピポットトランザクション  
      　ロールバックするかどうかの境界  
      再試行可能トランザクション  
      　必ず成功する
    - サーガの協調  
      コレオグラフィとオーケストレーション
