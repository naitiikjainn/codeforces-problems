# Codeforces Problems Static Repository

This repository contains pre-scraped Codeforces problem statements for use with the AI Code Editor project.

## Structure

```
content/
  {contestId}_{index}.html    # e.g., 1890_A.html, 2052_M.html
```

## Usage

Problems are accessible via raw GitHub content:

```
https://raw.githubusercontent.com/{YOUR_USERNAME}/codeforces-problems/main/content/{contestId}_{index}.html
```

## Setup Your Own Repository

### 1. Create GitHub Repository
1. Go to https://github.com/new
2. Name it `codeforces-problems`
3. Make it **public** (required for raw content access)
4. Initialize with README

### 2. Clone and Setup
```bash
cd AI-Code-Editor
git clone https://github.com/YOUR_USERNAME/codeforces-problems.git
# Or if you already have the folder:
cd codeforces-problems
git init
git remote add origin https://github.com/YOUR_USERNAME/codeforces-problems.git
```

### 3. Configure Backend
Update `backend/utils/scraperService.js`:
```javascript
const STATIC_REPO_CONFIG = {
    primary: {
        owner: "YOUR_GITHUB_USERNAME",  // <-- Change this
        repo: "codeforces-problems",
        branch: "main"
    },
    ...
};
```

Or set environment variables:
```bash
export GITHUB_PROBLEMS_OWNER=YOUR_USERNAME
export GITHUB_PROBLEMS_REPO=codeforces-problems
```

## Scraping Problems

### Using Puppeteer (Recommended - bypasses Cloudflare)

```bash
cd backend/scripts

# Single problem
node scrape_with_puppeteer.js 1890 A

# Entire contest
node scrape_with_puppeteer.js --contest 2050

# Recent contests (last N)
node scrape_with_puppeteer.js --recent 20
```

### Simple Scraper (May be blocked by Cloudflare)

```bash
# Single problem
node scrape_single_problem.js 1890 A

# Recent contests
node scrape_codeforces_problems.js --recent 50

# Specific contest
node scrape_codeforces_problems.js --contest 2050
```

## Uploading to GitHub

After scraping:
```bash
cd codeforces-problems
git add -A
git commit -m "Add new problems"
git push origin main
```

Or use the helper script:
```bash
cd backend/scripts
node upload_to_github.js
```

## Notes

- **Puppeteer scraper** opens a real browser window to bypass Cloudflare
- **Rate limiting**: Scripts wait 2-3 seconds between requests
- **Cloudflare protection**: If blocked, try again later or use a VPN
- **File naming**: Uses underscore (`_`) separator for Windows compatibility
- Problems are cached for 2 days in Redis, permanently in MongoDB

## Troubleshooting

### "403 Forbidden" errors
- Cloudflare is blocking requests
- Use the Puppeteer scraper instead
- Try from a different IP/network

### Browser closes immediately
- Install Chrome/Chromium
- Run `npm install puppeteer` in backend folder

### Files not appearing on GitHub
- Ensure repository is **public**
- Check that files were committed and pushed
- Wait a few minutes for GitHub CDN to update

## License

Problem statements are © Codeforces. This repository is for educational purposes only.
