# 基本运算

## join

### natural join 和 join using

natural join 要求两个关系相同的属性就要取值相同，但是join using 就只要求 using 指定的属性取值相同

```sql
select name, title
from (student natural join takes) join course using (course_id);
```