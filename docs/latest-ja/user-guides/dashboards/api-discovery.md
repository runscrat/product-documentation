# API ディスカバリーダッシュボード <a href="../../../about-wallarm/subscription-plans/#subscription-plans"><img src="../../../images/api-security-tag.svg" style="border: none;"></a>

**API ディスカバリー** Wallarm ダッシュボードは、[API ディスカバリー](../../about-wallarm/api-discovery.md) モジュールによって収集された API に関するデータを要約します。以下の指標に基づいた API インベントリーの包括的な概観を提供します：

* リスクレベル別のエンドポイント数
* API インベントリ全体、および過去 7 日間に新たに発見されたエンドポイントの中で [最もリスキー](../../about-wallarm/api-discovery.md#endpoint-risk-score) なエンドポイント

    最もリスキーなエンドポイントは、攻撃の対象になりやすい可能性が最も高いです。これは、アクティブな脆弱性、エンドポイントが [新規](../../about-wallarm/api-discovery.md#tracking-changes-in-api) または [シャドウ](../../about-wallarm/api-discovery.md#shadow-api) であること、その他のリスク要因によるものです。リスキーなエンドポイントごとに、目標とされたヒットの数が提供されます。

* 過去 7 日間における API の変更（新規、変更、未使用の API）
* 発見されたエンドポイントの総数と、それらが外部、内部のいずれであるか
* データグループ（個人、金融など）とタイプ別の API 内のセンシティブデータ
* API インベントリ：API ホストとアプリケーションによるエンドポイント数

![API Discovery widget](../../images/user-guides/dashboard/api-discovery-widget.png)

ダッシュボードは、頻繁に使用されるリスキーなエンドポイントや API が転送するセンシティブデータのボリュームが大きいなどの異常を検出することができます。さらに、常にチェックが必要な API の変更に注目を引き、セキュリティリスクを除外します。これにより、エンドポイントが攻撃の対象になるのを防ぐためのセキュリティ対策を実施するのに役立ちます。

ウィジェットの要素をクリックして **API ディスカバリー** セクションに移動し、フィルタリングされたデータを表示します。ヒット数をクリックすると、過去 7 日間の攻撃データが含まれた [イベントリスト](../events/check-attack.md) にアドレスされます。