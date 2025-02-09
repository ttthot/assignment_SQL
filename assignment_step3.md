# 課題:インターネットTVステップ3

1. 
```sql
SELECT bc.view_count, ep.episode_title
FROM broadcasts bc 
LEFT OUTER JOIN episodes ep ON bc.episode_id = ep.episode_id
ORDER BY bc.view_count DESC
LIMIT 3;
```

2. 
```sql
SELECT prg.program_title, ep.season_number, ep.episode_number, ep.episode_title, bc.view_count
FROM broadcasts bc
LEFT OUTER JOIN episodes ep ON bc.episode_id = ep.episode_id
LEFT OUTER JOIN programs prg ON ep.program_id = prg.program_id 
ORDER BY bc.view_count DESC
LIMIT 3;
```

3. 
```sql
SELECT ch.channel_name, bc.scheduled_start, bc.scheduled_end, ep.season_number, ep.episode_number, episode_title, ep.episode_description,  bc.view_count
FROM broadcasts bc 
LEFT OUTER JOIN channels ch ON bc.channel_id = ch.channel_id
LEFT OUTER JOIN episodes ep ON bc.episode_id = ep.episode_id
WHERE bc.scheduled_start 
LIKE '2025-02-10%' 
```

4. 
```sql
SELECT ch.channel_name, bc.scheduled_start, bc.scheduled_end, ep.season_number, ep.episode_number, ep.episode_title, ep.episode_description
FROM broadcasts bc
LEFT OUTER JOIN channels ch ON bc.channel_id = ch.channel_id
LEFT OUTER JOIN episodes ep ON bc.episode_id = ep.episode_id
WHERE (bc.scheduled_start BETWEEN '2025-02-10 00:00:00' AND '2025-02-16 23:59:59')
AND ch.channel_name = 'drama1'
ORDER BY bc.scheduled_start
```

5. 
```sql
SELECT pg.program_title, SUM(bc.view_count) 
FROM broadcasts bc 
LEFT OUTER JOIN episodes ep ON bc.episode_id = ep.episode_id
LEFT OUTER JOIN programs pg ON ep.program_id = pg.program_id
WHERE (bc.scheduled_start 
BETWEEN '2025-02-04 00:00:00' AND '2025-02-10 23:59:59')
GROUP BY pg.program_title
ORDER BY SUM(bc.view_count) DESC
LIMIT 2;
```

6. 
```sql
SELECT gn.genre_name, pg.program_title, AVG(bc.view_count)
FROM broadcasts bc
LEFT OUTER JOIN episodes ep ON bc.episode_id = ep.episode_id
LEFT OUTER JOIN programs pg ON ep.program_id = pg.program_id
LEFT OUTER JOIN program_genres pge ON ep.program_id = pge.program_id
LEFT OUTER JOIN genres gn ON pge.genre_id = gn.genre_id
GROUP BY gn.genre_name, pg.program_title;
```