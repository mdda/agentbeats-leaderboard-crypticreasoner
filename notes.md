

### Reformat leaderboard submission `json` to be valid ...
### in Green Agent 

* NB: Make the packages for green and purple agents *public* !


### DuckDB logic for leadboard score summaries

* Sadly, this one aggregates across multiple runs

```json
[
  {
    "name": "Overall Performance",
    "query": "SELECT list_extract(map_values(CAST(participants AS MAP(VARCHAR, VARCHAR))), 1) AS id, ROUND(AVG(task_result.score) * 100, 1) AS \"Correct Rate\", COUNT(*) AS \"NumTasks\" FROM results, UNNEST(results) AS t1(result_set), UNNEST(result_set.results) AS t2(task_result) GROUP BY id ORDER BY \"Correct Rate\" DESC"
  }
]
```

* This one outputs a separate row for each run (=correct)


```json
[
  {
    "name": "Overall Performance",
    "query": "SELECT to_json(participants) ->> (json_keys(to_json(participants))[1]) AS id, round((list_sum(flatten(list_transform(results, run -> list_transform(run.results, task -> task.score))))::DOUBLE / NULLIF(len(flatten(list_transform(results, run -> list_transform(run.results, task -> task.score)))), 0)) * 100, 1) AS \"Correct Rate\", len(flatten(list_transform(results, run -> list_transform(run.results, task -> task.score)))) AS NumTasks FROM results"
  }
]
```
