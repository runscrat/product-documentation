# 脆弱性の管理

脆弱性は、攻撃者がシステムで不正な行為を行うために悪用する可能性のあるインフラストラクチャのセキュリティ欠陥です。 Wallarmコンソールの**脆弱性**セクションでは、Wallarmがシステムで検出したセキュリティ欠陥を分析、管理できます。

Wallarmは、以下を含むセキュリティ弱点を[見つける](../about-wallarm/detecting-vulnerabilities.md)ために様々な手法を使用します：

* **パッシブ検出**：脆弱性は、発生したセキュリティインシデントのために見つかった。
* **アクティブな脅威の検証**：脆弱性は、攻撃検証プロセス中に見つかった
* **脆弱性スキャナー**：脆弱性は、[公開資産](scanner.md)のスキャンプロセス中に発見されました。

Wallarmは、全ての検出した脆弱性の履歴を**脆弱性**のセクションに保存します：

![!脆弱性タブ](../images/user-guides/vulnerabilities/check-vuln.png)

## 脆弱性のライフサイクル

脆弱性のライフサイクルには、評価、修復、および検証の段階が含まれます。各段階で、Wallarmは問題を徹底的に対処し、システムを強化するための必要なデータを提供します。さらに、Wallarm Consoleは**アクティブ**および**クローズド**のステータスを使用して脆弱性のステータスを簡単に監視および管理する機能を提供します。

