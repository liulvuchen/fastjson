# fastjson
fastjson的基本使用

-->#1//fastjson序列化单个对象 与反序列化 
Employee e = new Employee("001", "张三", 23, new Date()); 
//序列化 
String jsonStr = JSON.toJSONString(e); 
System.out.println(jsonStr); 
//反序列化 
Employee emp = JSON.parseObject(jsonStr, Employee.class); 
System.out.println(emp.getName()); 

-->#2//fastjson序列化list集合 与反序列化 
Employee e = new Employee("001", "张三", 23, new Date()); 
Employee e2 = new Employee("002", "李四", 29, new Date()); 
List<Employee> emps = new ArrayList<Employee>(); 
emps.add(e); 
emps.add(e2); 
//fastjson序列化list， 返回来的是一个json数组，由[]包含两个json 
String jsonArryStr = JSON.toJSONString(emps); 
System.out.println(jsonArryStr); 
// //反序列化 
//法一 
// List<Employee> empList = JSON.parseObject(jsonArryStr, new TypeReference<List<Employee>>(){} ); 
//法二 
List<Employee> empList = JSON.parseArray(jsonArryStr,Employee.class); 
for (Employee employee : empList) { 
    System.out.println(employee.getName()); 
    System.out.println(employee.getBirthDay()); 
} 

-->#3//fastjson序列化复杂对象 与反序列化 
Employee e = new Employee("001", "张三", 23, new Date()); 
Employee e2 = new Employee("002", "李四", 29, new Date()); 

List<Employee> emps = new ArrayList<Employee>(); 
emps.add(e); 
emps.add(e2); 

Dept dept = new Dept("d001", "研发部", emps); 

//序列化 
String jsonStr = JSON.toJSONString(dept); 
System.out.println(jsonStr); 

//反序列化 
Dept d = JSON.parseObject(jsonStr, Dept.class); 
System.out.println(d.getName()); 

-->#4//json转map 
//法一 
Map<String, Object> map1 = JSON.parseObject(jsonStr);//返回JSONObject，JSONObject实现Map<String, Object>接口 
//法二 
// Map<String, Object> map1 = (Map<String, Object>)JSON.parse(jsonStr); 
for (String key : map1.keySet()) { 
    System.out.println(key + ":" + map1.get(key)); 
} 

-->#5//fastjson 的 JSONObject的使用 
Employee e = new Employee("001", "张三", 23, new Date()); 

//序列化 
String jsonStr = JSON.toJSONString(e); 
System.out.println(jsonStr); 
//反序列化 (可以和#1比较)  
JSONObject emp = JSON.parseObject(jsonStr, JSONObject.class); 
System.out.println(emp); 
System.out.println(emp.getString("name")); 
//再放一个Employee不存在的字段 
emp.put("salary", "8000"); 
System.out.println(emp.toJSONString()); 
System.out.println(emp.get("salary")); 

-->#6//fastjson序列化字符串 
List<String> strs = new ArrayList<String>(); 
strs.add("hello"); 
strs.add("world"); 
strs.add("banana"); 
//序列化 
String jsonStr = JSON.toJSONString(strs); 
System.out.println(jsonStr); 

//反序列化 
List<String> strList = JSON.parseObject(jsonStr, new TypeReference<List<String>>(){} ); 
// List<String> strList = JSON.parseArray(jsonStr, String.class);//等同于上一句 
for (String str : strList) { 
    System.out.println(str); 
} 
-->#7//fastjson过滤字段 
Employee e = new Employee("001", "张三", 23, new Date()); 
Employee e2 = new Employee("002", "李四", 29, new Date()); 

List<Employee> emps = new ArrayList<Employee>(); 
emps.add(e); 
emps.add(e2); 

//构造过滤器 
SimplePropertyPreFilter filter = new SimplePropertyPreFilter(Employee.class, "id", "age"); 
String jsonStr =JSON.toJSONString(emps, filter); 
System.out.println(jsonStr); 

-->#8//fastjson 日期处理 
Date date = new Date(); 
String dateStr = JSON.toJSONString(date); 
System.out.println(dateStr); 
String dateStr2 = JSON.toJSONStringWithDateFormat(date, "yyyy-MM-dd HH:mm:ss"); 
System.out.println(dateStr2); 

//序列化实体 
Employee emp = new Employee("001", "张三", 23, new Date()); 

//法一 
String empStr = JSON.toJSONStringWithDateFormat(emp, "yyyy-MM-dd HH:mm:ss"); 
System.out.println(empStr); 

//法二 
String empStr2 = JSON.toJSONString(emp, SerializerFeature.WriteDateUseDateFormat); 
System.out.println(empStr2); 
//法三 
SerializeConfig config = new SerializeConfig(); 
config.put(Date.class, new SimpleDateFormatSerializer("yyyy年MM月dd日 HH时mm分ss秒")); 
String empStr3 = JSON.toJSONString(emp, config); 
System.out.println(empStr3);  

-->#9//fastjson 去掉值的双引号 实现JSONAware接口 
//见同级目录的Function.java 

-->#10//fastjson 注解形式  (别名命名, 过滤字段, 日期格式) 
Student stu = new Student("001", "张三", 23, new Date()); 
String jsonStr = JSON.toJSONString(stu); 
System.out.println(jsonStr); 
