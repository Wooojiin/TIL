# Persistence Context(영속성 컨텍스트)



#### 영속성 컨텍스트

- 엔티티를 영구 저장하는 환경
- 논리적인 개념
- 엔티티 매니저를 통해서 영속성 컨텍스트에 접근



#### 엔티티 생명주기

![](https://media.vlpt.us/post-images/conatuseus/3861eed0-d482-11e9-9b0f-dd1a4f570095/image.png)

- 비영속(new/transient)
  - 객체를 생성한 상태
- 영속(managed)
  - 영속성 컨텍스트에 관리되는 상태
  - 객체 생성 후 EntitiyManager에 객체 저장
  - entitiyManager.persist("객체");
- 준영속(detached)
  - 영속성 컨텍스트에 저장되었다가 분리된 상태
- 삭제(removed)
  - 삭제된 상태



#### 영속성 컨텍스트 이점

- 1차 캐시
  - 영속 컨텍스트에 저장후 조회시 영속 컨테스트에 먼저 조회
  - 없는 경우 DB 조회후 영속 컨텍스트에 저장 후 반환
  - 트랜잭션 단위라 큰 도움은 되지 않음
- 동일설 보장
- 트랜잭션을 지원하는 쓰기 지연
  - persist("객체") 실행시 쓰기 지연 SQL 저장소에 INSERT SQL 생성, 1차 캐시에 저장
  - transaction.commit() 시 쓰기 지연 SQL 저장소 flush 하면서 DB로 쿼리 전달
- 변경 감지(Dirty checking)
  - 커밋시 flush(), entity 와 스냅샷 비교
  - 최초 영속성 컨텍스트에 들어온 entity 스냅샷으로 저장해둠
  - 스냅샷과 비교해  현재 entity랑 다른 경우 SQL 저장소에 update 쿼리 생성
  - DB로 쿼리 전달
- 지연 로딩



#### Flush

- 영속성 컨텍스트 변경내용을 DB에 반영
- Flush해도 1차 캐시 유지, 쓰기 지연 SQL 저장소에 있는 변경 쿼리들 DB에 반영만
- 영속성 컨텍스트 비우는것 :x:
- 영속성 컨텍스트 변경내용 DB에 동기화