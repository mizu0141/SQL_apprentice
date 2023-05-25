

#### テーブル: channels
| 列名          | データ型     |Null   | Key | 初期値    | AUTO_INCREMENT |
|--------------|--------------|-------|-----|-----------|----------------|
|id   | INT          | -     |PRI  | -         |YES             |
| channel_name | VARCHAR(50)  | -     | -   | -         |NO              |

#### テーブル: programs
| 列名          | データ型     |Null   | Key | 初期値    | AUTO_INCREMENT |
|--------------|--------------|-------|-----|-----------|----------------|
| program_id     | INT          | -     |PRI  | -         |YES            |
| program_name   | VARCHAR(100) | -     | -   | -         |NO             |
| program_detail | TEXT         | -     | -   | -         |NO             |
| is_series      | BOOLEAN      | -     | -   | true      |NO             |

#### テーブル: seasons
| 列名          | データ型     |Null   | Key | 初期値    | AUTO_INCREMENT |
|--------------|--------------|-------|-----|-----------|----------------|
|id       | INT          | -     |PRI  | -         |YES             |
| program_id      | INT          | -     | -   | -         |NO              |
| season_number   | INT          | -     | -   | -         |NO              |

#### テーブル: genres
| 列名          | データ型     |Null   | Key | 初期値    | AUTO_INCREMENT |
|--------------|--------------|-------|-----|-----------|----------------|
|id     | INT          | -     |PRI  | -         |YES             |
| genre_name   | VARCHAR(50) | -     | -   | -         |NO              |


#### テーブル: program_genres
| 列名          | データ型     |Null   | Key | 初期値    | AUTO_INCREMENT | 参照|
|--------------|--------------|-------|-----|-----------|----------------|-----|
| program_id     | INT          | -     |PRI, FK | -         |NO         | programs(program_id)|
| genre_id       | INT          | -     |PRI, FK | -         |NO         |  genres(id)|


#### テーブル: time_slots
| 列名          | データ型     |Null   | Key | 初期値    | AUTO_INCREMENT |
|--------------|--------------|-------|-----|-----------|----------------|
| id   | INT          | -     |PRI  | -         |YES             |
| time_slot_name | VARCHAR(50) | -     | -   | -         |NO              |



#### テーブル: episodes
| 列名          | データ型     |Null   | Key | 初期値    | AUTO_INCREMENT |
|--------------|--------------|-------|-----|-----------|----------------|
|id   | INT          | -     |PRI  | -         |YES             |
| season_id      | INT          | -     | -   | -         |NO              |
| episode_number | INT          | -     | -   | -         |NO              |
| episode_title  | VARCHAR(100) | -     | -   | -         |NO              |
| episode_detail | TEXT         | -     | -   | -         |NO              |
| episode_length | TIME         | -     | -   | -         |NO              |
| published_date | DATETIME     | -     | -   | -         |NO              |




#### テーブル: channel_episodes
| 列名          | データ型     |Null   | Key | 初期値    | AUTO_INCREMENT | 参照|
|--------------|--------------|-------|-----|-----------|----------------|-----|
| channel_id     | INT          | -     |PRI  | -         |YES             | channels(id)|
| episode_id     | INT          | -     |PRI  | -         |YES             | episodes(id)|
| time_slot_id   | INT          | -     |FK | -         |NO              | time_slots(id)|
| season_id      | INT          | -     |   | -         |NO              | seasons(id)|
| start_time     | TIME         | -     | -   | -         |NO              |-|
| end_time       | TIME         | -     | -   | -         |NO              |-|

#### 追加
#### テーブル:view_count
| 列名          | データ型     |Null   | Key | 初期値    | AUTO_INCREMENT | 参照|
|--------------|--------------|-------|-----|-----------|----------------|-----|
| id   | INT          | -     |PRI  | -         |YES             |-|
| channel_id     | INT          | -     |FK   | -         |NO             | channels(id)|
| episode_id     | INT          | -     |FK   | -         |NO             | episodes(id)|
| view_count   | INT          | -     | -   | -         |NO             |-|

