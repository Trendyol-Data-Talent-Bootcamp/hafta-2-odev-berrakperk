# Soru 1) 1980’den itibaren spor grubu bazında en çok madalya alan 1. 3. 5. ülkeyi bulalım.

```SQL
with medal as
(
  select
    country,
    sport,
    count(*) as medal_count,
    dense_rank() over(partition by sport order by count(*) desc) as dense_rank,
  from `dsmbootcamp.berrak_perk.summer_medals`
  where year >= 1980
  group by sport,country
)
select 
    dense_rank,
    country,
    sport 
from medal
where dense_rank=1 or dense_rank=3 or dense_rank=5;

```
