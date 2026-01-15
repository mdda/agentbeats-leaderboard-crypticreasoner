

### Reformat leaderboard submission `json` to be valid ...
### in Green Agent 

* NB: Make the packages for green and purple agents *public* !

```json
[
  {
    "name": "Overall Performance",
    "query": "SELECT
      id,
      SUM(win) AS Correct,
      SUM(loss) AS Incorrect
    FROM (
      SELECT
        t.participants.crypticreasoner_solver AS id,
        CASE WHEN r.result.score>0 THEN 1 ELSE 0 END AS win,
        CASE WHEN r.result.score=0 THEN 1 ELSE 0 END AS loss
      FROM results t
      CROSS JOIN UNNEST(t.results) AS r(result)
    )
    GROUP BY id
    ORDER BY correct DESC, incorrect ASC, id;"
  }
]
```

```json
[
  {
    "name": "Overall Performance",
    "query": "SELECT id, SUM(win) AS Correct, SUM(loss) AS Incorrect FROM ( SELECT t.participants.crypticreasoner_solver AS id, CASE WHEN r.result.score>0 THEN 1 ELSE 0 END AS win, CASE WHEN r.result.score=0 THEN 1 ELSE 0 END AS loss FROM results t CROSS JOIN UNNEST(t.results) AS r(result) ) GROUP BY id ORDER BY correct DESC, incorrect ASC, id;"
  }
]
```

 