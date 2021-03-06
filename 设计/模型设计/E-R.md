## 通用实体属性

权限：数据表中记录单位、用户  
状态：已经上报过的不可以再上报  
数据可见性：状态可见，权限可见  
数据顺序：UpdateTime是非常必要的，究竟是CeateTime还是UpdateTime？time is null  


## 实体关系设计

- 批量插入，因为一对多/多对多的原因，导致插入的时候，需要循环遍历  
  要么采取补齐主表ID的方式，要么采取只循环子表的方式  
  https://blog.csdn.net/m0_37981235/article/details/79131493 特别注意：mysql默认接受sql的大小是1048576(1M)  
- 根据主表ID，删除关联表。
- 多选框的数据存储
- 复选条件查询，且带有层次关系的查询
- 物理删除/逻辑删除
  https://stackoverflow.com/questions/378331/physical-vs-logical-soft-delete-of-database-record  Physical vs. logical / soft delete of database record?

### 更新

对于关联更新，要十分小心，关联表最好不要用ID也不要建ID，因为关联表的ID会经常变化。  
关联表不建ID字段，而是创建联合主键，最简单。1:N的关联插入  
如果是有type这种情况呢，比如一张表是区域表，而另外的业务表需要指定区域。第一感觉是加字段区分type  

### 删除

关联关系的稳定性，比如网站和漏洞的关联关系是强关联，和单位的关联关系是弱关联。  
用户可以查看到单位下面的所有网站，所有任务。将来用户如果发生调岗、升迁，那么这些数据理应看不到，所以要么不存储用户ID，或者查询要做外关联。  

尽量把判断逻辑放在SQL之外，除非需要判断是正常情况属于业务需要  