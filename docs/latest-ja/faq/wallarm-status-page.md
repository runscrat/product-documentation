# Wallarm サービスステータスページ

このガイドはWallarmのサービスステータスページの詳細について説明します。

## Wallarmのサービスの利用状況を示すページはありますか？

はい、Wallarmのステータスページは[ここ](https://status.wallarm.com)から利用できます。このページではWallarm ConsoleおよびWallarm APIサービスの利用可能状況について、ライブデータと過去のデータをWallarm Cloudごとに表示します：

* **Wallarm US Cloud**
* **Wallarm EU Cloud**

![!Wallarm status page](../images/status-page.png)

## サービスステータスが変わったときに通知は受け取れますか？

はい、更新を購読している場合は通知を受け取れます。購読するには **SUBSCRIBE TO UPDATES**をクリックし、購読チャンネルを選択してください:

* **Email** Wallarmがインシデントを作成、更新、または解決したときに通知を受け取る
* **SMS** Wallarmがインシデントを作成または解決したときに通知を受け取る
* **Slack** インシデントの更新とメンテナンスステータスのメッセージを受け取る
* **Webhook** Wallarmがインシデントを作成、更新、解決、またはサービスステータスを変更したときに通知を受け取る

## サービスステータスは何を意味しますか？

* **劣化したパフォーマンス** は、サービスが動作しているが遅延していたり、他のマイナーな問題が発生していることを意味します。
* **部分的な停止** は、クライアントの一部でサービスが完全に機能していないことを意味します。
* **重大な停止** は、サービスが完全に利用できないことを意味します。

## インシデントは何時作成されますか？

インシデントは、サービスがダウンタイムを経験しているときに作成されます。ダウンタイム関連イベント中に、問題の説明、それについて何を行っているか、及び問題が解決される見込みがいつかについてのページを追加します。

時間と共に、インシデントの原因が特定され、特定されたインシデントが修正され、インシデントのステータスが現行の状況を反映するように更新されます。