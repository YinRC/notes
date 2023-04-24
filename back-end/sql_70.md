# 1. [游戏玩法分析 I](https://leetcode.cn/problems/game-play-analysis-i/)

```sql
select `player_id`, min(`event_date`) as `first_login`
from Activity
group by `player_id`
```



# 2. [游戏玩法分析 II](https://leetcode.cn/problems/game-play-analysis-ii/)

```sql
select `player_id`, `device_id`
from Activity
where (`player_id`, `event_date`) in (
    select `player_id`, min(`event_date`)
    from Activity
    group by `player_id`
)
```



# 3. 