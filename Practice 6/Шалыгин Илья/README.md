Для Олимпийских игр 2004 года сгенерируйте список, содержащий годы, в которые родились игроки, количество игроков, родившихся в каждый из этих лет, которые выиграли по крайней мере одну золотую медаль, и количество золотых медалей, завоеванных игроками, родившимися в этом году.

```sql
SELECT EXTRACT(year FROM players.birthdate) as birth_year,
       COUNT(players.player_id) as players,
       COUNT(results.medal) as medals FROM players
JOIN results
ON players.player_id = results.player_id
JOIN events
ON results.event_id = events.event_id
WHERE events.olympic_id LIKE '%ATH2004%'
AND results.medal LIKE '%GOLD%'
GROUP BY EXTRACT(year FROM players.birthdate);
```

Перечислите все индивидуальные (не групповые) соревнования, в которых была ничья в счете, и два или более игрока выиграли золотую медаль.

```sql
SELECT event_id FROM results
WHERE medal LIKE '%GOLD%'
GROUP BY event_id
HAVING COUNT(*) > 2;
```

Найдите всех игроков, которые выиграли хотя бы одну медаль (GOLD, SILVER и BRONZE) на одной Олимпиаде. (player-name, 
olympic-id).

```sql
SELECT DISTINCT players.name, events.olympic_id FROM players
JOIN results
ON players.player_id = results.player_id
JOIN events
ON results.event_id = events.event_id;
```

В какой стране был наибольший процент игроков (из перечисленных в наборе данных), чьи имена начинались с гласной?

```sql
SELECT countries.name
FROM countries
         JOIN players
              ON players.country_id = countries.country_id
WHERE SUBSTRING(players.name, 1, 1) in ('A', 'E', 'I', 'O', 'U')
GROUP BY countries.name
ORDER BY COUNT(*) DESC
LIMIT 1;
```

Для Олимпийских игр 2000 года найдите 5 стран с минимальным соотношением количества групповых медалей к численности населения.

```sql
SELECT countries.name, COUNT(*) as medals, countries.population FROM countries
JOIN players
ON countries.country_id = players.country_id
JOIN results
ON players.player_id = results.player_id
JOIN events
ON results.event_id = events.event_id
WHERE events.is_team_event = 1
AND events.olympic_id LIKE '%SYD2000%'
GROUP BY countries.name, countries.population
ORDER BY COUNT(*) / countries.population
LIMIT 5;
```
