フィルタリングノードの構成パラメータは、デプロイされたDockerコンテナに以下の方法のいずれかで渡す必要があります:

* **環境変数で**。このオプションでは、基本的なフィルタリングノードパラメータのみを設定できます。ほとんどの[ディレクティブ][nginx-waf-directives]は、環境変数を介して設定することができません。
* **マウントされた設定ファイルで**。このオプションでは、任意の[ディレクティブ][nginx-waf-directives]を使用して、フィルタリングノードの完全な設定を行うことができます。この設定方法では、フィルタリングノードとWallarm Cloud接続設定の環境変数もコンテナに渡されます。