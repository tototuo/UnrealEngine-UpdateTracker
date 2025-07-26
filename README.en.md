# Unreal Engine Update Tracker

[Read this in Japanese](README.md)

This project is an automated service that periodically monitors updates to Unreal Engine's private GitHub repository, summarizes important changes (such as new features and specification changes) using AI (Google Gemini), and posts them as reports to GitHub Discussions.

<table><tr><td>
<img width="644" alt="image" src="https://github.com/pafuhana1213/Screenshot/blob/master/Report_sample_en.png" />
</td></tr></table>

Note: This image is an example of a report, and the content shown is entirely dummy data. It does not represent actual updates made to Unreal Engine.

## 🌟 Key Features

-   **Automatic Update Checks:** Uses GitHub Actions to check for the latest commits in the UE repository on a schedule (daily at 23:00 UTC / 8:00 AM JST) or manually.
-   **AI-Powered Summaries:** The Gemini API analyzes commit contents, categorizes them into sections like "New Features" and "Specification Changes," and provides a summary for each.
-   **Posting to Discussions:** The generated report is posted to the repository's GitHub Discussions as "Unreal Engine Daily Report."
-   **Slack Notifications:** The report content can also be sent simultaneously to a specified Slack channel.
-   **Discord Notifications:** The report content can also be sent simultaneously to a specified Discord channel.

## 🚀 Subscribe to the Latest Reports

You can subscribe to the update reports without setting up this tool yourself.
In the repository below, reports generated at a fixed time every day are posted to GitHub Discussions.

