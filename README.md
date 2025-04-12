# DIGITAL-MARKETING-NEWSLETTER
const express = require('express');
const cors = require('cors');
const bodyParser = require('body-parser');
const mailchimp = require('@mailchimp/mailchimp_marketing');
require('dotenv').config();

const app = express();
const PORT = process.env.PORT || 5000;

app.use(cors());
app.use(bodyParser.json());

mailchimp.setConfig({
  apiKey: process.env.MAILCHIMP_API_KEY,
  server: process.env.MAILCHIMP_SERVER_PREFIX, // e.g., 'us21'
});

app.post('/api/subscribe', async (req, res) => {
  const { email } = req.body;

  if (!email) {
    return res.status(400).json({ error: 'Email is required' });
  }

  try {
    const response = await mailchimp.lists.addListMember(process.env.MAILCHIMP_AUDIENCE_ID, {
      email_address: email,
      status: 'subscribed',
    });
    return res.status(200).json({ message: 'Subscribed successfully', data: response });
  } catch (error) {
    return res.status(500).json({ error: error.response?.body?.detail || 'Something went wrong' });
  }
});

app.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`);
});

---

### 🚀 Now Push to GitHub
1. Open terminal:
```bash
cd path/to/newsletter-backend
git init
git remote add origin https://github.com/yourusername/newsletter-backend.git
git add .
git commit -m "Initial commit"
git push -u origin master

