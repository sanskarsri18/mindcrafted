# Mindcrafted Insights Clinic Website

A modern, responsive website for Mindcrafted Insights Clinic, a psychology clinic offering professional counseling services.

## Features

- Responsive design for mobile, tablet, and desktop
- Sticky navigation with mobile hamburger menu
- Smooth scrolling navigation
- Appointment booking form with validation
- Testimonial slider
- WhatsApp chat integration
- SEO optimized

## Pages

- Home (index.html)
- About (about.html)
- Services (services.html)
- Appointment (appointment.html)
- Contact (contact.html)

## Technologies Used

- HTML5
- CSS3 (Flexbox/Grid)
- Vanilla JavaScript

## Local Development

1. Clone the repository
2. Open index.html in your browser or use a local server

For Python server:

```bash
python -m http.server 8000
```

## Deployment

### Option 1: Surge (Recommended for quick deployment)

1. Install Surge: `npm install -g surge`
2. In the project directory: `surge`
3. Follow the prompts to login and deploy
4. Your site will be live at a .surge.sh domain

### Option 2: GitHub Pages

1. Create a new repository on GitHub
2. Push this code to the repository
3. Go to Settings > Pages
4. Select "Deploy from a branch" and choose main/master branch
5. The site will be live at `https://yourusername.github.io/repository-name/`

### Option 3: Vercel

1. Install Vercel CLI: `npm install -g vercel`
2. In the project directory: `vercel`
3. Follow the prompts to login and deploy
4. Your site will be live on Vercel

## Contact

For any questions about the clinic, visit the contact page or use the WhatsApp chat button.

## Optional: Store Bookings + Send Email via Google Sheets

This site can save appointment requests to a Google Sheet and send you an email notification using a Google Apps Script web endpoint.

### 1) Deploy a Google Apps Script Web App

1. Create a new Google Sheet.
2. In the sheet, go to **Extensions → Apps Script**.
3. Replace the default `Code.gs` content with the script below.

```javascript
function doPost(e) {
  try {
    const data = JSON.parse(e.postData.contents);
    const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    sheet.appendRow([
      new Date(),
      data.name,
      data.email,
      data.phone,
      data.service,
      data.date,
      data.time,
      data.message,
    ]);

    MailApp.sendEmail({
      to: "your-email@example.com",
      subject: `New appointment request: ${data.service}`,
      body: `New booking request received:\n\nName: ${data.name}\nEmail: ${data.email}\nPhone: ${data.phone}\nService: ${data.service}\nDate: ${data.date}\nTime: ${data.time}\nMessage: ${data.message}`,
    });

    return ContentService.createTextOutput(
      JSON.stringify({ success: true }),
    ).setMimeType(ContentService.MimeType.JSON);
  } catch (error) {
    return ContentService.createTextOutput(
      JSON.stringify({ success: false, error: error.message }),
    ).setMimeType(ContentService.MimeType.JSON);
  }
}
```

4. Deploy the script as a web app: **Deploy → New deployment → Web app**.
   - Execute as: **Me**
   - Who has access: **Anyone**
   - Copy the web app URL.

### 2) Configure the Website

In `index.html` (and/or `appointment.html`) set `window.BOOKING_ENDPOINT_URL` to your web app URL:

```html
<script>
  window.BOOKING_ENDPOINT_URL =
    "https://script.google.com/macros/s/YOUR_SCRIPT_ID/exec";
</script>
<script src="js/script.js"></script>
```

Once configured, the booking form will:

- Save requests to the Google Sheet
- Send you an email notification
- Still offer the “Add to Google Calendar” link
