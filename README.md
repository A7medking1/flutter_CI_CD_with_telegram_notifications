# 🚀 Flutter CI/CD with Telegram Notifications

Automatically build your Flutter app for **Android**, **Web**, and **Windows** using GitHub Actions, and receive the builds directly in Telegram!

![Flutter](https://img.shields.io/badge/Flutter-02569B?style=for-the-badge&logo=flutter&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white)
![Telegram](https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white)

---

## 📋 Table of Contents

- [Features](#-features)
- [Prerequisites](#-prerequisites)
- [Step 1: Create a Telegram Bot](#step-1-create-a-telegram-bot)
- [Step 2: Get Your Chat ID](#step-2-get-your-chat-id)
- [Step 3: Add Secrets to GitHub](#step-3-add-secrets-to-github)
- [Step 4: Add Workflow File](#step-4-add-workflow-file)
- [Step 5: Test the Setup](#step-5-test-the-setup)
- [Customization](#-customization)
- [Troubleshooting](#-troubleshooting)
- [FAQ](#-faq)

---

## ✨ Features

- 📱 **Android APK** - Automatic release build
- 🌐 **Web Build** - Optimized with CanvasKit renderer
- 🪟 **Windows Build** - x64 release executable
- 📬 **Telegram Notifications** - Receive builds instantly
- 📊 **Build Summary** - Status report for all platforms
- ⚡ **Parallel Builds** - Faster build times
- 🔄 **Auto-trigger** - Builds on push to main/develop branches

---

## 📦 Prerequisites

Before you begin, make sure you have:

- ✅ A Flutter project hosted on GitHub
- ✅ A Telegram account
- ✅ Access to your GitHub repository settings

---

## Step 1: Create a Telegram Bot

### 1.1 Open Telegram and Find BotFather

1. Open Telegram app or web version
2. Search for **@BotFather** (it's the official bot for creating bots)
3. Start a chat with BotFather

### 1.2 Create Your Bot

Send the following command to BotFather:

```
/newbot
```

### 1.3 Follow the Instructions

BotFather will ask you for:

1. **Bot Name**: Choose a display name (e.g., "My Flutter CI Bot")
2. **Bot Username**: Must end with "bot" (e.g., "my_flutter_ci_bot")

### 1.4 Save Your Bot Token

After creation, BotFather will send you a message like this:

```
Done! Congratulations on your new bot. You will find it at t.me/my_flutter_ci_bot. 
You can now add a description...

Use this token to access the HTTP API:
1234567890:ABCdefGHIjklMNOpqrsTUVwxyz123456789

Keep your token secure and store it safely...
```

**📝 Copy and save this token!** You'll need it in Step 3.

> ⚠️ **Important**: Never share your bot token publicly! It's like a password.

---

## Step 2: Get Your Chat ID

### 2.1 Start a Chat with Your Bot

1. Find your bot in Telegram (click the link from BotFather)
2. Click **START** or send any message like "Hello"

### 2.2 Get Your Chat ID

Open your browser and visit this URL (replace `YOUR_BOT_TOKEN` with your actual token):

```
https://api.telegram.org/botYOUR_BOT_TOKEN/getUpdates
```

**Example:**
```
https://api.telegram.org/bot1234567890:ABCdefGHIjklMNOpqrsTUVwxyz123456789/getUpdates
```

### 2.3 Find Your Chat ID in the Response

You'll see a JSON response like this:

```json
{
  "ok": true,
  "result": [
    {
      "update_id": 123456789,
      "message": {
        "message_id": 1,
        "from": {
          "id": 987654321,
          "is_bot": false,
          "first_name": "John"
        },
        "chat": {
          "id": 987654321,    👈 THIS IS YOUR CHAT_ID!
          "first_name": "John",
          "type": "private"
        },
        "date": 1234567890,
        "text": "Hello"
      }
    }
  ]
}
```

**📝 Copy the `"id"` number from the `"chat"` section!**

> 💡 **Tip**: If you see `"result": []`, it means the bot hasn't received your message yet. Go back and send a message to your bot first!

### 2.4 For Group Chats (Optional)

If you want to receive builds in a group:

1. Add your bot to the group
2. Make the bot an admin (required for sending files)
3. Send a message in the group
4. Check the getUpdates URL again
5. The chat ID for groups will be a **negative number** (e.g., `-1001234567890`)

---

## Step 3: Add Secrets to GitHub

### 3.1 Navigate to Repository Settings

1. Go to your GitHub repository
2. Click on **Settings** (top menu)
3. In the left sidebar, find **Secrets and variables**
4. Click **Actions**

### 3.2 Add TELEGRAM_BOT_TOKEN

1. Click **"New repository secret"**
2. Fill in the form:
   - **Name**: `TELEGRAM_BOT_TOKEN`
   - **Secret**: Paste your bot token from Step 1.4
3. Click **"Add secret"**

### 3.3 Add TELEGRAM_CHAT_ID

1. Click **"New repository secret"** again
2. Fill in the form:
   - **Name**: `TELEGRAM_CHAT_ID`
   - **Secret**: Paste your chat ID from Step 2.3
3. Click **"Add secret"**

### 3.4 Verify Your Secrets

You should now see two secrets listed:
- ✅ `TELEGRAM_BOT_TOKEN`
- ✅ `TELEGRAM_CHAT_ID`

> ⚠️ **Important**: Secret names must be EXACTLY as shown (all caps with underscores). GitHub won't show you the values after saving (that's normal for security).

---

## Step 4: Add Workflow File

### 4.1 Create the Workflow Directory

In your Flutter project root, create this folder structure:

```
your-flutter-project/
├── .github/
│   └── workflows/
│       └── ci.yml    👈 Create this file
├── lib/
├── android/
├── web/
├── windows/
└── pubspec.yaml
```

### 4.2 Create the Workflow File

1. Create a folder named `.github` in your project root
2. Inside `.github`, create a folder named `workflows`
3. Inside `workflows`, create a file named `ci.yml`

### 4.3 Copy the Workflow Configuration

Copy the content from the `ci.yml` artifact above into your new file,

### 4.4 Commit and Push

```bash
# Add the workflow file
git add .github/workflows/ci.yml

# Commit with a message
git commit -m "Add CI/CD pipeline with Telegram notifications"

# Push to GitHub
git push origin main
```

---

## Step 5: Test the Setup

### 5.1 Trigger the Workflow

The workflow will automatically run when you:
- Push to the `main` branch
- Push to the `develop` branch
- Create a pull request to `main`

Or you can manually trigger it:
1. Go to your GitHub repository
2. Click **Actions** tab
3. Select **Flutter CI/CD Pipeline**
4. Click **Run workflow**
5. Select the branch and click **Run workflow**

### 5.2 Monitor the Build

1. In the **Actions** tab, you'll see your workflow running
2. Click on the workflow run to see details
3. You can expand each job to see the logs
4. Watch for the green checkmarks ✅

### 5.3 Receive Builds in Telegram

Within 5-10 minutes, you should receive:

1. **📱 Android APK** - Ready to install on Android devices
2. **🌐 Web Build ZIP** - Extract and host on any web server
3. **🪟 Windows Build ZIP** - Extract and run the .exe file
4. **📊 Summary Message** - Overview of all build statuses

---



### Q: Can I customize the message format?

**A:** Yes! Edit the caption in the curl command:

```yaml
-F caption="🚀 New Build!%0A📱 Platform: Android%0A🏷️ Version: 1.0.0%0A📅 Date: $(date)"
```

Note: `%0A` is URL-encoded newline.

### Q: How do I revoke/change my bot token?

**A:**
1. Message @BotFather in Telegram
2. Send `/mybots`
3. Select your bot
4. Click "API Token"
5. Click "Revoke current token"
6. Get the new token
7. Update the secret in GitHub

### Q: Can I schedule builds?

**A:** Yes! Add a schedule trigger:

```yaml
on:
  schedule:
    - cron: '0 0 * * *'  # Daily at midnight UTC
  push:
    branches: [ main ]
```

---

## 📚 Additional Resources

- [Flutter Documentation](https://docs.flutter.dev/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Telegram Bot API](https://core.telegram.org/bots/api)
- [Flutter CI/CD Best Practices](https://docs.flutter.dev/deployment/cd)

---
