# YouTube Content Creator Real-Time Intelligence Analytics Preliminary
Analyzing how content performance, audience engagement, content characteristics, categories, and creator growth change over time.
___

## Tables

**1. creators**

| Attribute       | Description                               |
| --------------- | ----------------------------------------- |
| channel_id (PK) | Unique identifier for the YouTube channel |
| channel_name    | Name of the channel                       |
| description     | Channel description                       |
| channel_url     | URL of the channel                        |
| country         | Channel's country                         |
| created_at      | Channel creation date/time                |

**2. creator_daily_stats**

| Attribute        | Description                        |
| ---------------- | ---------------------------------- |
| channel_id (FK)  | Identifier of the YouTube channel  |
| collection_date  | Date the statistics were collected |
| subscriber_count | Number of channel subscribers      |
| total_view_count | Total views across the channel     |
| video_count      | Number of videos on the channel    |

**3. video_category**

| Attribute        | Description                              |
| ---------------- | ---------------------------------------- |
| category_id (PK) | Unique identifier for the video category |
| category         | Name of the video category               |

**4. videos**

| Attribute        | Description                                |
| ---------------- | ------------------------------------------ |
| video_id (PK)    | Unique identifier for the video            |
| channel_id (FK)  | Identifier of the YouTube channel          |
| category_id (FK) | Identifier of the video category           |
| title            | Title of the video                         |
| published_at     | Date and time the video was published      |
| duration         | Length of the video                        |
| video_url        | URL of the video                           |
| collected_at     | Date and time the video data was collected |

**5. video_daily_stats**

| Attribute       | Description                        |
| --------------- | ---------------------------------- |
| video_id (FK)   | Identifier of the video            |
| collection_date | Date the statistics were collected |
| view_count      | Number of views                    |
| like_count      | Number of likes                    |
| comment_count   | Number of comments                 |

## Table Relationships

| Parent Table         | Relationship | Child Table        | Foreign Key   |
|-------------------|--------------|-----------------------|---------------|
| `creators`        | 1 → *        | `videos`              | channel_id    |
| `creators`        | 1 → *        | `creator_daily_stats` | channel_id    |
| `video_category`  | 1 → *        | `videos`              | category_id   |
| `videos`          | 1 → *        | `video_daily_stats`   | video_id      |

## Data Collection Frequency

| Table                 | Extraction Frequency    |
| --------------------- | ----------------------- |
| `creators`            | Once / When Discovered  |
| `videos`              | Once / When Discovered  |
| `video_category`      | Once / When Discovered  |
| `creator_daily_stats` | Daily                   |
| `video_daily_stats`   | Daily                   |

## Modelling Concepts Utilised

- Normalization
- Primary/foreign keys
- Reference/lookup table
- Dimension-style tables
- Historical fact/snapshot tables
- One-to-many relationships
- Separation of static metadata from changing metrics

## Repository Structure

```text
youtube_content_creator_intelligence/
│
├── .env
├── .gitignore
├── README.md
├── main.py
├── requirements.txt
│
├── csv_output/
│   ├── creators.csv
│   ├── creator_daily_stats.csv
│   ├── video_category.csv
│   ├── video_daily_stats.csv
│   └── videos.csv
│
├── extraction/
│   ├── __init__.py
│   ├── creator_daily_stats.py
│   ├── creators.py
│   ├── video_category.py
│   ├── video_daily_stats.py
│   ├── videos.py
│   └── youtube_client.py
│
└── transformation/
    ├── __init__.py
    ├── creator_daily_stats.py
    ├── creators.py
    ├── video_category.py
    ├── video_daily_stats.py
    └── videos.py
```




# YouTube Content & Creator Intelligence — Build Order

## Project Build Sequence

### 1. YouTube API Client

**File:** `extraction/youtube_client.py`

* Load environment variables from `.env`
* Retrieve the YouTube API key
* Create a reusable YouTube API client
* Keep API setup separate from extraction logic

### 2. Video Categories

**File:** `extraction/video_category.py`

* Fetch all available video categories for `NG`
* Extract `category_id` and `category`
* This is a once-off extraction
* Output will eventually populate `video_category`

### 3. Creators

**File:** `extraction/creators.py`

* Fetch metadata for the selected YouTube channels
* Extract the fields required by `creators`
* This is primarily a once-off extraction
* Allow for occasional metadata updates later

### 4. Videos

**File:** `extraction/videos.py`

* Identify the videos selected for tracking
* Fetch video metadata
* Extract only fields belonging to `videos`
* Store video metadata when a video is first added to tracking
* Allow for occasional metadata changes later

### 5. Creator Daily Statistics

**File:** `extraction/creator_daily_stats.py`

* Fetch current statistics for tracked channels
* Extract:

  * `channel_id`
  * `collection_date`
  * `subscriber_count`
  * `total_view_count`
  * `video_count`
* Run daily

### 6. Video Daily Statistics

**File:** `extraction/video_daily_stats.py`

* Fetch current statistics for tracked videos
* Extract:

  * `video_id`
  * `collection_date`
  * `view_count`
  * `like_count`
  * `comment_count`
* Run daily

---

## Transformation Layer

Each table has its own transformation module.

### 7. Creators Transformation

**File:** `transformation/creators.py`

* Convert extracted data into a DataFrame
* Clean and standardise fields
* Save `creators.csv`

### 8. Creator Daily Statistics Transformation

**File:** `transformation/creator_daily_stats.py`

* Convert extracted data into a DataFrame
* Standardise dates and numeric fields
* Save `creator_daily_stats.csv`

### 9. Video Category Transformation

**File:** `transformation/video_category.py`

* Convert category data into a DataFrame
* Standardise fields
* Save `video_category.csv`

### 10. Videos Transformation

**File:** `transformation/videos.py`

* Convert extracted data into a DataFrame
* Standardise metadata
* Save `videos.csv`

### 11. Video Daily Statistics Transformation

**File:** `transformation/video_daily_stats.py`

* Convert extracted data into a DataFrame
* Standardise dates and numeric fields
* Save `video_daily_stats.csv`

---

## Orchestration

### 12. Main Pipeline

**File:** `main.py`

* Import the required extraction and transformation functions
* Run them in the appropriate order
* Coordinate the complete CSV-based pipeline
* Keep orchestration logic here rather than inside individual modules




