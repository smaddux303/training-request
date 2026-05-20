# WFD Training Request App

Westminster Fire Department Training & Travel Expense Authorization Form — PWA

## Files

- `index.html` — the form application
- `manifest.json` — PWA manifest (makes it installable on phones)
- `sw.js` — service worker (enables offline use and caching)
- `icons/` — app icons (see below)

## Setup steps

### 1. Add app icons

Create two PNG icons and place them in the `icons/` folder:

- `icons/icon-192.png` — 192×192 px
- `icons/icon-512.png` — 512×512 px

Use the WFD logo or a flame icon on a navy (#1a3a6b) background.
Free tool: https://realfavicongenerator.net

### 2. Connect Power Automate

In `index.html`, find this line near the bottom of the `<script>` block:

```
const POWER_AUTOMATE_URL = 'YOUR_POWER_AUTOMATE_HTTP_TRIGGER_URL_HERE';
```

Replace with your actual Power Automate HTTP trigger URL.

**Power Automate flow setup:**
1. Create a new flow: "When an HTTP request is received"
2. Set method to POST
3. Use this JSON schema for the body:

```json
{
  "type": "object",
  "properties": {
    "dateSubmitted": { "type": "string" },
    "name": { "type": "string" },
    "rankTitle": { "type": "string" },
    "shift": { "type": "string" },
    "email": { "type": "string" },
    "className": { "type": "string" },
    "vendorName": { "type": "string" },
    "classAddress": { "type": "string" },
    "classLocation": { "type": "string" },
    "classStartDate": { "type": "string" },
    "travelToDate": { "type": "string" },
    "classEndDate": { "type": "string" },
    "returnDate": { "type": "string" },
    "justification": { "type": "string" },
    "overtime": { "type": "string" },
    "registration": { "type": "string" },
    "registrationFee": { "type": "string" },
    "transportation": { "type": "string" },
    "transportationCost": { "type": "string" },
    "parkingCost": { "type": "string" },
    "mileageAmount": { "type": "string" },
    "airfareCost": { "type": "string" },
    "airline": { "type": "string" },
    "lodgingCost": { "type": "string" },
    "lodgingAddress": { "type": "string" },
    "mealCost": { "type": "string" },
    "otherCosts": { "type": "string" },
    "attachments": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "name": { "type": "string" },
          "type": { "type": "string" },
          "size": { "type": "number" },
          "data": { "type": "string" }
        }
      }
    }
  }
}
```

File attachments arrive as base64-encoded strings in the `attachments` array.
Use a "Apply to each" loop in Power Automate to save each one to SharePoint.

### 3. Deploy to GitHub Pages

1. Push all files to your GitHub repository
2. Go to Settings → Pages
3. Set source to your main branch, root folder
4. Your app will be live at: `https://yourusername.github.io/repo-name`

**Important:** If deploying to a subfolder (not root), update `start_url` in `manifest.json`:
```json
"start_url": "/your-repo-name/"
```

### 4. Share with staff

Send staff the GitHub Pages URL. On mobile:
- **iPhone**: Open in Safari → Share → "Add to Home Screen"
- **Android**: Open in Chrome → menu → "Add to Home Screen" or "Install app"

They'll get a proper app icon on their home screen with no app store required.

## Updating the form

Edit `index.html` directly and push to GitHub. Changes go live automatically within a minute.
