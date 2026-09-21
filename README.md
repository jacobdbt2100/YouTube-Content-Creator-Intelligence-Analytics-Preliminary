# YouTube Content Creator Real-Time Intelligence Analytics Preliminary
An incremental data pipeline for collecting, transforming, and analyzing YouTube creator and content data.
___

## Tables

**1. creators**

| Attribute       | Description                               |
| --------------- | ----------------------------------------- |
| channel_id (PK) | Unique identifier for the YouTube channel |
| channel_name    | Name of the channel                       |

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




