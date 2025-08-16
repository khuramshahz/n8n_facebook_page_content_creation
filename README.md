# 🤖 N8N Facebook Automation Bot

[![N8N](https://img.shields.io/badge/N8N-Automation-ff6d6d?style=for-the-badge&logo=n8n&logoColor=white)](https://n8n.io/)
[![Facebook](https://img.shields.io/badge/Facebook-API-1877f2?style=for-the-badge&logo=facebook&logoColor=white)](https://developers.facebook.com/)
[![OpenRouter](https://img.shields.io/badge/OpenRouter-AI-4285f4?style=for-the-badge)](https://openrouter.ai/)
[![Google Sheets](https://img.shields.io/badge/Google-Sheets-34a853?style=for-the-badge&logo=google-sheets&logoColor=white)](https://sheets.google.com/)

> **Automate your Facebook page with AI-generated content, scheduled daily posts, and intelligent topic management.**

## ✨ Features

- 🕐 **Automated Daily Posting** - Schedule posts at 2 AM daily using cron expressions
- 🧠 **AI-Powered Content Generation** - Leverages GPT-3.5-turbo for engaging post creation
- 📊 **Google Sheets Integration** - Manages topics and tracks posting status
- 📱 **Facebook Graph API** - Direct posting to your Facebook page
- 🔄 **Smart Topic Management** - Automatically marks topics as posted
- ⚡ **Zero Manual Intervention** - Fully automated workflow

## 🏗️ Architecture

```mermaid
graph LR
    A[Schedule Trigger] --> B[Get Unposted Topic]
    B --> C[Generate AI Content]
    C --> D[Extract Content]
    D --> E[Post to Facebook]
    B --> F[Mark as Posted]
```

## 🚀 Workflow Components

| Node | Purpose | Technology |
|------|---------|------------|
| **Schedule Trigger** | Triggers workflow daily at 2 AM | N8N Cron |
| **Get row(s) in sheet** | Fetches unposted topics from Google Sheets | Google Sheets API |
| **HTTP Request1** | Generates AI content using OpenRouter | OpenRouter API + GPT-3.5 |
| **Edit Fields1** | Extracts AI-generated text from response | N8N Data Processing |
| **HTTP Request** | Posts content to Facebook page | Facebook Graph API |
| **Update row in sheet** | Marks topic as posted in spreadsheet | Google Sheets API |

## 📋 Prerequisites

### Required APIs & Services
- **N8N Instance** (self-hosted or cloud)
- **Facebook Developer Account** with Graph API access
- **OpenRouter Account** with API key
- **Google Account** with Sheets API enabled

### Required Credentials
1. **Facebook Page Access Token**
2. **OpenRouter API Key** 
3. **Google Sheets OAuth2 Credentials**

## ⚙️ Setup Instructions

### 1. Clone & Import Workflow
```bash
# Download the workflow JSON
curl -O https://your-repo/DEMO.json

# Import into N8N dashboard
# Go to N8N → Workflows → Import from File → Select DEMO.json
```

### 2. Configure Google Sheets
Create a spreadsheet with the following columns:
| Topic | Posted | IsPosted | row_number |
|-------|--------|----------|------------|
| AI in Healthcare | | | 1 |
| Social Media Trends | | | 2 |

### 3. Set Up API Credentials

#### Facebook Graph API
```javascript
// Replace in HTTP Request node
url: "https://graph.facebook.com/v23.0/YOUR_PAGE_ID/feed"
access_token: "YOUR_FACEBOOK_ACCESS_TOKEN"
```

#### OpenRouter API
```javascript
// Replace in HTTP Request1 node
Authorization: "Bearer YOUR_OPENROUTER_API_KEY"
```

#### Google Sheets
```javascript
// Update document ID in Google Sheets nodes
documentId: "YOUR_GOOGLE_SHEETS_ID"
```

### 4. Customize Content Prompt
Modify the AI prompt in **HTTP Request1** node:
```json
{
  "model": "gpt-3.5-turbo",
  "messages": [
    {
      "role": "user",
      "content": "Write a facebook page post on the topic {{$json[\"Topic\"]}} that attract the people toward it. The page name is YOUR_PAGE_NAME that write daily one post. Do not write the first and last line that you write."
    }
  ]
}
```

## 🛠️ Configuration Options

### Schedule Customization
```javascript
// Modify cron expression in Schedule Trigger
"0 2 * * *"  // 2 AM daily (default)
"0 9 * * 1"  // 9 AM every Monday
"0 12 * * *" // 12 PM daily
```

### Content Filtering
Add filters to Google Sheets node:
```javascript
"filtersUI": {
  "values": [
    {
      "lookupColumn": "Posted",
      "lookupValue": "\"\""
    },
    {
      "lookupColumn": "IsActive", 
      "lookupValue": "true"
    }
  ]
}
```

## 📊 Monitoring & Analytics

### Workflow Status
- ✅ **Active**: Workflow runs automatically
- 📈 **Executions**: View in N8N execution history
- 🔍 **Logs**: Debug issues through N8N interface

### Data Tracking
- **Google Sheets**: Track posted topics and dates
- **Facebook Insights**: Monitor post performance
- **N8N Analytics**: Workflow success rates

## 🚨 Troubleshooting

### Common Issues

#### Facebook API Errors
```javascript
// Check access token validity
// Verify page permissions
// Ensure page ID is correct
```

#### Google Sheets Connection
```javascript
// Verify OAuth2 credentials
// Check sheet sharing permissions
// Validate document URL format
```

#### AI Content Generation
```javascript
// Monitor OpenRouter API limits
// Check prompt formatting
// Verify JSON structure
```

### Debug Steps
1. **Manual Execution**: Test workflow manually in N8N
2. **Node Inspection**: Check data flow between nodes
3. **API Testing**: Test endpoints independently
4. **Credential Validation**: Refresh authentication tokens

## 🔒 Security Best Practices

- 🔐 **Environment Variables**: Store API keys securely
- 🔄 **Token Rotation**: Regularly refresh access tokens
- 📝 **Access Logs**: Monitor API usage
- 🛡️ **Permission Scope**: Limit API permissions to minimum required

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📧 Support

- **Issues**: [GitHub Issues](https://github.com/your-repo/issues)
- **Documentation**: [N8N Docs](https://docs.n8n.io/)
- **Community**: [N8N Community](https://community.n8n.io/)

---

<div align="center">

**Made with ❤️ using N8N Automation**

[⭐ Star this repo](https://github.com/your-repo) • [🐛 Report Bug](https://github.com/your-repo/issues) • [✨ Request Feature](https://github.com/your-repo/issues)

</div>