[**Subscribe to the UnrealEngine-UpdateTrackerReport Repository**](https://github.com/pafuhana1213/UnrealEngine-UpdateTrackerReport)

**Note:** This report repository is private and requires a [GitHub account authorized to access the Unreal Engine source code repository](https://www.unrealengine.com/en-US/ue-on-github) to view.

## ✨ Please Consider Supporting!

I hope this tool is helping you with your daily UE catch-up.

This tool is developed and maintained by a single person, covering costs like coffee and API fees out of pocket as a passion project. ☕
If you find this tool useful, please consider supporting its development through GitHub Sponsors. Your support would be a great encouragement and a huge motivation to keep this project going!

[💖 **Support the developer on GitHub Sponsors**](https://github.com/sponsors/pafuhana1213)

---

**The following documentation is for those who want to fork and customize this tool themselves.**

## 🛠️ Setup Instructions

1.  **Fork this repository:**
    Click the **Fork** button in the top-right corner to copy this repository to your own GitHub account.

2.  **Set Up Basic Secrets:**
    First, register the following secrets, which are essential for the tool to operate, in your repository's `Settings` > `Secrets and variables` > `Actions`.
    -   `UE_REPO_PAT`: A [Personal Access Token (PAT)](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) with read access to the private Unreal Engine repository (`EpicGames/UnrealEngine`).
    -   `GEMINI_API_KEY`: The API key obtained from [Google AI Studio](https://aistudio.google.com/app/apikey).

3.  **Configure Notification Targets (At least one is required):**
    Next, choose and configure where you want to receive the reports. You can set up **GitHub Discussions**, **Slack**, **Discord**, or any combination of them.

    #### A) Posting to GitHub Discussions
    Ideal for team discussions and permanent record-keeping.
    1.  **Enable Discussions:** In the target repository's `Settings` > `General` > `Features`, enable `Discussions`.
    2.  **Create a Category:** Create a category in the Discussions tab (e.g., `Announcements`).
    3.  **Add Secrets:** Register the following secrets.
        -   `DISCUSSION_REPO`: The name of the **private** repository to post reports to (e.g., `MyOrg/MyTeamRepo`).
        -   `DISCUSSION_REPO_PAT`: A PAT with permission to write Discussions in `DISCUSSION_REPO`.

    #### B) Posting to Slack
    Suitable for real-time notifications and quick information sharing.
    1.  **Create an Incoming Webhook:** Follow [Slack's documentation](https://slack.com/help/articles/115005265063-Using-Incoming-Webhooks-in-Slack) to issue a webhook URL for your desired channel.
    2.  **Add Secrets:** Register the following secrets.
        -   `SLACK_WEBHOOK_URL`: The Incoming Webhook URL issued above.
        -   `SLACK_CHANNEL`: The name of the Slack channel to post to (e.g., `#ue-updates`).

    #### C) Posting to Discord
    Also suitable for real-time notifications.
    1.  **Create a Webhook:** Follow [Discord's documentation](https://support.discord.com/hc/en-us/articles/228383668-Intro-to-Webhooks) to create a webhook URL for your **desired channel**. In Discord, the URL itself determines the destination channel, so you don't need to specify a channel name separately like with Slack.
    2.  **Add a Secret:** Register the following secret.
        -   `DISCORD_WEBHOOK_URL`: The webhook URL created above.

 **⚠️ Important: Recommendation for Secure Operation**
 The update history of Unreal Engine is confidential information accessible only to authorized accounts under the Epic Games license agreement. To prevent unintentional information leaks, this tool is designed to **stop operating** if at least one notification target is not configured.

**Recommended Setup:**
It is strongly recommended to set `DISCUSSION_REPO` to a **fork of the Unreal Engine source code repository or another private repository where only members with equivalent access rights are present.** This ensures compliance with the license agreement and allows for secure information sharing.

## 🏃‍♀️ How to Run

-   **Automatic Execution:** The workflow runs automatically on the configured schedule (defaults to daily at 23:00 UTC / 8:00 AM JST).
-   **Manual Execution:** You can also run it manually by going to the repository's `Actions` tab, selecting the `Unreal Engine Update Tracker` workflow, and clicking the `Run workflow` button. **Note: Manual execution is restricted to repository administrators.**
    -   **Report Language:** Enter the language for the report (e.g., `English`, `Japanese`). Default: `Japanese`.
    -   **Commit Scan Limit:** Specify the number of recent commits to scan for manual runs (default: last 24 hours).
    -   **Discussion Category:** The name of the Discussion category to post the report to. Default: `Daily Reports`.
    -   **Gemini Model:** The name of the AI model to use for analysis. Default: `gemini-2.5-pro`.
    -   **Slack Webhook URL:** A temporary Slack Webhook URL to use, overriding the secret.
    -   **Slack Channel:** A temporary Slack channel name to use, overriding the secret.
    -   **Discord Webhook URL:** A temporary Discord Webhook URL to use, overriding the secret.

-   **Changing Default Values:**
    You can change the default values for scheduled and manual runs by setting repository **Variables**. Go to `Settings` > `Secrets and variables` > `Actions`, and from the `Variables` tab, set the following:
    -   `REPORT_LANGUAGE`: The default report language (e.g., `English`).
    -   `DISCUSSION_CATEGORY`: The default category for posts (e.g., `Announcements`).
    -   `GEMINI_MODEL`: The default AI model to use (e.g., `gemini-2.5-pro`).
    -   `UE_BRANCH`: The name of the branch to monitor (e.g., `release`). Defaults to `ue5-main`.

## 🎨 Customization

### Changing the Report Format

The output format, including the report's categories, summary style, and overall structure, is determined by the instructions (prompt) given to the AI.

If you want to change the format, such as requesting more detailed reports or emphasizing specific information, you can directly edit the `prompts/report_prompt.md` file at the root of the repository. By modifying this file, you can customize the AI's behavior without touching any Python code.

## 📝 License and Important Notices

**Please read the following carefully before using this tool.**

-   **User Responsibility:** While this tool is carefully designed to comply with the Unreal Engine license agreement, the ultimate operational responsibility lies with the user. Specifically, you must **always specify a private repository with restricted access** as the destination for reports (`DISCUSSION_REPO`). Posting to a public repository could constitute a license violation.

-   **API Keys and Billing:**
    *   This tool uses the Google Gemini API, which may incur costs based on usage.
    *   If you fork and use this repository, **the owner of the forked repository assumes all billing responsibility for their API key.**
    *   To ensure strict compliance with Unreal Engine's terms, **it is strongly recommended to use an API key from a license plan where submitted data is not used for AI training.**

-   **Safety by Design:**
    *   To minimize the risk of license violations, this tool **does not send any Unreal Engine source code or code diffs** when providing information to the AI. The analysis is based solely on commit messages and changed file paths.

-   **Execution Notes:**
    *   This script actually posts to GitHub Discussions according to its configuration. Please be careful during test runs.
    *   Various APIs have usage limits (rate limits).

---

