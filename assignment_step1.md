#### テーブル：channels

| カラム名     | データ型      | NULL | キー    | 初期値 | AUTO INCREMENT |
|--------------|---------------|------|---------|--------|----------------|
| channel_id   | INT           |      | PRIMARY |        | YES            |
| channel_name | VARCHAR(100)  |      | UNIQUE  |        |                |

外部キー制約はありません。


#### テーブル：programs

| カラム名            | データ型      | NULL | キー    | 初期値 | AUTO INCREMENT |
|---------------------|---------------|------|---------|--------|----------------|
| program_id          | INT           |      | PRIMARY |        | YES            |
| program_title       | VARCHAR(100)  |      |         |        |                |
| program_description | VARCHAR(255)  |      |         |        |                |
| is_series           | TINYINT(1)    |      |         |        |                |

外部キー制約はありません。


#### テーブル：episodes

| カラム名            | データ型      | NULL | キー    | 初期値 | AUTO INCREMENT |
|---------------------|---------------|------|---------|--------|----------------|
| episode_id          | INT           |      | PRIMARY |        | YES            |
| program_id          | INT           |      |         |        |                |
| season_number       | INT           | YES  |         |        |                |
| episode_number      | INT           | YES  |         |        |                |
| episode_title       | VARCHAR(100)  |      |         |        |                |
| episode_description | VARCHAR(255)  |      |         |        |                |
| video_duration      | INT           |      |         |        |                |
| release_date        | DATE          |      |         |        |                |

外部キー制約：program_id に対して、programs テーブルの program_id カラムから設定



#### テーブル：genres

| カラム名          | データ型     | NULL | キー    | 初期値 | AUTO INCREMENT |
|-------------------|--------------|------|---------|--------|----------------|
| genre_id          | INT          |      | PRIMARY |        | YES            |
| genre_name        | VARCHAR(50)  |      | UNIQUE  |        |                |
| genre_description | VARCHAR(255) | YES  |         |        |                |

外部キー制約はありません。



#### テーブル：program_genres

| カラム名   | データ型 | NULL | キー                      | 初期値 | AUTO INCREMENT |
|------------|----------|------|---------------------------|--------|----------------|
| program_id | INT      |      | PRIMARY KEY          |        |                |
| genre_id   | INT      |      | PRIMARY KEY          |        |                |

外部キー制約：program_id に対して、programs テーブルの program_id カラムから設定  
外部キー制約：genre_id に対して、genres テーブルの genre_id カラムから設定


#### テーブル：broadcasts

| カラム名        | データ型  | NULL | キー    | 初期値 | AUTO INCREMENT |
|-----------------|-----------|------|---------|--------|----------------|
| broadcast_id    | INT       |      | PRIMARY |        | YES            |
| channel_id      | INT       |      |         |        |                |
| episode_id      | INT       |      |         |        |                |
| scheduled_start | DATETIME  |      |         |        |                |
| scheduled_end   | DATETIME  |      |         |        |                |
| view_count      | INT       |      |         |        |                |

外部キー制約：channel_id に対して、channels テーブルの id カラムから設定  
外部キー制約：episode_id に対して、episodes テーブルの episode_id カラムから設定