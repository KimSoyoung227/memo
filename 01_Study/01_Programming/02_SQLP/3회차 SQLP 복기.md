[맞은 문제]
- 주식별자만 있어도 되는 엔터티 : 관계엔터티  : o
- 피벗 -> 언피벗 쿼리 -> avg(국어+영어+수학) 출력 : o
- 인스턴트 크러시일 경우 미커밋 데이터 날라가나요? + 트랜잭션 영속성 : o
- 주식별자는 null이 있으면 안된다. 존재성 : o
- …? \undo segment
- undo의 특징중 잘못된거 : o
- 자식 테이블의 fk delete casecade : o

[내가 쓴 번호가 기억이 안나는 문제]
- 로우락 select for update + 동기성제약 vs alter table 타입변경
- 비식별자 ie erd 관계 해석 : 고객-주문(자식)
- ctas 인덱스도 복사 안된다(x) vs mysql cats 대신 insert into 써야한다. (o)

[틀린 문제]
- regex_like (‘’,’’,’x’) -> 세번째 인자의 뜻 : x
- Batch, prefetch 실행계획 고르기 : x
- 예제 쿼리( select … from t1 where t2(+)=t1 and t3(+)=t2 and t4(+)=t3 and t5=t4… )에서 outer 가능한? 테이블을 고르시오. T2,t3,t4 : x

  ———————————————————————————————————————

### 실기1. leading(a t2@subq x) 인데 /*+ leading( a @subq x) use_nl(@subq) use_nl(x) */ 라고 함. 인라인뷰에 order by를 넣지 않음. 감점 예상됨.

```

SELECT 
	c1, min_c2, max_c2
FROM (
	SELECT /*+ leading(a @subq x) use_nl(@subq) use_nl(x) */
		a.c1, min_c2, max_c2
	FROM t1 a, (
				SELECT /*+ no_merge push_pred */
					c1, min(c2) as min_c2, max(c2) as max_c2
				FROM t3
				GROUP BY c1
				) x
	WHERE a.c1 >= 1000
	AND x.c1(+) = a.c1
	AND EXISTS (SELECT /*+ qb_name(subq) unnest nl_sj */ 'x'
				FROM t2
				WHERE c1 = a.c2)
	ORDER BY a.c1
)
WHERE ROWNUM <= 10;
```
### 실기 2. 불필요한 셀프조인으로 감점 예상

```

Merge
Into t1 t
using( select /*+ leading(a b) use_nl(b) rowid(b) */ 
			b.c1, a.c3_n
		from (
			select 
				rowid rn, c1, row_number() over(partition by c1 order by c2 ) as c3_n
			from t1
			where c1 <= 1000
		) a, t1 b
		where a.rn = b.rowid
) s
on(t.c1 = s.c1)
When matched then
Update set t.c3 = s.c3_n;
```