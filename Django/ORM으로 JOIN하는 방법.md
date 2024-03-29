## span relationships
- Django offers a powerful and intuitive way to “follow” relationships in lookups
- taking care of the SQL JOINs for you automatically, behind the scenes. 
- To span a relationship, just use the field name of related fields across models, separated by double underscores, until you get to the field you want.


```python
class Blog(models.Model):
    name = models.CharField(max_length=100)
    tagline = models.TextField()

    def __str__(self):
        return self.name

class Entry(models.Model):
    blog = models.ForeignKey(Blog, on_delete=models.CASCADE)
    headline = models.CharField(max_length=255)
    body_text = models.TextField()
    pub_date = models.DateField()
    mod_date = models.DateField()
    authors = models.ManyToManyField(Author)
    n_comments = models.IntegerField()
    n_pingbacks = models.IntegerField()
    rating = models.IntegerField()
```

- Blog에서 Entry로 접근
```python
    Blog.objects.filter(entry__authors__name='Lennon')
    Blog.objects.filter(entry__headline__contains='Lennon')
```

## 테이블이 외래키로 연결되어 있을 경우 

```python
# models.py 

class Company(models.Model):    
    group = models.ForeignKey(Group, blank=True, null=True, on_delete=models.SET_NULL, related_name="company")

class Group(models.Model):
   name = models.CharField()
```

```python
Company.objects.filter(pk=1).values('group__name')
```

이런 식으로, 굳이 select related 안써도 연결할 수 있다. (참조: queryset filter)
https://docs.djangoproject.com/en/2.1/topics/db/queries/#lookups-that-span-relationships


## select_related

- 쿼리를 실행했을 때에 지정된 외래 키의 개체도 함께 가져온다. 
- 이를 통해 DB에 접근하는 횟수를 줄일 수 있다. 
- 사용 예에 대해서 살펴보자면, 첫 번째 코드를 두 번째로 코드로 변경할 수 있다.

#### case1
```python
# DB에 접근
e = Entry.objects.get(id=5)
```

```python
# 아직 DB를 건들이고 있다.
b = e.blog
``` 

#### case2
```python
# DB에 접근
e = Entry.objects.select_related('blog').get(id=5)
```

```python
# 미리 DB에서 얻어낸 객체를 참고하므로 DB를 계속해서 접근하지 않는다.
b = e.blog
```


## Raw 쿼리

- RAW 쿼리셋 역시 ORM 영역임.
 
#### Django는 원시 SQL 쿼리를 수행하는 두 가지 방법을 제공한다. 
1. Manager.raw()를 사용하여 원시 쿼리(raw queries)를 수행하고, 모델 인스턴스를 반환. 
2. 모델 레이어를 완전히 피하고 사용자 정의 SQL을 직접 실행 

### RawQuerySet은 NativeSQL이 아니다
#### raw() 메서드에 대한 이해

- Django는 raw()라는 메서드를 제공한다
- 이 함수는 RawQuerySet을 반환하는데
raw_queryset  = User.objects.raw(
         raw_query='select * from auth_user where id=%(user_id)s',
         params={
              'user_id': 1, 
         },
)
type(raw_queryset) # RawQuerySet

- .raw() 메서드를 호출하면 QuerySet은 RawQuerySet으로 교체된다.

#### RawQuerySet은 완전 NativeSQL이 아니다.
- raw()메서드를 사용한다고 해도 **아직 QuerySet의 관리를 받는다.**
- RawQuerySet과 QuerySet의 차이점은
**메인쿼리를 NativeSQL로 작성한다는 점 한가지 뿐**이다. (-> 확실합니까?) 
그 이외에는 차이가 없다.
- 따라서 아래와 같은 QuerySet 작성이 가능하다

```python
raw_queryset = (User.objects
                .raw('select * from auth_user where id=1')
                .prefetch_related('user_permissions')
)
```
메인쿼리만 NativeSQL로 작성할 뿐이다.

추가 쿼리셋인 .prefetch_related() 와 Prefetch() 사용은 자유롭다.
같은 이유에서 .raw() 사용시 아래 메서드들은 사용 할 수 없다.

```
.select_related() [사용불가] # 메인쿼리의 JOIN 옵션을 주는 메서드
FilteredRelation()[사용불가] # JOIN이 안되니 ON절 제어 옵션도 당연히 불가
.annotate()       [사용불가] # 메인쿼리에 AS 옵션을 주는 메서드
.order_by()       [사용불가] # 메인쿼리에 order by 옵션 주는 메서드
.extra()          [사용불가] # 메인쿼리에 sql을 추가 반영하는 메서드
[:10]...[:2]...   [사용불가] # 메인쿼리에 limit 옵션을 걸 수 없다.
# 해당 내용들은 NativeSQL로 작성해줘야한다.

```

![image](https://user-images.githubusercontent.com/15938354/136784601-f72d26ee-3915-4165-8ded-058752cc9113.png)

주의: RawQuerySet 사용시 애트리뷰트 이름들과
모델의 프로퍼티 이름들이 반드시 매칭되어야 한다.
만약 매칭되지 않으면 해당 프로퍼티가 비어버리게되고
그러면 RawQuerySet은 그 값을 찾기위해 다시 쿼리를 호출한다.
raw_queryset = ( User.objects.raw(
               'select id, username as 없는_프로퍼티_명, 
                   from auth_user where id=1',
               )
list(raw_queryset) # 애트리뷰트와 프로퍼티가 매칭되지 못해서 sql을 두번 호출한다.
(0.002) select id, username as ddd from auth_user where id=1; 
(0.002) SELECT `auth_user`.`id`, `auth_user`.`username` # 불필요쿼리호출
                 FROM `auth_user` WHERE `auth_user`.`id` = 1;
2–1. 첨언 [Django1.9 미만의 경우]


## RAW와 ORM 방식 JOIN 차이 
- https://docs.djangoproject.com/en/3.2/topics/db/sql/
- https://russwest.tistory.com/43
- https://itinerant.tistory.com/27
- https://076923.github.io/posts/Python-Django-11/
- https://medium.com/deliverytechkorea/django%EC%97%90%EC%84%9C%EB%8A%94-queryset%EC%9D%B4-%EB%8B%B9%EC%8B%A0%EC%9D%84-%EB%A7%8C%EB%93%AD%EB%8B%88%EB%8B%A4-2-5f6f8c6cd7e3
- https://docs.djangoproject.com/en/3.2/topics/db/queries/
https://docs.djangoproject.com/en/2.1/topics/db/queries/#lookups-that-span-relationships
