# Google APIs Integration

Setup guide for adding Google Calendar and Gmail to any agent.

## Overview

This integration allows agents to:
- Read and create calendar events
- Read and send emails
- Support daily briefings and weekly schedule reviews

## Reference Implementation

**MILA** in `demo-repository` has the complete implementation:
- `MILA-VERCEL-SETUP.md` - Full deployment guide
- `api/mila-mcp-server.js` - MCP server with Google APIs
- `api/mila-task.js` - Task handling

## Quick Setup

### 1. Google Cloud Setup
- Create project at [console.cloud.google.com](https://console.cloud.google.com)
- Enable: Google Calendar API, Gmail API
- Create OAuth 2.0 credentials (Web application)

### 2. Environment Variables
```
GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=
GOOGLE_REFRESH_TOKEN=
GOOGLE_REDIRECT_URI=
GOOGLE_CALENDAR_ID=primary
GMAIL_USER=
```

### 3. Required Scopes
```
https://www.googleapis.com/auth/calendar
https://www.googleapis.com/auth/calendar.events
https://www.googleapis.com/auth/gmail.readonly
https://www.googleapis.com/auth/gmail.send
```

### 4. Dependencies
```bash
npm install googleapis
```

## Agent-Specific Guides

- [Samantha Setup](../../agents/samantha/google-api-setup.md)

## API Patterns

### Calendar Read
```javascript
const events = await calendar.events.list({
  calendarId: 'primary',
  timeMin: new Date().toISOString(),
  maxResults: 10,
  singleEvents: true,
  orderBy: 'startTime',
});
```

### Calendar Write
```javascript
const event = await calendar.events.insert({
  calendarId: 'primary',
  resource: {
    summary: 'Meeting',
    start: { dateTime: '2026-04-20T10:00:00' },
    end: { dateTime: '2026-04-20T11:00:00' },
  },
});
```

### Gmail Read
```javascript
const messages = await gmail.users.messages.list({
  userId: 'me',
  maxResults: 10,
});
```

### Gmail Send
```javascript
const message = Buffer.from([
  `To: recipient@example.com`,
  `Subject: Hello`,
  '',
  'Message body'
].join('\n')).toString('base64');

await gmail.users.messages.send({
  userId: 'me',
  resource: { raw: message },
});
```
