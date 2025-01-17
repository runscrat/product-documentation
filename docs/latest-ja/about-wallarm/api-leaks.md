# API漏洩の修復

Wallarmプラットフォームの**API漏洩**モジュールは、APIトークンの漏洩をチェックするために数千の公開リポジトリと情報源を積極的にスキャンし、デプロイされたWallarm [ノード](../installation/supported-deployment-options.md)を使用して漏洩した認証情報の使用をブロックすることができます。この記事では、API漏洩の概要、対処する問題、目的、主な可能性について説明します。

**API漏洩**モジュールの使用方法については、[ユーザーガイド](../user-guides/api-leaks.md)を参照してください。

![!API漏洩](../images/about-wallarm-waf/api-leaks/api-leaks.png)

## API漏洩が対処する問題

あなたの組織は、APIの異なる部分へのアクセスを提供するために、多数のAPIトークンを使用することがあります。これらのトークンが漏洩すると、それはセキュリティ上の脅威となります。

APIを保護するためには、公開リポジトリを監視して、漏洩したAPIトークンを見つけ出す必要があります。一つでも見逃せば、あなたはまだリスクにさらされています。これを達成するためには、大量のデータを何度も何度も分析し続ける必要があります。

漏洩したAPIの秘密が見つかった場合、あなたのAPIへの損害を防ぐためには多面的な対応が必要です。これには、漏洩した秘密が使用されているすべての場所を見つけ出し、そこでそれらを再生成し、コンプロマイズされたバージョンの使用をブロックすることが含まれます。そして、これは迅速に、そして100％完全に行わなければなりません。これを手動で行うことは困難です。

**API漏洩**Wallarmモジュールは、以下の機能を提供することでこれらの問題を解決します：

* 公開リソースからの漏洩したAPIトークンの自動検出と、検出された漏洩のWallarm Console UIでのログ記録。
* リスクレベルの検出。
* 漏れを手動で追加する機能。
* 漏洩したデータ問題が各ケースでどのように修復されるべきかを自己判断する能力。

## 見つかった漏洩の可視化

**API漏洩**セクションは、見つかったAPI漏洩に関する現状を豊かに視覚化します。この表現はインタラクティブで、ダイアグラム要素をクリックしてリスクレベルと情報源により漏洩をフィルタリングすることができます。

![!API漏洩 - 可視化](../images/about-wallarm-waf/api-leaks/api-leaks-visual.png)

## API漏洩へのアクセス

* 漏洩したAPIトークンの脅威を緩和するためには、Wallarm [ノード](../user-guides/nodes/nodes.md)がデプロイされるべきです。
* デフォルトでは、API漏洩モジュールは無効になっています。モジュールへのアクセスを取得するには、[Wallarmの技術サポート](mailto:support@wallarm.com) にリクエストを送ってください。