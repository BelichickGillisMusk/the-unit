# Samantha - Google Calendar & Gmail Setup

Add Google Calendar and Gmail access to Samantha following Mila's pattern.

## Prerequisites

1. Google Cloud Console project
2. Vercel project for Samantha

## Step 1: Enable Google APIs

1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Create or select your project
3. Enable these APIs:
   - Google Calendar API
   - Gmail API

## Step 2: Create OAuth Credentials

1. Go to **APIs & Services > Credentials**
2. Click **Create Credentials > OAuth 2.0 Client ID**
3. Application type: **Web application**
4. Add authorized redirect URI: `https://your-samantha-app.vercel.app/api/auth/callback`
5. Save your **Client ID** and **Client Secret**

## Step 3: Get Refresh Token

Use the OAuth 2.0 Playground or a local script to get a refresh token:

1. Go to [OAuth 2.0 Playground](https://developers.google.com/oauthplayground/)
2. Click gear icon > Use your own OAuth credentials
3. Enter your Client ID and Client Secret
4. Select scopes:
   - `https://www.googleapis.com/auth/calendar`
   - `https://www.googleapis.com/auth/calendar.events`
   - `https://www.googleapis.com/auth/gmail.readonly`
   - `https://www.googleapis.com/auth/gmail.send`
5. Authorize and exchange for tokens
6. Copy the **Refresh Token**

## Step 4: Configure Vercel Environment Variables

Add these to your Samantha Vercel project:

```
GOOGLE_CLIENT_ID=your_client_id_here
GOOGLE_CLIENT_SECRET=your_client_secret_here
GOOGLE_REFRESH_TOKEN=your_refresh_token_here
GOOGLE_REDIRECT_URI=https://your-samantha-app.vercel.app/api/auth/callback
GOOGLE_CALENDAR_ID=primary
GMAIL_USER=your_email@gmail.com
API_SECRET=your_strong_random_secret_here
```

Generate `API_SECRET` with: `openssl rand -base64 32`

All API requests must include `Authorization: Bearer <API_SECRET>` header.

## Step 5: Add API Endpoints

Copy these from Mila's implementation in `demo-repository/api/`:

### Calendar Endpoint (`api/calendar.js`)
```javascript
import crypto from 'crypto';
import { google } from 'googleapis';

const oauth2Client = new google.auth.OAuth2(
  process.env.GOOGLE_CLIENT_ID,
  process.env.GOOGLE_CLIENT_SECRET,
  process.env.GOOGLE_REDIRECT_URI
);

oauth2Client.setCredentials({
  refresh_token: process.env.GOOGLE_REFRESH_TOKEN
});

const calendar = google.calendar({ version: 'v3', auth: oauth2Client });

function authenticate(req, res) {
  const token = req.headers.authorization;
  const expected = `Bearer ${process.env.API_SECRET}`;
  if (
    !token ||
    Buffer.byteLength(token) !== Buffer.byteLength(expected) ||
    !crypto.timingSafeEqual(Buffer.from(token), Buffer.from(expected))
  ) {
    res.status(401).json({ error: 'Unauthorized' });
    return false;
  }
  return true;
}

export default async function handler(req, res) {
  if (!authenticate(req, res)) return;

  if (req.method === 'GET') {
    const events = await calendar.events.list({
      calendarId: process.env.GOOGLE_CALENDAR_ID || 'primary',
      timeMin: new Date().toISOString(),
      maxResults: 10,
      singleEvents: true,
      orderBy: 'startTime',
    });
    return res.json(events.data.items);
  }
  
  if (req.method === 'POST') {
    const event = await calendar.events.insert({
      calendarId: process.env.GOOGLE_CALENDAR_ID || 'primary',
      resource: req.body,
    });
    return res.json(event.data);
  }

  return res.status(405).json({ error: 'Method not allowed' });
}
```

### Gmail Endpoint (`api/gmail.js`)
```javascript
import crypto from 'crypto';
import { google } from 'googleapis';

const oauth2Client = new google.auth.OAuth2(
  process.env.GOOGLE_CLIENT_ID,
  process.env.GOOGLE_CLIENT_SECRET,
  process.env.GOOGLE_REDIRECT_URI
);

oauth2Client.setCredentials({
  refresh_token: process.env.GOOGLE_REFRESH_TOKEN
});

const gmail = google.gmail({ version: 'v1', auth: oauth2Client });

function authenticate(req, res) {
  const token = req.headers.authorization;
  const expected = `Bearer ${process.env.API_SECRET}`;
  if (
    !token ||
    Buffer.byteLength(token) !== Buffer.byteLength(expected) ||
    !crypto.timingSafeEqual(Buffer.from(token), Buffer.from(expected))
  ) {
    res.status(401).json({ error: 'Unauthorized' });
    return false;
  }
  return true;
}

function sanitizeHeader(value) {
  return String(value).replace(/[\r\n]/g, '');
}

export default async function handler(req, res) {
  if (!authenticate(req, res)) return;

  if (req.method === 'GET') {
    const messages = await gmail.users.messages.list({
      userId: 'me',
      maxResults: 10,
    });
    return res.json(messages.data);
  }
  
  if (req.method === 'POST') {
    const { to, subject, body } = req.body;
    if (!to || !subject || !body) {
      return res.status(400).json({ error: 'Missing required fields: to, subject, body' });
    }

    const message = [
      `To: ${sanitizeHeader(to)}`,
      `Subject: ${sanitizeHeader(subject)}`,
      '',
      body
    ].join('\n');
    
    const encodedMessage = Buffer.from(message)
      .toString('base64')
      .replace(/\+/g, '-')
      .replace(/\//g, '_');
    
    const sent = await gmail.users.messages.send({
      userId: 'me',
      resource: { raw: encodedMessage },
    });
    return res.json(sent.data);
  }

  return res.status(405).json({ error: 'Method not allowed' });
}
```

## Step 6: Install Dependencies

```bash
npm install googleapis
```

## Step 7: Deploy & Test

1. Deploy to Vercel: `vercel --prod`
2. Test calendar: `curl -H "Authorization: Bearer $API_SECRET" https://your-app.vercel.app/api/calendar`
3. Test gmail: `curl -H "Authorization: Bearer $API_SECRET" https://your-app.vercel.app/api/gmail`
4. Verify unauthorized access is blocked: `curl https://your-app.vercel.app/api/calendar` (should return 401)

## Reference

Full implementation: `BelichickGillisMusk/demo-repository`
- `MILA-VERCEL-SETUP.md` - Complete setup guide
- `api/mila-mcp-server.js` - MCP server integration
- `api/mila-task.js` - Task handling

## Daily Usage

Once configured, Samantha can:
- Read your calendar events for weekly schedule reviews
- Create calendar events for appointments
- Read incoming emails
- Send emails on your behalf
