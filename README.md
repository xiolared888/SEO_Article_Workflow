# Article Outlines Workflow

AI-powered automation that transforms content ideas into complete, SEO-optimized articles in minutes. One click generates structured outlines, 1,200-word articles, and three branded images—fully automated.

## Overview

This n8n workflow eliminates manual content creation. Feed it a topic, and it produces:

- **SEO Outline** — Meta tags, keywords, section structure, FAQs, APA references
- **1,200-word Article** — Narrative, reflective, conversational tone
- **Three DALL-E 3 Images** — Professional hero, detailed illustration, conceptual design
- **Auto-updated Status** — Marks content as published in your queue

**Runtime:** 2-3 minutes per article  
**Cost:** ~$0.15-0.25 per article (OpenAI + DALL-E)

## Quick Start

### Prerequisites

- **n8n** (cloud or self-hosted)
- **Google Account** with:
  - Google Sheets (content queue)
  - Google Docs (outline storage, article output)
  - Google Drive (image storage)
- **OpenAI API Key** (GPT-5-mini or compatible model)
- **DALL-E 3 Access** via OpenAI

### Setup

1. **Create a content queue in Google Sheets:**
   - Columns: `Title`, `Summary`, `Status`
   - Add rows with Status = "Idea"

2. **Set up storage in Google Docs & Drive:**
   - Create `Outlines` document for tracking
   - Create `Articles` folder in Google Drive for images

3. **Import the workflow into n8n:**
   - Copy the workflow JSON from `/workflow/article-outlines.json`
   - Paste into n8n
   - Connect your Google and OpenAI credentials

4. **Test the trigger:**
   - Add a test row to your Sheets queue
   - Click "Execute Workflow" in n8n
   - Check your Google Docs and Drive for outputs

## Workflow Architecture

```
Trigger (Manual or Scheduled)
  ↓
Pull Idea from Google Sheets
  ↓
Generate SEO Outline (GPT-5-mini)
  ↓
Clean & Store Outline in Google Docs
  ↓
Write Full Article (GPT-5-mini)
  ↓
Clean & Create New Google Doc
  ↓
Generate Three Image Prompts
  ↓
Create Images with DALL-E 3
  ↓
Upload Images to Google Drive
  ↓
Insert Image Links into Article Doc
  ↓
Update Row Status to "Published"
```

## System Prompts

All system prompts are customizable. The defaults enforce:

**Outline Generator:**
- 60-character meta title, 155-character meta description
- 8-15 SEO keywords + 5-10 long-tail phrases
- Section-by-section structure with word count targets
- 4-7 FAQs + 3-6 APA references
- Flesch Reading Ease 60-70

**Article Writer:**
- Narrative, reflective introduction
- 1,200-word body with clear section headers
- Energetic, intellectually engaging tone
- No em dashes, no hyphens
- FAQ section + APA references
- Flesch score 60-70

**Image Generation:**
- Image 1: Professional hero image — high quality, modern, clean composition
- Image 2: Detailed illustration — vibrant colors, engaging visual
- Image 3: Conceptual image — abstract, thought-provoking design
- All 1024×1024, vivid style

## Customization

### Change the Model
Replace GPT-5-mini with Claude, GPT-4o, or any OpenAI-compatible model. Update the model name in both AI Agent nodes.

### Adjust Tone & Voice
Edit system prompts in the AI Agent nodes. Examples:
- Formal academic tone
- Technical documentation style
- Marketing/sales messaging
- Casual blog voice

### Modify Output Format
- Change word count targets in the outline prompt
- Add/remove FAQ sections
- Adjust image dimensions (currently 1024×1024)
- Change image style from "vivid" to "natural"

### Trigger Options
- **Manual:** Click "Execute Workflow" (current)
- **Scheduled:** Add a Schedule node for daily runs
- **Row Watch:** Trigger automatically when new rows are added to Sheets

### Add an Approval Step
Insert an Approval node before the "Update Status to Published" step. Requires manual approval before content goes live.

## Integrations

