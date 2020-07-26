# Builder
### 빌더 패턴은 생성 패턴중 하나이다.
### 객체 생성 후 상태값을 변경 할수 없게 없게 동결 시키는 패턴이다.
### 빌더 패턴은 많은 Optional한 변수(혹은 파라미터)나 지속성 없는 상태 값들에 대해 처리해야 하는 문제들을 해결한다.
### 예를들어, **팩토리 패턴**이나 **추상 팩토리 패턴**에서는 생성해야하는 클래스에 대한 속성 값이 많을 때 다음과 같은 **이슈**들이 있다.
1. 클라이언트 프로그램으로부터 팩토리 클래스로 많은 파라미터를 넘겨줄 때 타입, 순서 등에 대한 관리가 어려워져 에러가 발생할 확률이 높아진다.
2. 경우에 따라 필요 없는 파라미터들에 대해서 팩토리 클래스에 일일이 null 값을 넘겨줘야 한다.
3. 생성해야 하는 sub class가 무거워지고 복잡해짐에 따라 팩토리 클래스 또한 복잡해진다.
### 빌더 패턴은 이러한 문제들을 해결하기 위해 별도의 Builder 클래스를 만들어 필수 값에 대해서는 생성자를 통해 선택적인 값들에 대해서는 메서드를 통해 step-by-step으로 값을 받은 후에 build() 메서드를 통해 최종적으로 하나의 인스턴스를 리턴하는 방식이다.
# 빌더패턴을 구현하는 방법
1. 빌더 클래스를 Static Nested Class로 생성한다. 이때, 관례적으로 생성하고자 하는 클래스 이름 뒤에 Builder를 붙인다. 예를들어, Computer 클래스에 대한 빌더 클래스에 이름은 ComputerBuilder라고 정의한다.
2. 빌더 클래스의 생성자는 public으로 하며, 필수 값들에 대해 생성자의 파라미터로 받는다.
3. 옵셔널한 값들에 대해서는 각각의 속성 메서드로 제공하며, 이때 중요한 것은 메서드의 리턴 값이 빌더 객체 자신이어야 한다.
4. 마지막으로, 빌더 클래스 내에 build() 메서드를 정의하여 클라이언트 프로그램에게 최종 생성된 결과물을 제공한다. 이렇듯 build()를 통해서만 객체 생성을 제공하기 때문에 생성 대상이 되는 클래스의 생성자는 private으로 정의해야 한다.
# 빌더패턴의 장단점
## 장점
- 인스턴스를 생성하는데 있어서 필수적인 인자와 선택적인 인자를 구별할 수 있다.
- 가독성 좋게 코드로 인자를 넘길 수 있다.
- 객체의 일관성을 깨지 않는다.
## 단점
- 객체 생성을 추가하는 것은 쉬우나 객체를 구성하는 각 부분들을 새롭게 추가하는 것은 어렵다.
## 활용상황
- 생성자 인자가 많은 경우
# 예제코드
> builder
```java
public class Guitar {
	
    private String body;
    private String strings;
	
    private boolean isMuteEnabled;
    private boolean isSoundsEnabled;
	
    public String getBody() {
        return body;
    }
 
    public String getStrings() {
        return strings;
    }
 
    public boolean isMuteEnabled() {
        return isMuteEnabled;
    }
 
    public boolean isSoundsEnabled() {
        return isSoundsEnabled;
    }
	
    private Guitar(GuitarBuilder builder) {
        this.body=builder.body;
        this.strings=builder.strings;
        this.isMuteEnabled=builder.isMuteEnabled;
        this.isSoundsEnabled=builder.isSoundsEnabled;
    }
	
    public static class GuitarBuilder{
 
        private String body;
        private String strings;
 
        private boolean isMuteEnabled;
        private boolean isSoundsEnabled;
		
        public GuitarBuilder(String body, String strings){
            this.HDD=body;
            this.strings=strings;
        }
 
        public GuitarBuilder setMuteEnabled(boolean isMuteEnabled) {
            this.isMuteEnabled = isMuteEnabled;
            return this;
        }
 
        public GuitarBuilder setSoundsEnabled(boolean isSoundsEnabled) {
            this.isSoundsEnabled = isSoundsEnabled;
            return this;
        }
		
        public Guitar build(){
            return new Guitar(this);
        }
 
    }
}
```
> main
```java
public class TestBuilderPattern {
 
    public static void main(String[] args) {
        Guitar guitar = new Guitar.GuitarBuilder("Dreadnought", "Coating")
                .setSoundsEnabled(true)
                .setMuteEnabled(true)
                .build();
    }
 
}
```