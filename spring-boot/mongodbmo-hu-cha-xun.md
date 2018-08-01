# mongodb模糊查询

1、对查询字符串转义，以处理输入为regex的特定字符（同时允许输入以数字开头的模糊查询）：

```
//regex对输入特殊字符转义
	String escapeExprSpecialWord(String keyword) {  
	      if (StringUtils.isNotBlank(keyword)) {  
	         String[] fbsArr = { "\\", "$", "(", ")", "*", "+", ".", "[", "]", "?", "^", "{", "}", "|" };  
	         for (String key : fbsArr) {  
	             if (keyword.contains(key)) {  
	                 keyword = keyword.replace(key, "\\" + key);  
	            }  
	         }  
	     }  
	     return keyword;  
	 }
```

2、使用Criteria

```
//m.get("parma1")为前端传入参数param1
Pattern pattern = Pattern.compile(".*?" + escapeExprSpecialWord(m.get("param1").toString()) + ".*"); 
Criteria criteria=Criteria.where("param1").regex(pattern);
pattern = Pattern.compile(".*?" + escapeExprSpecialWord(m.get("param2").toString()) + ".*"); 
criteria.and("param2").regex(pattern);

Query query = new Query(criteria);		
// mongoTemplate.count计算总数
long total = mongoTemplate.count(query, Ticket.class);
List<MyEntity> items = mongoTemplate.find(query.with(pageable), MyEntity.class);
return new PageImpl(items, pageable, total);
```

3、对于子查询elemmatch的处理

一直未成功，采用以下方式解决：

* 映射类中按collection结构设置，对子集不要设置为map而是设置子类
* 查询中直接使用a.b即可查询

例：collection结构为{id:XXX,content:XXX,project:{id:XXX,name:XXX}}

```
@Document(collection="Ticket")
@JsonInclude(value = Include.NON_NULL)
@JsonPropertyOrder(alphabetic = true)
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@ToString(callSuper = true, includeFieldNames = true)
@ApiModel(value = "ticket", description = "工单管理")
public class Ticket extends BaseEntity implements java.io.Serializable {
	@Id	
	private String id;
	private String content;
	private Project project;  //定义Location以匹配collection中的location字段
……
}

@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
class Location{
	long id;
	String name;	
}

//查询project.id=1的记录
Criteria.where("project.id").is(1);
//查询project.name包含武汉的记录 
Criteria.where("project.name").regex(".*?武汉.*"); //如从前端获取有特定字符的查询条件或以数字开头的查询条件请使用前面说的转义方法处理



```



