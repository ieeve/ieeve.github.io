## 大数据量对比FirstOrDefault性能差的解放方法
使用Dictionary 替代
```
var summaryList=new DbList(){};
//原先的方式
 foreach (var map in MapperEntity)
 {
  var exist = summaryList.FirstOrDefault(s => s.WORK_NO == map.WORK_NO); //性能差
     if (exist != null)
     {
         map.createtime = DateTime.Now;
         map.createuid = "SYS";
         ...
     }
 }

```

```
//改进后
//* 替代FirstOrDefault,大幅提高性能。第一个循环就是所有的数据用自己的ID构建字典
Dictionary<string, Pro_box_summaryEntity> dict = new Dictionary<string, Pro_box_summaryEntity>();
foreach (var item in summaryList)
{
    dict.Add(item.ID, item);
}

foreach (var map in MapperEntity)
 {
     if (dict.ContainsKey(map.ID)) //大幅提高性能
     {
         map.createtime = DateTime.Now;
         map.createuid = "SYS";
         ...
     }
 }
```
