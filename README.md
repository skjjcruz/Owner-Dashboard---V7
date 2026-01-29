# NFL Draft Rankings Auto-Updater

Automatically fetches and updates 2026 NFL Draft rankings from Pro Football Network's Industry Consensus Big Board every day at 3 AM EST.

## 🎯 What This Does

- **Fetches** the latest rankings JSON from PFN daily
- **Transforms** 300+ player rankings into your CSV format
- **Updates** both this repo AND your main Owner-Dashboard repo automatically
- **Runs** at 3 AM EST every day when traffic is low

## 📁 Files

- `update_rankings.py` - Main scraper script that fetches and transforms data
- `.github/workflows/update-rankings.yml` - GitHub Actions automation
- `requirements.txt` - Python dependencies
- `2026-Dynasty-Rankings.csv` - Output file (auto-generated)
- `2026-Dynasty-Rankings_timestamp.txt` - Last update time

## 🚀 Setup Instructions

### Step 1: Create This Repository

1. Create a new repo: `NFL-Draft-Rankings-Scraper`
2. Upload these files to it
3. Make sure Actions are enabled (Settings → Actions → General)

### Step 2: Set Up Personal Access Token (PAT)

The workflow needs permission to push to your Owner-Dashboard repo:

1. Go to GitHub Settings → Developer Settings → Personal Access Tokens → Tokens (classic)
2. Click "Generate new token (classic)"
3. Give it a name: "Rankings Scraper"
4. Select scopes:
   - ✅ `repo` (Full control of private repositories)
5. Generate token and COPY IT (you won't see it again!)
6. Go to your NFL-Draft-Rankings-Scraper repo → Settings → Secrets and variables → Actions
7. Click "New repository secret"
8. Name: `PAT_TOKEN`
9. Value: Paste your token
10. Click "Add secret"

### Step 3: Configure GitHub Actions Permissions

In your NFL-Draft-Rankings-Scraper repo:

1. Settings → Actions → General → Workflow permissions
2. Select "Read and write permissions"
3. Check "Allow GitHub Actions to create and approve pull requests"
4. Save

### Step 4: Test It!

You can test the workflow manually before waiting for 3 AM:

1. Go to Actions tab in your repo
2. Click "Update NFL Draft Rankings" workflow
3. Click "Run workflow" → "Run workflow"
4. Watch it run!

## 📊 Data Sources

Currently fetching from:
- **PFN (Pro Football Network)** - Industry Consensus Big Board
- Maps to these columns in your CSV:
  - PFF → PFF column
  - CBS → CBS column  
  - ESPN → Mel Kiper column
  - PFSN → PFN column
  - B/R → Drafttech column
  - The Athletic → NFL Draft Buzz column

## ⏰ Schedule

- Runs daily at **3:00 AM EST** (8:00 AM UTC)
- Can also be triggered manually anytime via Actions tab

## 🔧 How It Works

1. **3:00 AM** - GitHub Actions triggers workflow
2. **Fetch** - Script downloads latest JSON from PFN
3. **Transform** - Converts JSON to your CSV format
4. **Commit** - Commits to this repo with timestamp
5. **Sync** - Copies updated CSV to Owner-Dashboard repo
6. **Done** - Your app automatically picks up changes!

## 📝 Notes

- The script tries to find the JSON file using pattern matching (dates in filename)
- If today's file isn't available, it checks the last 7 days
- Only commits if there are actual changes (prevents empty commits)
- Timestamps show when data was last updated
- Both repos get updated in the same workflow run

## 🆘 Troubleshooting

### "No changes detected"
- This is normal! Means rankings haven't changed since last run
- Data only updates when PFN publishes new rankings

### "Permission denied" errors  
- Check that PAT_TOKEN secret is set correctly
- Verify workflow permissions are set to "Read and write"

### Script fails to find JSON
- PFN might have changed their file structure
- Check the URL pattern in `update_rankings.py`
- Look at PFN website source to find current pattern

## 🔄 Updating the Script

If you need to modify the transformation logic:

1. Edit `update_rankings.py`
2. Test locally: `python update_rankings.py`
3. Commit and push
4. Next run will use the updated script

## 📈 Future Enhancements

Possible additions:
- Add more ranking sources as they become available
- Calculate consensus rankings from multiple sources
- Send notifications when rankings change significantly
- Historical tracking of ranking changes over time
