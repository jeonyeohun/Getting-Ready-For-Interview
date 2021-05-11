# Join 연산 

## Natural Join

- Natural Join은 두 테이블에서 공통된 속성을 찾아 필드 값이 같은 튜플을 합쳐서 하나의 테이블로 만들어줍니다. Natural Join 은 동일한 자동으로 공통되는 속성을 찾기 때문에 두 테이블의 도메인, 속성값, 필드값이 같아야 한다는 조건이 있습니다.

## Inner Join 

- Inner Join 은 두 테이블 사이에 공통된 속성을 찾아 필드 값이 같은 튜플을 합쳐서 하나의 테이블로 만듭니다. 이때 Join 에 대한 조건을 직접 지정해주어서 지정된 속성과 속성값을 기준으로 테이블을 합치게 됩니다.

## Natural Join 과 Inner Join 의 차이를 말해보세요.

- Inner Join 은 동일한 속성을 두 테이블의 각각 속한 별개의 속성으로 표시하는 반면에 Natural Join 은 하나의 속성으로 처리합니다.

~~~sql
# Inner Join
| table1.name | table2.name |

# Natural Join
| name |
~~~

## Outer Join 

- Outer Join 은 두 테이블을 동일한 속성과 속성값을 기준으로 합치지만 양 테이블에 동일한 값이 없는 튜플도 함께 합쳐줍니다. 이때 값이 없는 필드는 NULL 로 표시합니다.

## Outer Join 에는 어떤 종류가 있어요?

- Left Join, Right Join, Full Join 이 있습니다.
- Left Join 은 왼쪽 테이블에 있는 데이터를 모두 읽은 뒤 좌측 테이블을 읽으며 공통된 속성과 속성값을 가진 튜플을 찾습니다. 따라서 왼쪽 테이블에는 존재하지 않지만 오른쪽 테이블에 존재하는 튜플은 제외됩니다.
- Right Join 은 반대로 오른쪽 테이블을 먼저 읽기 때문에 오른쪽 테이블에 존재하지 않는 속성값을 가진 튜플은 왼쪽 테이블에서 합칠 때 제외됩니다.
- Full Join 은 두 테이블의 모든 튜플을 합쳐줍니다. 따라서 서로 겹치지 않는 튜플도 제외되지 않고 모두 포함됩니다.

## MySQL 에서는 Full Join 을 어떻게 구현하는지 알아요?

- MySQL 에서는 Full Join 을 지원하지 않기 때문에 두 테이블을 각각 left join, right join 하고 UNION 으로 합쳐주어야 합니다.
