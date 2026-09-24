title: java中的foreach遍历
tags:
  - Java
categories: []
date: 2016-01-12 18:32:00
---
``` java
		List<Person> persons = new ArrayList<Person>();
		persons.add(new Person("name1", 11));
		persons.add(new Person("name2", 12));
		persons.add(new Person("name3", 13));
		
		for (Person person : persons) {
			person.setAge(person.getAge()+5);
		}
		
		for (Person person : persons) {
			System.out.println(person);
			System.out.println(person == persons.get(0));
		}
```
<pre>
Person [name=name1, age=16]
true
Person [name=name2, age=17]
false
Person [name=name3, age=18]
false
</pre>
对于在for遍历中的每个person对象,其实是persons中的一个对象,如果改变person的值,也就造成了persons中对象的改变.