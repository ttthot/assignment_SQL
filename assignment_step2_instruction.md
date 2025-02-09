## 前提条件

- MySQLがインストール済みで起動していること
- rootユーザーでログインできること
- 本手順書で動作確認が取れているのは以下の環境です
  - mysql Ver 9.2.0 for macos15.2 on arm64 (Homebrew)

## ステップ 1: データベースの作成と選択

手順説明:
データベース "internet_tv" を作成し、使用するように選択します。

実行するSQL文

```sql
CREATE DATABASE internet_tv;
USE internet_tv;
```

確認用SQL文

```sql
SHOW DATABASES
```

## ステップ 2: channels テーブルの作成

手順説明:
channels テーブルを作成します。
このテーブルはチャンネルの名前を持ちます。

実行するSQL文

```sql
CREATE TABLE channels (
    channel_id INT AUTO_INCREMENT PRIMARY KEY,
    channel_name VARCHAR(100) NOT NULL UNIQUE
);
```

確認用SQL文

```sql
DESCRIBE channels
```

## ステップ 3: programs テーブルの作成

手順説明:
programs テーブルを作成します。
このテーブルは番組のタイトル、番組詳細、シリーズか否か（is_series）を持ちます。

実行するSQL文

```sql
CREATE TABLE programs (
    program_id INT AUTO_INCREMENT PRIMARY KEY,
    program_title VARCHAR(100) NOT NULL,
    program_description VARCHAR(255) NOT NULL,
    is_series TINYINT(1) NOT NULL
);
```

確認用SQL文

```sql
DESCRIBE programs
```

## ステップ 4: episodes テーブルの作成

手順説明:
episodes テーブルを作成します。
このテーブルはシーズン数、エピソード数、タイトル、エピソード詳細、動画時間、公開日を持ちます。

実行するSQL文

```sql
CREATE TABLE episodes (
    episode_id INT AUTO_INCREMENT PRIMARY KEY,
    program_id INT NOT NULL,
    season_number INT,
    episode_number INT,
    episode_title VARCHAR(100) NOT NULL,
    episode_description VARCHAR(255) NOT NULL,
    video_duration INT NOT NULL,
    release_date DATE NOT NULL,
    FOREIGN KEY (program_id) REFERENCES programs(program_id)
);
```

確認用SQL文

```sql
DESCRIBE episodes
```

## ステップ 5: genres テーブルの作成

手順説明:
genres テーブルを作成します。
このテーブルは番組のジャンルを持ちます。

実行するSQL文

```sql
CREATE TABLE genres (
    genre_id INT AUTO_INCREMENT PRIMARY KEY,
    genre_name VARCHAR(50) NOT NULL UNIQUE,
    genre_description VARCHAR(255)
);
```

確認用SQL文

```sql
DESCRIBE genres
```

## ステップ 6: program_genres テーブルの作成

手順説明:
program_genres テーブルを作成します。
このテーブルは、各番組とジャンルの多対多の関係を持ちます。

実行するSQL文

```sql
CREATE TABLE program_genres (
    program_id INT NOT NULL,
    genre_id INT NOT NULL,
    PRIMARY KEY (program_id, genre_id),
    FOREIGN KEY (program_id) REFERENCES programs(program_id),
    FOREIGN KEY (genre_id) REFERENCES genres(genre_id)
);
```

確認用SQL文

```sql
DESCRIBE program_genres
```

## ステップ 7: broadcasts テーブルの作成

手順説明:
broadcasts テーブルを作成します。
このテーブルは、各チャンネルの放送スケジュールと視聴数を持ちます。

実行するSQL文

```sql
CREATE TABLE broadcasts (
    broadcast_id INT AUTO_INCREMENT PRIMARY KEY,
    channel_id INT NOT NULL,
    episode_id INT NOT NULL,
    scheduled_start DATETIME NOT NULL,
    scheduled_end DATETIME NOT NULL,
    view_count INT NOT NULL,
    FOREIGN KEY (channel_id) REFERENCES channels(channel_id),
    FOREIGN KEY (episode_id) REFERENCES episodes(episode_id)
);
```

確認用SQL文

```sql
DESCRIBE broadcasts
```

## ステップ 8: channels テーブルへのサンプルデータ挿入

実行するSQL文

```sql
INSERT INTO channels (channel_name) VALUES 
('Drama1'),
('Drama2'),
('Anime1'),
('Anime2'),
('Sports'),
('Pets');
```

確認用SQL文

```sql
SELECT * FROM channels
```

## ステップ 9: programs テーブルへのサンプルデータ挿入

実行するSQL文

```sql
INSERT INTO programs (program_title, program_description, is_series) VALUES
('City Lights', 'A compelling drama about life in the big city', 1),
('Laugh Factory', 'Stand-up comedy from the hottest new comedians', 0),
('Detective Smith', 'Follow Detective Smith as he solves complex crimes', 1),
('Galaxy Quest', 'Join the crew of the starship Adventurer on their intergalactic journey', 1),
('Wild Planet', 'Explore the wonders of nature in this breathtaking documentary series', 1),
('Fun Time', 'Educational and entertaining content for children of all ages', 0),
('Master Chef Challenge', 'Amateur chefs compete for the title of Master Chef', 0),
('Unsolved History', 'Delve into historical mysteries and unsolved cases', 1),
('Tech Today', 'Stay up-to-date with the latest in technology and innovation', 0),
('Sports Center', 'Live sports coverage and in-depth analysis', 0),
('Hollywood Insider', 'Get the latest scoop on celebrity news and movie reviews', 0),
('Cartoon Craziness', 'Hilarious animated series for the whole family', 1),
('Medical Miracles', 'Showcasing groundbreaking procedures in modern medicine', 1),
('Law & Order', 'Gripping courtroom drama series', 1),
('Survival Island', 'Reality show where contestants compete on a deserted island', 0),
('Music Box', 'Live performances from top artists across all genres', 0),
('Pet Paradise', 'All about pet care and adorable animals', 0),
('Fitness Frenzy', 'Workout programs and health tips', 0),
('Travel Diaries', 'Explore world destinations and cultures', 1),
('Anime Showcase', 'Featuring the best in Japanese animation', 1);
```

確認用SQL文

```sql
SELECT * FROM programs
```

## ステップ 10: episodes テーブルへのサンプルデータ挿入

実行するSQL文

```sql
INSERT INTO episodes (program_id, season_number, episode_number, episode_title, episode_description, video_duration, release_date)
SELECT 
    CEIL(seq / 5) AS program_id,
    FLOOR(1 + (seq - 1) / 10) AS season_number,
    ((seq - 1) % 5) + 1 AS episode_number,
    CONCAT('Episode ', ((seq - 1) % 5) + 1, ': ', 
           ELT(FLOOR(1 + RAND() * 5), 'New Beginnings', 'Unexpected Turn', 'The Big Reveal')) AS episode_title,
    CONCAT('In this episode, ', 
           ELT(FLOOR(1 + RAND() * 5), 'our characters face a new challenge')) AS episode_description,
    FLOOR(1800 + RAND() * 1800) AS video_duration,
    DATE_ADD('2025-02-03', INTERVAL (seq - 1) DAY) AS release_date
FROM 
    (SELECT seq FROM seq_100);
```

確認用SQL文

```sql
SELECT * FROM episodes
```


