# Soru 2) 1980’den itibaren herhangi bir spor grubunda üst üste 3 veya daha fazla madalya almış atletleri bulalım.

```SQL
with athletes as
(
  select
  athlete,
  sport,
  year, 
  count(*) over(partition by athlete order by year rows between unbounded preceding and unbounded following) as year_count,
  first_value(year) over(partition by athlete order by year) as first,
  lead(year,1) over(partition by athlete order by year) as second,
  lead(year,2) over(partition by athlete order by year) as third,
  from `dsmbootcamp.berrak_perk.summer_medals`
  where year >= 1980 
)
select
athlete,
sport,
first,second,third
from athletes
where year_count >= 3 and third - second = 4 and second - first = 4

```
