# save-picture

A private Telegram bot that saves pictures to my personal Google Drive.

> **Status: in development.** This is a Python rewrite of my existing .NET bot, built as a learning project. The features below describe the target version; this README is updated as they are implemented.

## What it does

The bot serves a single user (me) and ignores everyone else.

| I send the bot                    | The bot                                                        |
| --------------------------------- | -------------------------------------------------------------- |
| a photo                           | saves it to my Google Drive                                    |
| a direct link to an image         | downloads the image and saves it                               |
| a link to a Reddit post           | finds the image(s) of that post (single image or gallery) and saves them to a separate Drive folder |
| anything else                     | replies that it did not understand                             |

Typical use: I see a picture on Reddit, tap **Share**, send the link to my bot, and the picture ends up in my Google Drive.

## Reddit API usage

Reddit access goes through the official Data API only, in line with Reddit's [Responsible Builder Policy](https://support.reddithelp.com/hc/en-us/articles/42728983564564-Responsible-Builder-Policy) and [Data API Terms](https://redditinc.com/policies/data-api-terms).

- **Read-only.** The bot only fetches the single post whose link I send (and resolves Reddit share links to that post). No listing or crawling of subreddits, no search, no user data.
- **No write actions.** No posts, comments, votes or messages.
- **No scraping.** HTML pages and unauthenticated `.json` endpoints are not used.
- **Authentication:** OAuth with a registered app.
- **User-Agent:** unique and descriptive, in the format required by Reddit (platform, app id, version and the owner's Reddit username).
- **Rate limits:** a few requests per day; `X-Ratelimit-*` response headers are respected.
- **Optional:** without Reddit API credentials the Reddit feature is disabled and the rest of the bot works as usual.

## Data handling

- Only image files from posts I explicitly share are stored, in my private Google Drive.
- Images from Reddit go to a separate folder, so they can be reviewed and cleaned up independently.
- No usernames, comments, profiles or other user data are stored.
- Nothing is shared, published, sold or used for AI/ML training.

## Tech stack

- Python 3.14, [uv](https://docs.astral.sh/uv/)
- [aiogram](https://aiogram.dev/) — Telegram bot framework
- `aiohttp` — HTTP client for downloads and Reddit API requests
- Google Drive API (`google-api-python-client`, `google-auth`)
- [Infisical](https://infisical.com/) — secrets management
- ruff, mypy (strict), pytest
- Deployed with systemd on a personal linux-arm64 server

## Contact

Owner: [@vSakun](https://github.com/vSakun).