* **アクティブ**ステータスは、脆弱性がインフラストラクチャに存在することを示します。
* **クローズド**ステータスは、脆弱性がアプリケーション側で解決されたか、誤検知であると判断された場合に使用されます。

    身元が確認されたエンティティが誤って脆弱性として特定されると、[誤検知](../about-wallarm/detecting-vulnerabilities.md#false-positives)が発生します。誤検知だと思われる脆弱性に出くわした場合は、脆弱性メニューの適切なオプションを使用して報告できます。これにより、Wallarmの脆弱性検出の精度が向上します。Wallarmは、脆弱性を誤検知として再分類し、ステータスを**クローズド**に変更し、以降[再チェック](#verifying-vulnerabilities)対象になりません。

脆弱性を管理する際、手動で脆弱性のステータスを切り替えることができます。また、Wallarmは定期的に脆弱性を[再チェック](#verifying-vulnerabilities)し、結果に応じて脆弱性のステータスを自動的に変更します。

![!脆弱性ライフサイクル](../images/user-guides/vulnerabilities/vulnerability-lifecycle.png)

脆弱性ライフサイクルの変更は、脆弱性変更履歴で反映されます。

## 脆弱性の評価と修復

Wallarmは、リスクレベルを評価し、セキュリティ問題に対処するための手順を講じるために必要な詳細を、各脆弱性に提供します：

* Wallarmシステム内の脆弱性の一意の識別子
* 脆弱性が悪用される結果の危険を示すリスクレベル

    Wallarmは、Common Vulnerability Scoring System（CVSS）フレームワーク、脆弱性が悪用される可能性、そのシステムへの潜在的な影響などを使用して、自動的に脆弱性リスクを示します。ユニークなシステム要件とセキュリティ優先事項に基づいて、リスクレベルを自分の値に変更することが可能です。
* 脆弱性の種類([ターゲットの脆弱性](../attacks-vulns-list.md)は、脆弱性を悪用する攻撃のタイプにも対応します
* 脆弱性が存在するドメインとパス
* 悪意のあるペイロードを脆弱性に渡すために使用できるパラメータ
* 脆弱性が特定された方法([検出](../about-wallarm/detecting-vulnerabilities.md#vulnerability-detection-methods)の手法)
* 脆弱性が悪用されると影響を受ける可能性のある対象コンポーネント。これは**サーバー**、**クライアント**、**データベース**になる可能性があります。
* 脆弱性が検出された日付と時間
* 脆弱性の最後の[検証日](#verifying-vulnerabilities)
* 詳細な脆弱性の説明、悪用の例、および推奨される修復手順
* 関連するインシデント
* 脆弱性ステータス変更の履歴

[検索文字列](search-and-filters/use-search.md)と事前に定義されたフィルターを使用して脆弱性をフィルタリングできます。

![!詳細な脆弱性情報](../images/user-guides/vulnerabilities/vuln-info.png)

すべての脆弱性は、システムが不正な行為に対してより脆弱になるため、アプリケーション側で修正する必要があります。脆弱性を修正できない場合は、[バーチャルパッチ](rules/vpatch-rule.md)ルールを使用して関連する攻撃をブロックし、インシデントのリスクを排除することができます。

## 脆弱性の確認<a href="../../about-wallarm/subscription-plans/#subscription-plans"><img src="../../images/api-security-tag.svg" style="border: none;margin-bottom: -4px;"></a>

Wallarmは定期的にアクティブな脆弱性と解決済みの脆弱性の両方を再チェックします。これには、以前に発見されたセキュリティ問題のためのインフラストラクチャのリピートテストが含まれます。再チェック結果が脆弱性が存在しなくなったことを示している場合、Wallarmはそのステータスを**クローズド**に変更します。これは、サーバーが一時的に利用できない場合にも発生する可能性があります。反対に、閉じた脆弱性の再チェックがそれがアプリケーションに存在することを示している場合、Wallarmはそのステータスを再度**アクティブ**に変更します。

アクティブな脆弱性と1か月以内に修正された脆弱性は毎日1回再チェックされます。1か月以上前に修正された脆弱性は、週に1回再チェックされます。

初期脆弱性検出手法次第で、テストは**脆弱性スキャナー**または**アクティブな脅威の確認**モジュールによって実行されます。自動再チェックプロセスの設定は、[**設定**](#configuring-vulnerability-detection)ボタンを介して制御できます。

パッシブに検出された脆弱性を再チェックすることはできません。

脆弱性を手動で再チェックする必要がある場合は、脆弱性メニューの適切なオプションを使用して再チェックプロセスを開始できます：

![!再チェック可能な脆弱性](../images/user-guides/vulnerabilities/recheck-vuln.png)

## 脆弱性検出の設定

脆弱性検出の設定は、**設定**ボタンを使用して微調整でき、以下のオプションがあります：

* 脆弱性スキャナを使用して検出したい特定の脆弱性のタイプを選択できます。デフォルトでは、スキャナーはすべての可能な脆弱性タイプを対象に設定されます。
* **基本スキャナ機能** を有効/無効にすることができます。これには、脆弱性および[公開資産](scanner.md)の検出プロセスの両方が含まれます。デフォルトでは、この機能は有効になっています。

    同じ切り替えスイッチを**スキャナー**セクションでも見つけることができます。1つのセクションでスイッチを切り替えると、他のセクションの設定も自動的に更新されます。
* スキャナを使用した脆弱性の再チェックを有効/無効にすることができます。**脆弱性再チェック**オプションを使用します。
* 脆弱性の検出と再チェックのための**アクティブな脅威の確認**モジュールを有効/無効にします。注意してください、このオプションはモジュール自体を制御し、再チェックプロセスだけでなく。

    デフォルトでは、このモジュールは無効になっており、有効化する前にその設定[ベストプラクティス](../admin-en/attack-rechecker-best-practices.md)を学びます。

![!脆弱性スキャンの設定](../images/user-guides/vulnerabilities/vuln-scan-settings.png)

さらに、UIの[**スキャナー**](scanner.md)セクションでは、どの公開資産を脆弱性スキャナーでスキャンするべきか、各資産に対してスキャナーが生成するRPS/RPMを制御することができます。

## 脆弱性レポートのダウンロード

UI内の対応するボタンを使用して、脆弱性データをPDFまたはCSVのレポートにエクスポートすることができます。Wallarmは指定されたアドレスにレポートをメールで送ります。

PDFは、脆弱性とインシデントの要約を含む視覚的に豊かなレポートを提示するのに適しています、一方、CSVは技術的な目的により適しており、各脆弱性に関する詳細な情報を提供します。ダッシュボードの作成、最も脆弱なAPIホスト/アプリケーションのリストの作成など、CSVを使用することができます。

## 脆弱性取得のAPI呼び出し

脆弱性の詳細を取得するには、Wallarm Console UIを使用する代わりに、Wallarm APIを直接[呼び出す](../api/overview.md)ことができます。以下に該当するAPI呼び出しの例を示します。

過去24時間以内にステータス**アクティブ**の最初の50の脆弱性を取得するには、次のリクエストを使用し、`TIMESTAMP`を24時間前の日付に[Unix Timestamp](https://www.unixtimestamp.com/)形式で変換して置き換えます：

--8<-- "../include-ja/api-request-examples/get-vulnerabilities.md"