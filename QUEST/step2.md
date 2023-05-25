## STEP1

### 1.データベースの作成・選択

- MySQLを起動して、ログインし、以下のコマンドを実行してデータベースを作成してください。

  ```sql
    CREATE DATABASE internet_tv;
  ```
- 作成したデータベースを選択して下さい。
  ```sql
    USE internet_tv;
  ```

### 2.テーブルの作成

## チャンネルテーブルの作成
- チャンネルテーブルを以下のコマンドを実行して、作成してください。

  ```sql
    CREATE TABLE channels (
       channel_id INT PRIMARY KEY AUTO_INCREMENT,
       channel_name VARCHAR(50) NOT NULL
     );
  ```

## 番組テーブルの作成
- 番組テーブルを以下のコマンドを実行して、作成してください。

　　　```sql
    CREATE TABLE programs (
       program_id INT PRIMARY KEY AUTO_INCREMENT,
       program_name VARCHAR(100) NOT NULL,
       program_detail TEXT NOT NULL
       is_series BOOLEAN NOT NULL  DEFAULT TRUE
　　　);
  ```

## シーズンテーブルの作成
- シーズンテーブルを以下のコマンドを実行して、作成してください。

 ```sql
    CREATE TABLE seasons (
       season_id INT PRIMARY KEY AUTO_INCREMENT,
       program_id INT NOT NULL,
       season_number INT NOT NULL,
    FOREIGN KEY (program_id) REFERENCES programs(program_id)
    );
  ```

## ジャンルテーブルの作成
- ジャンルテーブルを以下のコマンドを実行して、作成してください。

 ```sql
    CREATE TABLE genres (
       genre_id INT PRIMARY KEY AUTO_INCREMENT,
       genre_name VARCHAR(50) NOT NULL
       );
  ```

## 番組とジャンルの関連テーブルの作成
- 番組とジャンルの関連テーブルを以下のコマンドを実行して、作成してください。

  ```sql
    CREATE TABLE program_genres (
       program_id INT NOT NULL,
       genre_id INT NOT NULL,
    PRIMARY KEY (program_id, genre_id),
    FOREIGN KEY (program_id) REFERENCES programs(program_id),
    FOREIGN KEY (genre_id) REFERENCES genres(genre_id)
    );
  ```

## 時間帯テーブルの作成
- 時間帯テーブルを以下のコマンドを実行して、作成してください。

```sql
    CREATE TABLE time_slots (
       time_slot_id INT PRIMARY KEY AUTO_INCREMENT,
       time_slot_name VARCHAR(50) NOT NULL,
       );
```

## エピソードテーブルの作成
- エピソードテーブルを以下のコマンドを実行して、作成してください。

```sql
    CREATE TABLE episodes (
       episode_id INT PRIMARY KEY AUTO_INCREMENT,
       season_id INT NOT NULL,
       episode_number INT NOT NULL,
       episode_name VARCHAR(255) NOT NULL,
       episode_detail TEXT,
       episode_length INT
       publish_date DATE,
    FOREIGN KEY (season_id) REFERENCES seasons(season_id)
    );
```

## チャンネルとエピソードの関連テーブルの作成
- チャンネルと時間帯の関連テーブルを以下のコマンドを実行して、作成してください。

```sql
    CREATE TABLE channel_episodes (
       channel_id INT NOT NULL,
       episode_id INT NOT NULL,
       time_slot_id INT NOT NULL,
       broadcast_date DATE NOT NULL,
       start_time TIME,
       end_time TIME,
    PRIMARY KEY (channel_id, episode_id, time_slot_id),
    FOREIGN KEY (channel_id) REFERENCES channels(channel_id),
    FOREIGN KEY (episode_id) REFERENCES episodes(episode_id),
    FOREIGN KEY (time_slot_id) REFERENCES time_slots(time_slot_id),
    UNIQUE (channel_id, broadcast_date, time_slot_id)
    );
 ```

