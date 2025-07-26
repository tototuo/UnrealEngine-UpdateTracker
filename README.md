# Unreal Engine Update Tracker

[English version](README.en.md)

このプロジェクトは、Unreal EngineのプライベートGitHubリポジトリの更新を定期的に監視し、AI（Google Gemini）を使って重要な変更（新機能、仕様変更など）を要約し、GitHub Discussionsにレポートとして投稿する自動化サービスです。

<table><tr><td>
<img width="644" alt="image" src="https://github.com/pafuhana1213/Screenshot/blob/master/Report_sample_jp.png" />
</td></tr></table>

注意：この画像はレポートの例であり、書かれている内容は完全にダミーです。UnrealEngineで実際に行われた更新内容ではありません。

## 🌟 主な機能

-   **自動更新チェック:** GitHub Actionsを使い、スケジュール（毎日日本時間午前8時 / UTC 23:00）または手動でUEリポジトリの最新コミットをチェックします。
-   **AIによる要約:** Gemini APIがコミット内容を分析し、「新機能」「仕様変更」などのカテゴリに分類し、内容を要約します。
-   **Discussionへの投稿:** 生成されたレポートを、リポジリのGitHub Discussionsに「Unreal Engine Daily Report」として投稿します。
-   **Slack通知:** レポートの内容を、指定したSlackチャンネルにも同時に通知できます。
-   **Discord通知:** レポートの内容を、指定したDiscordチャンネルにも同時に通知できます。

## 🚀 最新レポートを購読する

このツールを自分でセットアップしなくても、更新レポートの結果を購読できます。
以下のリポジトリでは、毎日定時に生成されたレポートがGitHub Discussionsに投稿されています。

