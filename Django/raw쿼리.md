
- django에서는 raw query를 사용할 수 있는 두 가지 방법을 
- Manage.raw()를 사용하면 원시 SQL 쿼리를 실행하고, django.db.models.query.RawQuerySet 인스턴스 반환.
- 이 Raw QuerySet 인스턴스는 일반 쿼리셋처럼 반복해서 객체 인스턴스를 제공할 수 있다.



- raw()는 인덱스를 제공한다. 그러나 인덱싱과 슬라이싱은 데이터베이스 레벨에서 실행되지 않는다.
- 만약 데이터베이스에 


### 주의사항
- raw 쿼리 혹은 quote placeholder을 사용할 때 string formatting을 사용하지 마라
(Do not use string formatting on raw queries or quote placeholders in your SQL strings!)

ex)
```
query = 'SELECT * FROM myapp_person WHERE last_name = %s' % lname
Person.objects.raw(query)
```

```
query = "SELECT * FROM myapp_person WHERE last_name = '%s'"
```



