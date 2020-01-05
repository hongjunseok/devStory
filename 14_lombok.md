# Lombok
## Lombok 이란
> Lombok 은 JAVA에서 Model(DTO, VO, Domain) Object를 만들때, 멤버필드(프로퍼티)에 대한 Getter / Setter, ToString과 멤버필드에 주입하는 생성자를
코드 등 불필요하게 반복적으로 만드는 코드를 어노테이션을 통해 줄여주는 라이브러리, 프로젝트

## 다운로드
[Lombok.jar 다운로드](http://projectlombok.org/download.html)

## 의존성 주입
### Maven
```
//pom.xml
<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
<dependency>
	<groupId>org.projectlombok</groupId>
	<artifactId>lombok</artifactId>
	<version>1.16.20</version>
</dependency>
```

### Gradle
```
//build.gradle
provided 'org.projectlombok:lombok:1.16.20'
```

## 사용
### 1. 방법
* 콘솔창에서 ‘java –jar lombok.jar’ 실행
* 직접 다운로드 하여 폴더 이동 후 실행
* Maven 또는 Gradle로 다운 받은 library 실행

### 2. 어노테이션
@Data : 다음 어노테이션을 모두 한번에 처리 한다.
  - @ToString
  - @EqualsAndHashCode
  - @Getter(모든 필드)
  - @Setter(모든 필드-final로 성언되지 않은)
  - @RequiredArgsConstructor
[
	@ToString : 모든 필드를 출력하는 toString() 메소드 생성
	@EqualsAndHashCode : hascode 와 equals 메소드를 생성
	@Getter / @Setter : getter, setter를 생성하지 않도록 지원
	@NoArgsConstructor, @RequriedArgsConstructor and @AllArgsConstructor : 
	 - 인자 없는 생성자 생성, 필수 인자만 있는 생성자 생성, 모든 인자를 가진 생성자 생성
]
```
//lombok 적용 전
// 생성자와 Getter, Setter가 있음.
// VO에 변수가 추가될 때 마다 Getter, Setter도 추가해줘야함
// ToString 도 마찬 가지로 추가해 줘야 한다.
public class ExampleVO {
    private final String name;
    private int age;
    public SimpleVO(String name) {
        this.name = name;
    }
    public String getName() {
        return name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    @Override
    public String toString() {
        return "name=" + name + ", age=" + age;
    }
}

```
```
//@Data 적용 후
public @Data class ExampleVO {
    private final String name;
    private int age;
}
```
```
//@Builder 선언
//다수의 필드를 가지는 복잡한 클래스의 경우, 생성자 대신에 빌더를 사용하는 경우가 많다.
//빌더 패턴을 직접 작성해보면 코딩량이 의외로 상당함을 깨닫게 되는데 이 때, 
//@Builder 어노테이션을 사용하면 자동으로 해당 클래스에 빌더를 추가해주기 때문에 매우 편리하다.
@Builder
public class User {
    private Long id;
    private String username;
    @Singular
    private List<Integer> score;
}
//사용예제
User user = User.builder()
  .id(1004)
    private int age;
  .username("god")
  .score(70)
  .score(80)
  .build();
// User(id = 1, username = god, scores=[70,80])

```
```
//@NonNull
//@NonNull 어노테이션을 변수에 붙이면 자동으로 null 체크를 해준다.
//해당 변수가 null로 넘어온 경우, NullPointerException 예외가 발생함.
//선언
@NonNull @Setter
private String id;
//사용
obj.setId(null); // NullPointerException 발생

```