| Service | Role | Why |
|---------|------|-----|
| **Google Sheets** | Content queue & tracking | Central hub for ideas and status |
| **OpenAI (GPT-5-mini)** | Outline + article generation | Fast, reliable, customizable tone |
| **DALL-E 3** | Image generation | High-quality, consistent style |
| **Google Docs** | Outline storage & article output | Easy sharing, collaborative editing |
| **Google Drive** | Image file storage | Organized, accessible, integrated |

## Cost Breakdown (Per Article)

| Task | Cost |
|------|------|
| GPT-5-mini (outline) | ~$0.02 |
| GPT-5-mini (article) | ~$0.08 |
| DALL-E 3 (3 images) | ~$0.30 |
| **Total** | **~$0.40** |

*Costs vary by model and length. GPT-4o articles cost slightly more.*

## File Structure

```
article-outlines-workflow/
├── README.md                          (this file)
├── workflow/
│   └── article-outlines.json          (n8n workflow export)
├── docs/
│   ├── SETUP.md                       (detailed setup guide)
│   ├── CUSTOMIZATION.md               (prompts & configuration)
│   ├── TROUBLESHOOTING.md             (common issues)
│   └── EXAMPLES.md                    (sample outputs)
├── prompts/
│   ├── outline-system-prompt.txt      (outline generator)
│   ├── article-system-prompt.txt      (article writer)
│   └── image-prompts.txt              (image generation)
└── templates/
    ├── google-sheets-template.csv     (content queue template)
    └── presentation.pptx              (24-slide walkthrough)
```

## Next Steps

**Go Further:**

- **Schedule Trigger** — Run daily at a specific time without manual clicking
- **CMS Integration** — Push articles directly to Medium, WordPress, Substack, or custom CMS via HTTP Request nodes
- **Notifications** — Send Slack or email alerts when articles are ready
- **Approval Gate** — Add a manual approval step before publishing
- **Analytics** — Track publication dates, word counts, and image stats in Sheets
- **Multi-language** — Translate outlines and articles to Spanish, French, Chinese, etc.
- **A/B Testing** — Generate multiple article versions with different tones, compare performance

## Troubleshooting

**Article doesn't generate?**
- Check OpenAI API quota and balance
- Verify API key is active
- Check that Sheets row has Title and Summary fields

**Images fail to upload?**
- Verify Google Drive folder exists and permissions are correct
- Check DALL-E 3 quota in OpenAI account
- Ensure folder name matches exactly in workflow

**Text overflows in Google Docs?**
- Reduce image count or place them at end of doc
- Split article into multiple sections
- Use "Insert → Break" to control flow

**Workflow runs slow?**
- OpenAI response times vary; 2-3 min is normal
- Check n8n execution logs for bottlenecks
- Consider using GPT-5-mini instead of GPT-4o (faster, cheaper)

See [TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md) for more.

## Examples

### Sample Output Structure

**Outline (Google Docs):**
```
Meta Title: The Rise of AI Automation in Content Creation
Meta Description: Discover how AI tools are transforming content workflows...

Keywords: AI automation, content creation, workflow optimization...

Sections:
1. Introduction (150 words)
2. What Is AI Automation? (200 words)
3. How It Works (300 words)
4. Real-World Examples (250 words)
5. ROI & Metrics (250 words)
6. Getting Started (150 words)

FAQs:
Q: Is AI-generated content plagiarism?
A: No, when used properly...

References:
[1] OpenAI. "GPT-5 Technical Report." https://...
```

**Article (Google Docs):**
Full 1,200-word narrative with headers, transitions, and readable prose.

**Images (Google Drive):**
Three distinct images tagged as Image 1, Image 2, Image 3 with Drive links embedded in article.

## Contributing

Issues, suggestions, and improvements welcome. Open an issue or submit a pull request.

## License

MIT License. See LICENSE file for details.

## Author

Built by Binary Horizon — AI automation and technical education.

---

**Questions?** Check the [docs folder](docs/) or open a GitHub issue.
