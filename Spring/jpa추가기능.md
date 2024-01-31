# MappedSuperclass
@MappedSuperclass를 가진 클래스를 만든다.  
//여러 엔티티 클래스를 만들 때 중복해야 할 엔티티가 있는 경우 한 곳에 모아서 사용하도록 할 수 있다.

예를들어 @Id같은 경우 모든 엔티티 클래스에서 필요로 한다.  
그리고 @Id 때문에 엔티티 클래스들은 @Setter 롬북을 사용하지 못한다.  
그래서 @Id를 한곳에 정의 하고 그것을 @Id가 필요한 엔티티 클래스에 상속을 해주면  
엔티티 클래스들은 @Id를 정의하지 않아도 사용할 수 있게 된다.

```java
@MappedSuperclass
@Getter
public abstract class BaseEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
}
```

```java
@Entity
@NoArgsConstructor
@Getter
@AllArgsConstructor
@Builder
@Setter
public class Article extends BaseEntity { //BaseEntity를 상속받음
//    @Id
//    @GeneratedValue(strategy = GenerationType.IDENTITY)
//    private Long id;
    private String title;
    private String content;
    private String writer;
}
```

```java
@Entity
@NoArgsConstructor
@Getter
@AllArgsConstructor
@Builder
@Setter //BaseEntity를 상속 받으므로 @Setter을 해도 id에 영향을 주지 않는다.
public class Comment extends BaseEntity {//BaseEntity를 상속받음
//    @Id
//    @GeneratedValue(strategy = GenerationType.IDENTITY)
//    private Long id;

    private String content;
    private String writer;
}
```

또한 상속 받는 클래스들이 필요한 기능을 상속 할 수 있다,,, 이건 뭐 당연한거니까,,,  

# 엔티티의 생성일자와 최종 수정일자를 관리할 수 있는 방법.

@CreatedDate와 @LastModifiedDate는 Spring Data JPA에서 제공하는 어노테이션으로, 엔티티의 생성일자와 최종 수정일자를 자동으로 관리하는 데 사용됩니다.  
@CreatedDate: 엔티티가 생성될 때 해당 필드에 현재 일시를 자동으로 저장합니다.  
@LastModifiedDate: 엔티티가 수정될 때 해당 필드에 현재 일시를 자동으로 저장합니다.

@EntityListeners 어노테이션은 특정 엔티티에 대한 감시자 클래스를 지정합니다.  
AuditingEntityListener 클래스는 주로 생성일자(@CreatedDate)와 최종 수정일자(@LastModifiedDate)를 자동으로 관리하는 데 사용됩니다.

원할때 기능을 넣거나 원하지 않을 때 기능을 해제하기 위해 어플리케이션이 아닌 다른 클래스에 @EnableJpaAuditing를 추가한다.

```java
@Configuration
@EnableJpaAuditing //Entity Auditing 기능 설정
public class JpaConfig {
}
```

```java
@MappedSuperclass
@Getter
@EntityListeners(AuditingEntityListener.class)
public abstract class BaseEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @CreatedDate
    private LocalDateTime createAt; //채팅 기록을 남길  때 채팅한 시간을 남길 수 있다.
    @LastModifiedDate
    private LocalDateTime updateAt;
}
```