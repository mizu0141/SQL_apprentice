### 1.よく見られているエピソード
```sql
SELECT e.episode_name, SUM(ce.views_count) as total_views
FROM episodes AS e
JOIN channel_episodes AS ce
ON e.episode_id = ce.episode_id
ORDER BY views_count DESC LIMIT 3;
```

### 2.よく見られているエピソードの番組情報やシーズン情報
```sql
SELECT p.program_name, s.season_number, e.episode_number, e.episode_name, SUM(ce.views_count) AS total_views
FROM episodes AS e
JOIN seasons AS s
ON e.season_id = s.season_id
JOIN programs AS p
ON s.program_id = p.program_id
JOIN channel_episodes AS ce
ON e.episode_id = ce.episode_id
ORDER BY views_count DESC LIMIT 3;
```

### 3.本日放送される全ての番組に対して、チャンネル名、放送開始時刻、放送終了時刻、シーズン数、エピソード数、エピソードタイトル、エピソード詳細を表示する
```sql
SELECT  c.channel_name, CONTACT(ce.broadcast_date, ' ', ce.start_time) AS start_time,
        CONTACT ce.broadcast_date, ' ', ce.end_time) AS end_time, season_number, episode_number, episode_name,
        episode_detail
FROM channels AS c
JOIN channel_episodes AS ce
ON c.channel_id = ce.channel_id
JOIN episodes AS e
ON ce.episode_id = e.episode_id
JOIN seasons AS s
ON e.season_id = s.season_id
WHERE DATE(broadcast_date) = DATE(NOW())
ORDER BY channel_name;
```
- ORDER BY で、SELECT で指定した項目の中から並び替えたい項目を指定する
### 4.ドラマのチャンネルに対して、放送開始時刻、放送終了時刻、シーズン数、エピソード数、エピソードタイトル、エピソード詳細を本日から一週間分表示する
```sql
SELECT CONTACT(ce.broadcast_date, ' ', ce.start_time) AS start_time, CONTACT ce.broadcast_date, ' ', ce.end_time) AS
       end_time, s.season_number, e.episode_number, e.episode_name, e.episode_detail
FROM channels AS c
JOIN channel_episodes AS ce
ON c.channel_id = ce.channel_id
JOIN episodes AS e
ON ce.episode_id = e.episode_id
JOIN seasons AS s
ON e.season_id = s.season_id
WHERE DATE(broadcast_date) BETWEEN DATE(NOW()) AND DATE_ADD(DATE(NOW()), INTERVAL 7 DAY)
AND c.channel_name = 'ドラマ';
```

### 5.直近一週間で最も観られた番組
```sql
SELECT  p.program_name, SUM(ce.views_count) AS total_views
FROM episodes AS e
JOIN seasons AS s
ON e.season_id = s.season_id
JOIN programs AS p
ON s.program_id = p.program_id
JOIN channel_episodes AS ce
ON e.episode_id = ce.episode_id
WHERE ce.broadcast_date BETWEEN DATE_SUB(CURDATE(), INTERVAL 7 DAY) AND CURDATE()
GROUP BY p.program_id
ORDER BY total_views DESC LIMIT 2;
```
### 6.ジャンルごとに視聴数トップの番組の、ジャンル名、番組タイトル、エピソード平均視聴数
```sql
SELECT  g.genre_name, p.program_name, AVG(ce.views_count) AS average_views
FROM genres AS g
JOIN program_genres AS pg
ON g.genre_id = pg.genre_id
JOIN programs AS p
ON pg.program_id = p.program_id
JOIN seasons AS s
ON p.program_id = s.program_id
JOIN episodes AS e
ON s.season_id = e.season_id
JOIN channel_episodes AS ce
ON e.episode_id = ce.episode_id
GROUP BY p.program_id ,g.genre_id
ORDER BY average_views DESC ;
```
