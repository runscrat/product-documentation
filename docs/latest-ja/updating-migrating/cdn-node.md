# CDNノードのアップグレード

これらの指示は、バージョン3.6以降で利用可能なWallarm CDNノードをアップグレードする手順を説明しています。

1. 保護対象のドメインのDNSレコードから、Wallarm CNAMEレコードを削除します。

    !!! warning "悪意のあるリクエストの緩和が停止します"
        CNAMEレコードが削除され、変更がインターネット上で有効になると、Wallarm CDNノードはリクエストのプロキシを停止し、正当なトラフィックと悪意のあるトラフィックは直接保護対象のリソースに向かいます。

        それは、削除されたDNSレコードが有効になったが、新しいノードバージョンのCNAMEレコードがまだ有効になっていないときに、保護されたサーバーの脆弱性が悪用されるリスクをもたらします。
1. 変更が伝播されるのを待ちます。実際のCNAMEレコードステータスは、Wallarmコンソール →  **ノード** → **CDN** → **ノード削除**で表示されます。
1. Wallarmコンソール → **ノード**からCDNノードを削除します。

    ![!ノードの削除](../images/user-guides/nodes/delete-cdn-node.png)
1. 同じドメインを保護する新しいバージョンのCDNノードを[指示](../installation/cdn-node.md)に従って作成します。

すべてのCDNノード設定はWallarmクラウドに保存されているため、新しいCDNノードはそれらを自動的に取得します。保護対象のドメインが変わっていなければ、手動でノードの設定を移動する必要はありません。