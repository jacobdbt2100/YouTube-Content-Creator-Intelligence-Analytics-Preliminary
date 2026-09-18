# YouTube Content Creator Intelligence Analytics Preliminary
Analyzing how content performance, audience engagement, content characteristics, categories, and creator growth change over time.
___

1. creators

| Attribute       | Description                               |
| --------------- | ----------------------------------------- |
| channel_id (PK) | Unique identifier for the YouTube channel |
| channel_name    | Name of the channel                       |
| description     | Channel description                       |
| channel_url     | URL of the channel                        |
| country         | Channel's country                         |
| category        | Channel category                          |
| created_at      | Channel creation date/time                |

2. creator_daily_stats

| Attribute        | Description                        |
| ---------------- | ---------------------------------- |
| channel_id (FK)  | Identifier of the YouTube channel  |
| collection_date  | Date the statistics were collected |
| subscriber_count | Number of channel subscribers      |
| total_view_count | Total views across the channel     |
| video_count      | Number of videos on the channel    |

3. video_category

| Attribute        | Description                              |
| ---------------- | ---------------------------------------- |
| category_id (PK) | Unique identifier for the video category |
| category         | Name of the video category               |

4. videos

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

5. video_daily_stats

| Attribute       | Description                        |
| --------------- | ---------------------------------- |
| video_id (FK)   | Identifier of the video            |
| collection_date | Date the statistics were collected |
| view_count      | Number of views                    |
| like_count      | Number of likes                    |
| comment_count   | Number of comments                 |
