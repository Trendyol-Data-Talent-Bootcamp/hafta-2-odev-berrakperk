# soru 4)

```SQL
merge `dsmbootcamp.berrak_perk.content_category`  as target
using `dsmbootcamp.berrak_perk.content_category_20201222_00_59` as source
on target.id = source.id
when matched and target.cdc_date != source.cdc_date then
  update set target.cdc_date = source.cdc_date
when not matched by source then
  update set target.is_deleted = true
when not matched by target then
  insert(cdc_date,is_deleted,id,category)
  values(source.cdc_date,source.is_deleted,source.id,source.category)



select count(*) as count_not_equal from
(select farm_fingerprint(to_json_string(t1)) as hash1, 
farm_fingerprint(to_json_string(t2)) as hash2
from `dsmbootcamp.berrak_perk.content_category_target` t1
join `dsmbootcamp.berrak_perk.content_category` t2
on farm_fingerprint(to_json_string(t1)) = farm_fingerprint(to_json_string(t2))) 
where hash1 != hash2;
 ```
 

