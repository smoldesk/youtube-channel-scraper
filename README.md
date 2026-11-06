# YouTube Channel Scraper

![YouTube Channel Scraper](https://raw.githubusercontent.com/smoldesk/apify-scrapers/main/assets/youtube-channel/banner.webp)

Scrape YouTube channels: subscribers, exact total views, video count, join date, country, description and social links, plus the channel's latest videos, Shorts and live streams. No API key, no quota, no login. Runs on [Apify](https://apify.com/smoldesk/youtube-channel-scraper).

## What you get

- **Channel profiles**: subscribers, exact total views, video count, join date, country, verified badge, description, keywords, About-page links (Instagram, TikTok, website, shop…), avatar, banner and RSS URL.
- **Videos, Shorts and live streams** from each channel: title, exact views and likes, comment count, publish date, duration, description links, hashtags and chapters.
- **Profiles only**: leave `channelTabs` empty to turn a list of handles into a creator table, fast and cheap.
- **New videos only**: `publishedAfter` stops reading a channel once it reaches older uploads.

## Run it

Open the [YouTube Channel Scraper on Apify](https://apify.com/smoldesk/youtube-channel-scraper), fill in the form and click Start. Or call it from code with your [Apify token](https://console.apify.com/settings/integrations):

```bash
curl -X POST "https://api.apify.com/v2/acts/smoldesk~youtube-channel-scraper/run-sync-get-dataset-items?token=YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "startUrls": ["@mkbhd", "https://www.youtube.com/@veritasium"],
    "channelTabs": ["videos", "shorts"],
    "maxResults": 50,
    "publishedAfter": "30 days"
  }'
```

Python, profiles only:

```python
from apify_client import ApifyClient

client = ApifyClient("YOUR_TOKEN")
run = client.actor("smoldesk/youtube-channel-scraper").call(
    run_input={"startUrls": ["@mkbhd", "@veritasium", "@kurzgesagt"], "channelTabs": []}
)
for ch in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(ch["name"], ch["subscriberCount"], ch["viewCount"], ch["country"])
```

## Sample output

```json
{
  "type": "channel",
  "id": "UCBJycsmduvYEL83R_U4JriQ",
  "url": "https://www.youtube.com/@mkbhd",
  "handle": "@mkbhd",
  "name": "Marques Brownlee",
  "subscriberCount": 21400000,
  "videoCount": 1864,
  "viewCount": 5873912525,
  "joinedAt": "2008-03-21",
  "country": "United States",
  "isVerified": true,
  "links": [
    {"title": "Twitter", "url": "http://twitter.com/MKBHD"},
    {"title": "Instagram", "url": "http://instagram.com/MKBHD"}
  ],
  "rssUrl": "https://www.youtube.com/feeds/videos.xml?channel_id=UCBJycsmduvYEL83R_U4JriQ"
}
```

Video records carry the same keys as [YouTube Scraper](https://github.com/smoldesk/youtube-scraper): views, likes, comment count, publish date, duration, chapters, `sourceInput` and `sourcePosition` (1 = newest).

## Pricing

Pay per channel profile or video. No monthly fee. The price drops on bigger Apify plans.

| Apify plan | Per 1,000 profiles or videos |
|---|---|
| Free | $1.00 |
| Starter | $0.80 |
| Scale | $0.60 |
| Business and up | $0.40 |

## Limits

- No keyword search for channels: give channel URLs or handles.
- Subscriber and comment counts are rounded the way YouTube displays them. View and like counts are exact.
- Shorts and live streams have `durationSeconds: null`.

## FAQ

**Do I need a YouTube API key?**
No. There is no daily quota and no Google Cloud project to set up.

**How do I get every video of a channel?**
Set `maxResults` to 0. Add `shorts` and `streams` to `channelTabs` for those tabs.

**Can I get only subscriber counts for a list of channels?**
Yes. Leave `channelTabs` empty and you pay only for the profiles.

## More scrapers

YouTube Channel is one of 17 no-login scrapers by Smoldesk Studios. For keyword search and comments see [YouTube Scraper](https://github.com/smoldesk/youtube-scraper) and [YouTube Comments Scraper](https://github.com/smoldesk/youtube-comments-scraper). Full list: [smoldesk/apify-scrapers](https://github.com/smoldesk/apify-scrapers).
