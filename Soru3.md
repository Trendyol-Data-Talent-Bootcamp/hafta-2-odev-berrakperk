# Soru 3) Bu çalışmada çıkarmak istediğimiz bilgi, günün her bir dakikası için aktif kullanıcı sayısının hesaplanması.

```SQL
create table berrak_perk.pageview_minute
as 
  select timestamp_trunc(pv.view_ts,minute) view_min,
         hll_count.init(deviceid,24) approx_dist_user
  from `dsmbootcamp.berrak_perk.pageview`
  group by 1
  order by view_min
  
create table berrak_perk.fivemin
as 
  select view_min,approx_dist_user,
        lag(view_min,0) over(order by view_min) as min1,
        lag(view_min,4) over(order by view_min) as min2,
  from  `dsmbootcamp.berrak_perk.pageview_minute` 
  order by view_min  

select active_users,view_min
from (
select hll_count.merge(approx_dist_user) active_users,view_min
from `dsmbootcamp.berrak_perk.fivemin`
where view_min between min2 and min1
group by view_min
order by view_min
)

```