[**UnrealEngine-UpdateTrackerReport リポジトリを購読する**](https://github.com/pafuhana1213/UnrealEngine-UpdateTrackerReport)

**注意:** このレポートリポジトリはプライベートであり、閲覧するには[Unreal Engineのソースコードリポジトリへのアクセスが許可されたGitHubアカウント](https://www.unrealengine.com/ja/ue-on-github)が必要です。

## ✨ よろしければご支援を！

このツールが、皆さんの日々のUEキャッチアップに少しでも役立っていれば、とても嬉しいです。

開発は一個人が趣味と実益を兼ねて、コーヒー代やAPI利用料を自腹でやりくりしながら進めています☕
もし「このツール、なかなか良いじゃん！」と思っていただけたら、GitHub Sponsorsで応援していただけると、開発の大きな励みになります。

[💖 **GitHub Sponsorsで応援する**](https://github.com/sponsors/pafuhana1213)

---

**ここから先は、ご自身でこのツールをフォークしてカスタマイズしたい方向けのドキュメントです。**

## 🛠️ セットアップ方法

1.  **このリポジトリをフォーク (Fork):**
    右上の **Fork** ボタンをクリックして、このリポジトリを自身のGitHubアカウントにコピーします。

2.  **基本シークレットの設定:**
    まず、ツールの動作に必須となる以下のシークレットを、リポジトリの `Settings` > `Secrets and variables` > `Actions` に登録します。
    -   `UE_REPO_PAT`: Unreal Engineのプライベートリポジトリ (`EpicGames/UnrealEngine`) への読み取りアクセス権を持つ[Personal Access Token (PAT)](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)を登録します。
    -   `GEMINI_API_KEY`: [Google AI Studio](https://aistudio.google.com/app/apikey) で取得したAPIキーを登録します。

3.  **通知先の設定 (少なくとも1つ必須):**
    次に、レポートの通知先を選択し、設定します。**GitHub Discussion**, **Slack**, **Discord** のいずれか、またはすべてを設定できます。
    
    #### A) GitHub Discussionへの投稿設定
    チームでの議論や記録の永続化に適しています。
    1.  **Discussionsを有効化:** レポート投稿先リポジトリの `Settings` > `General` > `Features` で `Discussions` を有効化します。
    2.  **カテゴリを作成:** Discussionsタブでカテゴリを作成します（例: `Announcements`）。
    3.  **シークレットを追加:** 以下のシークレットを登録します。
        -   `DISCUSSION_REPO`: レポートを投稿する先の**プライベート**リポジトリ名（例: `MyOrg/MyTeamRepo`）。
        -   `DISCUSSION_REPO_PAT`: `DISCUSSION_REPO`にDiscussionを書き込む権限を持つPAT。

    #### B) Slackへの投稿設定
    リアルタイムな通知と迅速な情報共有に適しています。
    1.  **Incoming Webhookを作成:** [Slackのドキュメント](https://slack.com/intl/ja-jp/help/articles/115005265063-Slack-%E3%81%A7%E3%81%AE-Incoming-Webhook-%E3%81%AE%E5%88%A9%E7%94%A8)に従い、通知したいチャンネル用のWebhook URLを発行します。
    2.  **シークレットを追加:** 以下のシークレットを登録します。
        -   `SLACK_WEBHOOK_URL`: 上記で発行したIncoming WebhookのURL。
        -   `SLACK_CHANNEL`: 通知を投稿するSlackチャンネル名（例: `#ue-updates`）。

    #### C) Discordへの投稿設定
    Slackと同様に、リアルタイムな通知に適しています。
    1.  **Webhookを作成:** [Discordのドキュメント](https://support.discord.com/hc/ja/articles/228383668-%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC%E3%81%A7Webhooks%E3%82%92%E4%BD%BF%E3%81%86%E3%81%AB%E3%81%AF)に従い、**通知したいチャンネル用のWebhook URL**を作成します。Discordでは、このURL自体が投稿先チャンネルを決定するため、Slackのように別途チャンネル名を指定する必要はありません。
    2.  **シークレットを追加:** 以下のシークレットを登録します。
        -   `DISCORD_WEBHOOK_URL`: 上記で作成したWebhookのURL。

 **⚠️ 重要: 安全な運用に関する推奨事項**
 Unreal Engineの更新履歴は、Epic Gamesのライセンス契約に基づき、許可されたアカウントのみがアクセスできる機密情報です。意図しない情報漏洩を防ぐため、このツールは**少なくとも1つの通知先が設定されていないと動作を停止します。**

**推奨される設定:**
レポートの投稿先（`DISCUSSION_REPO`またはSlackチャンネル）は、**Unreal Engineのソースコードリポジトリのフォーク、または同等のアクセス権を持つメンバーのみが参加しているプライベートな場所**を指定することを強く推奨します。これにより、ライセンス契約を遵守し、安全に情報を共有できます。

## 🏃‍♀️ 実行方法

-   **自動実行:** 設定されたスケジュール（デフォルトでは毎日日本時間午前8時 / UTC 23:00）になると、自動的にワークフローが実行されます。
-   **手動実行:** リポジトリの`Actions`タブに移動し、`Unreal Engine Update Tracker`ワークフローを選択して、`Run workflow`ボタンから手動で実行することも可能です。**注意: 手動実行はリポジトリの管理者のみが可能です。**
    -   **Report Language:** レポートを出力したい言語を自由に入力します（例: `Japanese`, `English`）。デフォルトは `Japanese` です。
    -   **Commit Scan Limit:** 手動実行時にスキャンする最新コミット数を指定できます。（デフォルト: 過去24時間）
    -   **Discussion Category:** レポートを投稿するDiscussionカテゴリ名。デフォルトは `Daily Reports` です。
    -   **Gemini Model:** 解析に使用するAIモデル名。デフォルトは `gemini-2.5-pro` です。
    -   **Slack Webhook URL:** 一時的に使用するSlack Webhook URL。Secretの値を上書きします。
    -   **Slack Channel:** 一時的に使用するSlackチャンネル名。Secretの値を上書きします。
    -   **Discord Webhook URL:** 一時的に使用するDiscord Webhook URL。Secretの値を上書きします。

-   **各種デフォルト値の変更:**
    スケジュール実行時や手動実行時のデフォルト値は、リポジトリの **Variables** で設定することで変更できます。`Settings` > `Secrets and variables` > `Actions` の `Variables` タブから、以下の変数を設定します。
    -   `REPORT_LANGUAGE`: デフォルトのレポート言語（例: `English`）
    -   `DISCUSSION_CATEGORY`: デフォルトの投稿先カテゴリ名（例: `Daily Reports`）
    -   `GEMINI_MODEL`: デフォルトで使用するAIモデル（例: `gemini-2.5-pro`）
    -   `UE_BRANCH`: 監視対象のブランチ名（例: `release`）。デフォルトは `ue5-main` です。

## 🎨 カスタマイズ

### レポートフォーマットの変更

レポートのカテゴリ、要約のスタイル、全体の構成など、出力されるフォーマットはAIへの指示（プロンプト）によって決定されます。

より詳細なレポートが欲しい、あるいは特定の情報を強調したいなど、フォーマットを自由に変更したい場合は、リポジトリのルートにある `prompts/report_prompt.md` ファイルを直接編集してください。このファイルを変更することで、Pythonコードに触れることなく、AIの振る舞いをカスタマイズできます。

## 📝 ライセンスと利用上の注意 (License and Important Notices)

**本ツールを利用する前に、以下の内容を必ずお読みください。**

-   **利用者の責任:** このツールは、Unreal Engineのライセンス契約を遵守するよう慎重に設計されていますが、最終的な運用責任は利用者にあります。特に、レポートの投稿先 (`DISCUSSION_REPO`) には、**必ずアクセスが制限されたプライベートリポジトリを指定してください。** 公開リポジトリに投稿した場合、ライセンス違反となる可能性があります。

-   **APIキーと課金:**
    *   本ツールはGoogle Gemini APIを利用しており、利用量に応じた料金が発生する場合があります。
    *   このリポジトリをフォークして利用する場合、**フォークしたリポジトリのオーナーが自身のAPIキーに対する全ての課金責任を負います。**
    *   Unreal Engineの規約を確実に遵守するため、**送信データがAIの学習に利用されないライセンスのAPIキーを使用することを強く推奨します。**

-   **設計上の安全性:**
    *   ライセンス違反のリスクを最小限に抑えるため、本ツールはAIへの情報提供に際し、**Unreal Engineのソースコードやコード差分（diff）そのものは一切送信しません。** 分析対象となるのは、コミットメッセージと変更されたファイルパスのみです。

-   **実行に関する注意:**
    *   このスクリプトは、設定に従って実際にGitHub Discussionsへ投稿を行います。テスト実行の際はご注意ください。
    *   各種APIには利用制限（レートリミット）が存在します。


---

