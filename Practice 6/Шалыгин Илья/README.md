Для Олимпийских игр 2004 года сгенерируйте список, содержащий годы, в которые родились игроки, количество игроков, родившихся в каждый из этих лет, которые выиграли по крайней мере одну золотую медаль, и количество золотых медалей, завоеванных игроками, родившимися в этом году.

```sql
SELECT EXTRACT(year FROM birthdate) AS year, COUNT(*) AS players_amount
FROM players
         JOIN
     (
         SELECT player_id
         FROM results
                  JOIN
              (
                  SELECT event_id
                  FROM events
                  WHERE olympic_id = 'ATH2004'
              )
                  AS event2004 ON
                  results.event_id = event2004.event_id
         WHERE medal LIKE '%GOLD%'
         GROUP BY player_id
         HAVING COUNT(*) > 1)
         AS gold_players
     ON players.player_id = gold_players.player_id
GROUP BY EXTRACT(year FROM players.birthdate)
ORDER BY EXTRACT(year FROM players.birthdate);

```

Перечислите все индивидуальные (не групповые) соревнования, в которых была ничья в счете, и два или более игрока выиграли золотую медаль.

```sql
```

Найдите всех игроков, которые выиграли хотя бы одну медаль (GOLD, SILVER и BRONZE) на одной Олимпиаде. (player-name, 
olympic-id).

```sql
```

В какой стране был наибольший процент игроков (из перечисленных в наборе данных), чьи имена начинались с гласной?

```sql
```

Для Олимпийских игр 2000 года найдите 5 стран с минимальным соотношением количества групповых медалей к численности населения.

```sql
```
