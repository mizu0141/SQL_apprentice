

#### テーブル: channels
| 列名         | データ型      | 制約                  |
|--------------|--------------|-----------------------|
| channel_id   | INT          | PRIMARY KEY, AUTO_INCREMENT |
| channel_name | VARCHAR(255) | NOT NULL              |

#### テーブル: programs
| 列名           | データ型      | 制約                  |
|----------------|--------------|-----------------------|
| program_id     | INT          | PRIMARY KEY, AUTO_INCREMENT |
| program_name   | VARCHAR(255) | NOT NULL              |
| program_detail | TEXT         |                       |

#### テーブル: seasons
| 列名            | データ型 | 制約                           |
|-----------------|---------|--------------------------------|
| season_id       | INT     | PRIMARY KEY, AUTO_INCREMENT    |
| program_id      | INT     | NOT NULL, FOREIGN KEY         |
| season_number   | INT     | NOT NULL                       |

#### テーブル: genres
| 列名          | データ型      | 制約                  |
|---------------|--------------|-----------------------|
| genre_id      | INT          | PRIMARY KEY, AUTO_INCREMENT |
| genre_name    | VARCHAR(255) | NOT NULL              |

#### テーブル: program_genres
| 列名         | データ型 | 制約                            |
|--------------|---------|---------------------------------|
| program_id   | INT     | NOT NULL, FOREIGN KEY           |
| genre_id     | INT     | NOT NULL, FOREIGN KEY           |
|              |         | PRIMARY KEY (program_id, genre_id) |

#### テーブル: time_slots
| 列名            | データ型      | 制約                  |
|-----------------|--------------|-----------------------|
| time_slot_id    | INT          | PRIMARY KEY, AUTO_INCREMENT |
| time_slot_name  | VARCHAR(255) | NOT NULL              |
| start_time      | TIME         | NOT NULL              |
| end_time        | TIME         | NOT NULL              |

#### テーブル: episodes
| 列名            | データ型 | 制約                           |
|-----------------|---------|--------------------------------|
| episode_id      | INT     | PRIMARY KEY, AUTO_INCREMENT    |
| season_id       | INT     | NOT NULL, FOREIGN KEY         |
| episode_number  | INT     | NOT NULL                       |
| episode_name    | VARCHAR(255) | NOT NULL                    |
| episode_detail  | TEXT    |                               |
| start_time      | TIME    |                               |
| end_time        | TIME    |                               |

#### テーブル: channel_episodes
| 列名             | データ型 | 制約                              |
|------------------|---------|-----------------------------------|
| channel_id       | INT     | NOT NULL, FOREIGN KEY             |
| episode_id       | INT     | NOT NULL, FOREIGN KEY             |
| broadcast_date   | DATE    | NOT NULL                          |
| time_slot_id     | INT     | NOT NULL, FOREIGN KEY             |
| start_time       | TIME    |                                   |
| end_time         | TIME    |                                   |
| views_count      | INT     | DEFAULT 0                         |
|                  |         | PRIMARY KEY (channel_id, episode_id, broadcast_date) |
|                  |         | UNIQUE (channel_id, broadcast_date, time_slot_id)   |
