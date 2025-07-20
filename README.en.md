# Unreal Engine Update Tracker

[Read this in Japanese](README.md)

This project is an automated service that periodically monitors updates to Unreal Engine's private GitHub repository, summarizes important changes (such as new features and specification changes) using AI (Google Gemini), and posts them as reports to GitHub Discussions.

<table><tr><td>
<img width="644" alt="image" src="https://github.com/user-attachments/assets/ad69f54f-9e18-49db-8024-aa3052d97ffa" />
</td></tr></table>

Note: This image is an example of a report, and the content shown is entirely dummy data. It does not represent actual updates made to Unreal Engine. Although the example is in Japanese, reports can also be generated in English by adjusting the prompt in the workflow file.

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
    -   **Discussion Category:** You can specify the target discussion category name at runtime (default: `Daily Reports`).
    -   **Gemini Model:** You can temporarily specify the Gemini model name to use for analysis.

-   **Changing the Default Model:** If you want to change the default model used for scheduled runs, you only need to edit one line in the `.github/workflows/main.yml` file.
    ```yaml
    # .github/workflows/main.yml

    jobs:
      run-update-check:
        runs-on: ubuntu-latest
        env:
          DEFAULT_GEMINI_MODEL: 'gemini-2.5-pro' # <-- Change the default model here
    ```

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