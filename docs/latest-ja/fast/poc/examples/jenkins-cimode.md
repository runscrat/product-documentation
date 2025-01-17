[jenkins-config-pipeline]:      https://jenkins.io/doc/book/pipeline
[fast-node-token]:              ../../operations/create-node.md
[jenkins-parameterized-build]:  https://wiki.jenkins.io/display/JENKINS/Parameterized+Build
[jenkins-example-env-var]:     ../../../images/fast/poc/common/examples/jenkins-cimode/jenkins-add-token-example.png
[fast-example-jenkins-result]:  ../../../images/fast/poc/common/examples/jenkins-cimode/jenkins-result-example.png
[fast-ci-mode-record]:          ../ci-mode-recording.md#environment-variables-in-recording-mode
[fast-ci-mode-test]:            ../ci-mode-testing.md#environment-variables-in-testing-mode
[mail-to-us]:                   mailto:support@wallarm.com
[fast-examples-github]:         https://github.com/wallarm/fast-examples 
[fast-example-jenkins]:         https://jenkinsfast.demo.wallarm.com/

# FASTとJenkinsの統合

FASTのCIモードをJenkinsワークフローに統合する設定は `Jenkinsfile` ファイルで行います。 Jenkinsワークフロー設定の詳細については、[Jenkinsの公式ドキュメンテーション][jenkins-config-pipeline]を参照してください。

## FASTノードトークンの受け渡し

[FASTノードトークン][fast-node-token]を安全に使用するためには、[プロジェクト設定の環境変数][jenkins-parameterized-build]でその値を渡してください。

![!Jenkins環境変数の受け渡し][jenkins-example-env-var]

--8<-- "../include-ja/fast/fast-cimode-integration-examples/configured-workflow.md"

## リクエスト録画のステップを追加する

--8<-- "../include-ja/fast/fast-cimode-integration-examples/request-recording-setup.md"

??? info "記録モードでFASTノードを実行する自動テストステップの例"
    ```
    stage('Run autotests with recording FAST node') {
          steps {
             sh label: 'create network', script: 'docker network create my-network'
             sh label: 'run fast with recording', script: 'docker run --rm  --name fast -d -e WALLARM_API_TOKEN=$WALLARM_API_TOKEN -e CI_MODE=recording -e WALLARM_API_HOST=us1.api.wallarm.com -p 8088:8080 --network my-network wallarm/fast'
             sh label: 'run selenium', script: 'docker run --rm -d --name selenium -p 4444:4444 --network my-network -e http_proxy=\'http://fast:8080\' -e https_proxy=\'https://fast:8080\' selenium/standalone-firefox:latest'
             sh label: 'run application', script: 'docker run --rm --name app-test --network my-network -e CAPYBARA_SERVER_HOST=app-test -p 3000:3000 app-test bundle exec rspec spec/features/posts_spec.rb'
             sh label: 'stop selenium', script: 'docker stop selenium'
             sh label: 'stop fast', script: 'docker stop fast'
             sh label: 'remove network', script: 'docker network rm my-network'
          }
       }
    ```

    この例には以下のステップが含まれています：

    1. Dockerネットワーク `my-network` を作成します。
    2. ネットワーク `my-network` 上で記録モードのFASTノードを実行します。
    3. ネットワーク `my-network` 上でFASTノードをプロキシとして使用する自動テストツールのSeleniumを実行します。
    4. テストアプリケーションと自動テストを実行します。
    5. SeleniumとFASTノードを停止します。
    6. `my-network` ネットワークを削除します。

## セキュリティテストのステップを追加する

--8<-- "../include-ja/fast/fast-cimode-integration-examples/security-testing-setup.md"

??? info "セキュリティテストステップの例"

    ```
    stage('Run security tests') {
          steps {
             sh label: 'create network', script: 'docker network create my-network'
             sh label: 'start application', script: ' docker run --rm -d --name app-test --network my-network -e CAPYBARA_SERVER_HOST=app-test -p 3000:3000 app-test'
             sh label: 'run fast in testing mode', script: 'docker run --name fast -e WALLARM_API_TOKEN=$WALLARM_API_TOKEN -e CI_MODE="testing" -e WALLARM_API_HOST="us1.api.wallarm.com"  --network my-network -e TEST_RUN_URI="http://app-test:3000" --rm wallarm/fast'
             sh label: 'stop application', script: ' docker stop app-test '
            sh label: 'remove network', script: ' docker network rm my-network '
          }
       }
    ```

    この例には以下のステップが含まれています：

    1. Dockerネットワーク `my-network` を作成します。
    2. `my-network` ネットワーク上でテストアプリケーションを実行します。
    3. ネットワーク `my-network` 上でテストモードのFASTノードを実行します。 `TEST_RECORD_ID` 変数は省略されています。というのも、基準リクエストのセットは現在のパイプラインで作成され、最後に記録されたものだからです。テストが終了したらFASTノードは自動的に停止します。
    4. テストアプリケーションを停止します。
    5. `my-network` ネットワークを削除します。

## テストの結果を取得する

セキュリティテストの結果はJenkinsのインターフェースに表示されます。

![!テストモードでFASTノードを実行する結果][fast-example-jenkins-result]

## 他の例

FASTをJenkinsワークフローに統合する例は、私たちの [GitHub][fast-examples-github] と [Jenkins][fast-example-jenkins]で見つけることができます。

!!! info "その他の質問"
    FASTの統合に関する質問がある場合は、[こちらからお問い合わせください][mail-to-us]。