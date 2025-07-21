# Unreal Engine Update Tracker

[Read this in Japanese](README.md)

This project is an automated service that periodically monitors updates to Unreal Engine's private GitHub repository, summarizes important changes (such as new features and specification changes) using AI (Google Gemini), and posts them as reports to GitHub Discussions.

<table><tr><td>
<img width="644" alt="image" src="https://raw.githubusercontent.com/pafuhana1213/Screenshot/master/468407129-ad69f54f-9e18-49db-8024-aa3052d97ffa.png" />
</td></tr></table>

Note: This image is an example of a report, and the content shown is entirely dummy data. It does not represent actual updates made to Unreal Engine.

## 🌟 Key Features

-   **Automatic Update Checks:** Uses GitHub Actions to check for the latest commits in the UE repository on a schedule (daily at 23:00 UTC / 8:00 AM JST) or manually.
-   **AI-Powered Summaries:** The Gemini API analyzes commit contents, categorizes them into sections like "New Features" and "Specification Changes," and provides a summary for each.
-   **Posting to Discussions:** The generated report is posted to the repository's GitHub Discussions as "Unreal Engine Daily Report."

## 🛠️ Setup Instructions

1.  **Fork this repository:**
    Click the **Fork** button in the top-right corner to copy this repository to your own GitHub account. This will enable the GitHub Actions workflow to run under your account.

2.  **Enable GitHub Discussions:**
    Go to your repository's `Settings` > `General` > `Features` and check the `Discussions` box to enable it.

3.  **Create a Discussion Category:**
    In the Discussions tab, create a category for posting reports (e.g., `Announcements` or `Daily Reports`).

4.  **Configure Repository Secrets:**
    Go to your repository's `Settings` > `Secrets and variables` > `Actions` and register the following repository secrets:
    -   `UE_REPO_PAT`: Register a [Personal Access Token (PAT)](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) with read access to the private Unreal Engine repository (`EpicGames/UnrealEngine`).
    -   `GEMINI_API_KEY`: Register the API key obtained from [Google AI Studio](https://aistudio.google.com/app/apikey).
    -   `DISCUSSION_REPO`: The name of the **private** repository where reports will be posted (e.g., `MyOrg/MyTeamRepo`).
    -   `DISCUSSION_REPO_PAT`: A PAT with permissions to write Discussions in the repository specified in `DISCUSSION_REPO`.
  
 **⚠️ Important: Recommendation for Secure Operation**  
 The update history of Unreal Engine is confidential information accessible only to authorized accounts under the Epic Games license agreement. To prevent unintentional information leaks, this tool is designed to **stop operating** if `DISCUSSION_REPO` and `DISCUSSION_REPO_PAT` are not set.

**Recommended Setup:**
It is strongly recommended to set `DISCUSSION_REPO` to a **fork of the Unreal Engine source code repository or another private repository where only members with equivalent access rights are present.** This ensures compliance with the license agreement and allows for secure information sharing.

## 🏃‍♀️ How to Run

-   **Automatic Execution:** The workflow runs automatically on the configured schedule (defaults to daily at 23:00 UTC / 8:00 AM JST).
-   **Manual Execution:** You can also run it manually by going to the repository's `Actions` tab, selecting the `Unreal Engine Update Tracker` workflow, and clicking the `Run workflow` button. **Note: Manual execution is restricted to repository administrators.**
    -   **Report Language:** Enter the language for the report (e.g., `English`, `Japanese`). Default: `Japanese`.
    -   **Commit Scan Limit:** Specify the number of recent commits to scan for manual runs (default: last 24 hours).
    -   **Discussion Category:** The name of the Discussion category to post the report to. Default: `Daily Reports`.
    -   **Gemini Model:** The name of the AI model to use for analysis. Default: `gemini-2.5-pro`.

-   **Changing Default Values:**
    You can change the default values for scheduled and manual runs by setting repository **Variables**. Go to `Settings` > `Secrets and variables` > `Actions`, and from the `Variables` tab, set the following:
    -   `REPORT_LANGUAGE`: The default report language (e.g., `English`).
    -   `DISCUSSION_CATEGORY`: The default category for posts (e.g., `Announcements`).
    -   `GEMINI_MODEL`: The default AI model to use (e.g., `gemini-2.5-pro`).

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

## ✨ Please Consider Supporting!

I hope this tool is helping you with your daily UE catch-up.

This tool is developed and maintained by a single person, covering costs like coffee and API fees out of pocket as a passion project. ☕
If you find this tool useful, please consider supporting its development through GitHub Sponsors. Your support would be a great encouragement and a huge motivation to keep this project going!

[💖 **Support the developer on GitHub Sponsors**](https://github.com/sponsors/pafuhana1213)