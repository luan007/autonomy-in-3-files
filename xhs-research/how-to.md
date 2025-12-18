# XHS Fetch CLI - How To Guide

This tool (`xhs-fetch.js`) is designed to scrape data from Xiaohongshu (Little Red Book) via a local bridge service. It supports finding content, authors, and extracting detailed post information including comments.

## Prerequisites
1.  **Bridge Service**: The browser automation bridge must be running.
    ```bash
    cd xhs/builder
    npm start
    ```
    *Keep this running in a separate terminal.*

## Command Reference

Run the tool using `node xhs-fetch.js`.

### 1. Search
Fetch posts matching a keyword.
```bash
node xhs-fetch.js search <keyword> <limit> [options]
```
- `keyword`: Search term (e.g. "camping", "coffee").
- `limit`: Number of items to *Deep Fetch* (see options).

### 2. Author Feed
Fetch posts from a specific author.
```bash
node xhs-fetch.js author <url_or_id> <limit> [options]
```
- `url_or_id`: Full URL to author profile or just the ID.

### 3. Index Feed
Fetch the main recommendation feed.
```bash
node xhs-fetch.js index <limit> [options]
```

### 4. Detail
Fetch a specific note by URL.
```bash
node xhs-fetch.js detail <url> [options]
```
*Note: This implicitly uses Deep Fetch.*

## Options

### `--session <name>`
**Highly Recommended.**
Appends data to `xhs/data/session-<name>.json`.
- **Deduplication**: Automatically deduplicates items by ID.
- **Persistence**: Safe to run multiple times to build a dataset.
- **Example**: `--session my-research`

### `--deep`
Enables "Deep Fetch" mode.
- **Default (Shallow)**: Fast. Captures Title, Image URL, Likes, Link, Author.
- **Deep**: Slower. Visits *each* note page to capture:
  - Full Description
  - Tags
  - Comments (Top ~20)
  - Timestamps

## Data Strategy (Harvest Mode)

The tool is optimized to "Harvest" data efficiently:
1.  **Collateral Saving**: When you run a command, it scrolls the feed to find enough items. *Every item seen* is saved as a "Shallow" item, even if it exceeds your requested limit.
    - *Example*: `search "cats" 1` might save 10 items to your session file because they were visible on screen.
2.  **Deep Limit**: The `<limit>` argument controls how many items are *Deep Fetched* (scraped for details).
    - *Example*: `search "cats" 2 --deep` will save ~10 shallow items, and then upgrade the first 2 to deep.
3.  **Versioning**: 
    - Shallow items have `"fetch_status": "shallow"`.
    - Deep items have `"fetch_status": "deep"`.
    - If you run deep fetch on an existing shallow item, it updates the record to deep.

## Example Workflow

### 1. Broad Search (Harvest)
Gather many potential candidates quickly.
```bash
node xhs-fetch.js search "glamping" 1 --session glamp-research
```
*Result*: `data/session-glamp-research.json` populated with ~10-20 shallow items.

### 2. Deep Clean
Fetch details for top results.
```bash
node xhs-fetch.js search "glamping" 5 --session glamp-research --deep
```
*Result*: The first 5 items in the file are upgraded to contain full text and comments.

## JSON Structure
```json
{
  "meta": { ... },
  "data": [
    {
      "id": "67bb...",
      "link": "https://...",
      "title": "Camping Guide",
      "likes": "500",
      "fetch_status": "shallow"
    },
    {
      "id": "69ac...",
      "title": "Best Tents",
      "fetch_status": "deep",
      "desc": "Full post content...",
      "tags": ["#camping", "#gear"],
      "comments": [ { "author": "User A", "content": "Cool!" } ]
    }
  ]
}
```