## テーブル: view_countの作成
``` sql
CREATE TABLE view_count (
    view_count_id INT PRIMARY KEY AUTO_INCREMENT,
    episode_id INT NOT NULL,
    view_date DATE NOT NULL,
    view_count INT NOT NULL,
    FOREIGN KEY (episode_id) REFERENCES episodes(episode_id)
);
```
### 3.サンプルデータの挿入
- INSERT文を実行して、チャンネルテーブルに以下のデータを入力してください。

    ```sql
       INSERT INTO channels(channel_name) VALUES ('チャンネル1'), ('チャンネル2'), ('チャンネル3'), ('チャンネル4'), ('チャンネル5'), ('チャンネル6'), ('チャンネル7'), ('チャンネル8'), ('チャンネル9'), ('チャンネル10');

       INSERT INTO programs(program_name, program_detail) VALUES ('プログラム1', 'プログラム1の詳細'), ('プログラム2', 'プログラム2の詳細'), ('プログラム3', 'プログラム3の詳細'), ('プログラム4', 'プログラム4の詳細'), ('プログラム5', 'プログラム5の詳細'), ('プログラム6', 'プログラム6の詳細'), ('プログラム7', 'プログラム7の詳細'), ('プログラム8', 'プログラム8の詳細'), ('プログラム9', 'プログラム9の詳細'), ('プログラム10', 'プログラム10の詳細');

       INSERT INTO seasons (program_id, season_number) VALUES (1, 1),(1, 2),(1, 3), (1, 4), (1, 5),(1 ,6),(1, 7),(1, 8),(1, 9),(2, 1),(2, 2),(2, 3),(2, 4), (2, 5), (2, 6),(3, 1),(3, 2),(3, 3),(4, 1), (4, 2),(4, 3),(4, 4),(4,5),(4, 6)

       INSERT INTO genres (genre_name) VALUES ('アクション'), ('SF'), ('恋愛'), ('ホラー'), ('ドラマ'), ('アニメ'), ('バラエティ'), ('ニュース'), ('スポーツ'), ('音楽');

       INSERT INTO program_genres  (program_id, genre_id) VALUES (1, 1),(1, 2), (2, 3), (2, 4), (3, 1), (3, 5), (4, 6), (4, 7), (5, 8), (5, 9), (6, 10);;

       INSERT INTO episodes (season_id, episode_number, episode_name, episode_detail, duration, publish_date) VALUES (1, 1, 'エピソード1', 'エピソード1の詳細', '00:30:00','2020-01-01'), (1, 2, 'エピソード2', 'エピソード2の詳細', '00:30:00', '2020-01-01'), (1, 3, 'エピソード3', 'エピソード3の詳細', '00:30:00', '2020-01-01'), (1, 4, 'エピソード4', 'エピソード4の詳細', '00:30:00', '2020-01-01'), (1, 5, 'エピソード5', 'エピソード5の詳細', '00:30:00', '2020-01-01'), (1, 6, 'エピソード6', 'エピソード6の詳細', '00:30:00', '2020-01-01'), (1, 7, 'エピソード7', 'エピソード7の詳細', '00:30:00', '2020-01-01'), (1, 8, 'エピソード8', 'エピソード8の詳細', '00:30:00', '2020-01-01'), (1, 9, 'エピソード9', 'エピソード9の詳細', '00:30:00', '2020-01-01'), (2, 1, 'エピソード1', 'エピソード1の詳細', '00:30:00', '2020-01-01'), (2, 2, 'エピソード2', 'エピソード2の詳細', '00:30:00', '2020-01-01'), (2, 3, 'エピソード3', 'エピソード3の詳細', '00:30:00', '2020-01-01');

       INSERT INTO time_slots (time_slot_name, start_time, end_time) VALUES ('朝', '06:00:00', '11:59:59'), ('昼', '12:00:00', '17:59:59'), ('夜', '18:00:00', '23:59:59'), ('深夜', '00:00:00', '05:59:59');

       INSERT INTO channel_episodes (channel_id, episode_id, time_slot_id, broadcast_date, start_time, end_time) VALUES (1, 1, 1, '2020-01-01', '00:00:00', '00:30:00'), (1, 2, 1, '2020-01-01', '00:00:00', '00:30:00'), (1, 3, 1, '2020-01-01', '00:00:00', '00:30:00'), (1, 4, 1, '2020-01-01', '00:00:00', '00:30:00'), (1, 5, 1, '2020-01-01', '00:00:00', '00:30:00'), (1, 6, 1, '2020-01-01', '00:00:00', '00:30:00'), (1, 7, 1, '2020-01-01', '00:00:00', '00:30:00'), (1, 8, 1, '2020-01-01', '00:00:00', '00:30:00'), (1, 9, 1, '2020-01-01', '00:00:00', '00:30:00'), (1, 10, 1, '2020-01-01', '00:00:00', '00:30:00'), (1, 11, 1, '2020-01-01', '00:00:00', '00:30:00'), (1, 12, 1, '2020-01-01', '00:00:00', '00:30:00'), (1, 13, 1, '2020-01-01', '00:00:00', '00:30:00'), (1, 14, 1, '2020-01-01', '00:00:00', '00:30:00'), (1, 15, 1, '2020-01-01', '00:00:00', '00:30:00'), (1, 16, 1, '2020-01-01', '00:00:00', '00:30:00');
    ```

