# Unreal Engine Update Tracker

[English version](README.en.md)

このプロジェクトは、Unreal EngineのプライベートGitHubリポジトリの更新を定期的に監視し、AI（Google Gemini）を使って重要な変更（新機能、仕様変更など）を要約し、GitHub Discussionsにレポートとして投稿する自動化サービスです。

<table><tr><td>
<img width="644" alt="image" src="https://github.com/user-attachments/assets/ad69f54f-9e18-49db-8024-aa3052d97ffa" />
</td></tr></table>

注意：この画像はレポートの例であり、書かれている内容は完全にダミーです。UnrealEngineで実際に行われた更新内容ではありません。

## 🌟 主な機能

-   **自動更新チェック:** GitHub Actionsを使い、スケジュール（毎日日本時間午前8時 / UTC 23:00）または手動でUEリポジトリの最新コミットをチェックします。
-   **AIによる要約:** Gemini APIがコミット内容を分析し、「新機能」「仕様変更」などのカテゴリに分類し、内容を要約します。
-   **Discussionへの投稿:** 生成されたレポートを、リポジリのGitHub Discussionsに「Unreal Engine Daily Report」として投稿します。

## 🛠️ セットアップ方法

1.  **このリポジトリをフォーク (Fork):**
    右上の **Fork** ボタンをクリックして、このリポジトリを自身のGitHubアカウントにコピーします。これにより、GitHub Actionsのワークフローが自分のアカウントで実行できるようになります。

2.  **GitHub Discussionsを有効化:**
    リポジトリの `Settings` > `General` > `Features` に移動し、`Discussions` にチェックを入れて有効化します。

3.  **Discussionカテゴリの作成:**
    Discussionsタブで、レポートを投稿するためのカテゴリを作成します（例: `Announcements` や `Daily Reports`）。

4.  **リポジトリのSecretsを設定:**
    リポジトリの `Settings` > `Secrets and variables` > `Actions` に移動し、以下のリポジトリシークレットを登録します。
    -   `UE_REPO_PAT`: Unreal Engineのプライベートリポジトリ (`EpicGames/UnrealEngine`) への読み取りアクセス権を持つ[Personal Access Token (PAT)](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)を登録します。
    -   `GEMINI_API_KEY`: [Google AI Studio](https://aistudio.google.com/app/apikey) で取得したAPIキーを登録します。
    -   `DISCUSSION_REPO`: レポートを投稿する先の**プライベート**リポジトリ名（例: `MyOrg/MyTeamRepo`）。
    -   `DISCUSSION_REPO_PAT`: `DISCUSSION_REPO`で指定したリポジトリにDiscussionを書き込む権限を持つPAT。
  
 **⚠️ 重要: 安全な運用に関する推奨事項**  
 Unreal Engineの更新履歴は、Epic Gamesのライセンス契約に基づき、許可されたアカウントのみがアクセスできる機密情報です。意図しない情報漏洩を防ぐため、このツールは `DISCUSSION_REPO` と `DISCUSSION_REPO_PAT` が設定されていないと**動作を停止する仕様**になっています。

**推奨される設定:**
レポートの投稿先 (`DISCUSSION_REPO`) には、**Unreal Engineのソースコードリポジトリのフォーク、または同等のアクセス権を持つメンバーのみが参加している別のプライベートリポジトリ**を指定することを強く推奨します。これにより、ライセンス契約を遵守し、安全に情報を共有できます。

## 🏃‍♀️ 実行方法

-   **自動実行:** 設定されたスケジュール（デフォルトでは毎日日本時間午前8時 / UTC 23:00）になると、自動的にワークフローが実行されます。
-   **手動実行:** リポジトリの`Actions`タブに移動し、`Unreal Engine Update Tracker`ワークフローを選択して、`Run workflow`ボタンから手動で実行することも可能です。**注意: 手動実行はリポジトリの管理者のみが可能です。**
    -   **Discussion Category:** 実行時に、投稿先のディスカッションカテゴリ名を指定できます。（デフォルト: `Daily Reports`）
    -   **Gemini Model:** 解析に使用するGeminiのモデル名を一時的に指定できます。

-   **デフォルトモデルの変更:** スケジュール実行時などに使われるデフォルトのモデルを変更したい場合は、`.github/workflows/main.yml` ファイル内の1箇所を直接編集してください。
    ```yaml
    # .github/workflows/main.yml

    jobs:
      run-update-check:
        runs-on: ubuntu-latest
        env:
          DEFAULT_GEMINI_MODEL: 'gemini-2.5-pro' # <-- ここでデフォルトモデルを変更
    ```

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
