# カスタムルールセットの構築とアンロード

カスタムルールセットは、特定のクライアントトラフィックの処理の詳細を定義します（例えば、カスタム攻撃検出ルールの設定や機密データのマスキングが可能）。 Wallarmノードは、受信リクエストの分析中にカスタムルールセットに依存しています。

カスタムルールの変更はすぐには有効になりません。変更は、カスタムルールセットの**構築**および**フィルタリングノードへのアンロード**が完了した後にのみ、リクエスト分析プロセスに適用されます。

## カスタムルールセットの構築

Wallarmコンソール → **ルール**で新しいルールを追加したり、既存のルールを削除したり変更したりすると、カスタムルールセットの構築が開始されます。構築プロセス中に、ルールは最適化され、フィルタリングノード用に採用された形式にコンパイルされます。カスタムルールセットの構築プロセスは通常、ルール数が少ない場合は数秒から、複雑なルールツリーの場合は最大1時間程度かかります。

カスタムルールセットの構築ステータスと予想される完了時間がWallarmコンソールに表示されます。進行中のビルドがない場合、インターフェイスには最後に完了したビルドの日付が表示されます。

![!ビルドステータス](../../images/user-guides/rules/build-rules-status.png)

## カスタムルールセットをフィルタリングノードにアンロードする

カスタムルールセットの構築は、フィルタリングノードとWallarmクラウドの同期中にフィルタリングノードにアンロードされます。デフォルトでは、フィルタリングノードとWallarmクラウドの同期は2から4分ごとに実行されます。[フィルタリングノードとWallarmクラウドの同期設定の詳細はこちら→](../../admin-en/configure-cloud-node-synchronization-en.md)

カスタムルールセットをフィルタリングノードにアンロードするステータスは、ファイル`/var/log/wallarm/syncnode.log`に記録されます。