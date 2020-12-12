---
title: "MongoDB TTL Index"
date: 2020-12-12 13:53:00 +0900
categories: MongoDB
---
TTL Index는 일정 시간 이후에 Document를 삭제합니다.   
MongoDB에서 일반적인 인덱스는 다음의 함수로 생성합니다.    

>MongoDB   
>```
>db.collection.createIndex({ field : -1 })
>```   
   
>Kotlin(KMongo)   
>```
>collection.createIndex(Indexes.ascending(FIELD))
>```   

주어진 Collection에 새 Index를 추가합니다.   
혹시 이미 Index가 만들어져 있다면 오류가 발생합니다.   
필요할 경우에 다음의 함수를 사용합니다.

>MongoDB
>```
>collection.ensureIndex({ field : -1 })
>```   
   
>Kotlin(KMongo)   
>```
>collection.ensureIndex(Indexes.ascending(FIELD))
>```

TTL 인덱스를 설정하기 위해서는 expireAfterSeconds 옵션을 주면 됩니다.   
Index 대상 필드는 Date 타입이어야 합니다.

>MongoDB
>```
>db.eventlog.createIndex( { "lastModifiedDate": 1 }, { expireAfterSeconds: 3600 } )
>```   

>Kotlin(KMongo)   
>```
>collection.createIndex(Indexes.ascending(FIELD), IndexOptions().expireAfter(30, TimeUnit.MINUTE))
>```

## 주의사항
### TimeZone 문제
MongoDB에서 Date 필드는 기본적으로 UTC로 저장이 됩니다. 따라서 로컬 TimeZone을 기준으로 저장을 해 놓으면 삭제되지 않는 경우가 발생합니다.   
저장할 때에는 UTC로 저장하도록 합니다.

### 참고문헌   
* https://docs.mongodb.com/manual/indexes/