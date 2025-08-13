# `equals()` 메서드
자바의 모든 객체는 최상위 부모 클래스인 Object 클래스를 상속받는다. 따라서 자바의 모든 객체는 object 클래스에 정의된 메서드들을 사용할 수 있다.
자바 객체의 공통 메서드인 `equals()` 메서드에 대해 알아보자.

## 🎯 핵심 개념

java 객체의 논리적 동등성을 판단하는 메서드로, `object` 클래스에서 상속받아 재정의(override)하여 사용한다. 

### equals() 메서드 사용법
- **`==` 연산자:** 두 객체가 물리적으로 동일한 인스턴스인지(같은 메모리 주소를 가리키는지) 확인한다.

- **`equals()` 메서드:** 두 객체가 논리적으로 동일한지 확인한다.

`Object` 클래스의 기본 `equals()`는 `==` 연산자와 동일하게 작동한다. 하지만 대부분의 클래스에서 객체의 논리적 동등성을 판단하기 위해 **`equals()`를 재정의**하여 사용하는 것이 일반적이다.

## ⚖️ equals() 일반 규약 

`equals()` 메서드를 재정의할 때 반드시 지켜야 할 규약이 있다. 규약을 지키지 않으면 프로그램의 신뢰도와 안정성이 크게 떨어지기 때문에 꼭 지켜야한다.

- **반사성 (Reflexive):** 자기 자신과 같아야 한다.
  
  `x.equals(x) == true`
  
- **대칭성 (Symmetric):** 순서에 관계없이 결과가 동일해야 한다.
   
   `x.equals(y) == y.equals(x)`

- **추이성 (Transitive):** 연쇄적 동등성을 가져야 한다.
  
   `if (x.equals(y) && y.equals(z)) then x.equals(z)`
  
- **일관성 (Consistent):** 객체의 변경이 없으면 결과도 일정해야 한다.
  
   `x.equals(y) 를 여러 번 호출해도 같은 결과`

- **null 처리:** null과는 절대 같지 않다.
  
   `x.equals(null) == false`

###  규약 위반 예시

```JAVA
// ❌ 대칭성 위반
public class CaseInsensitiveString {
    private String s;
    
    public boolean equals(Object obj) {
        if (obj instanceof String) {
            return s.equalsIgnoreCase((String) obj);
        }
        if (obj instanceof CaseInsensitiveString) {
            return s.equalsIgnoreCase(((CaseInsensitiveString) obj).s);
        }
        return false;
    }
}

CaseInsensitiveString cis = new CaseInsensitiveString("Polish");
String s = "polish";

cis.equals(s);  // true
s.equals(cis);  // false - 대칭성 위반
```

## 🛠️ 올바른 equals() 구현 패턴

### 표준 구현 템플릿

```JAVA
@Override
public boolean equals(Object obj) {
    // 1단계: 자기 자신과의 비교 (성능 최적화)
    if (this == obj) {
        return true;
    }
    
    // 2단계: null 체크
    if (obj == null) {
        return false;
    }
    
    // 3단계: 클래스 타입 체크
    if (getClass() != obj.getClass()) {
        return false;
    }
    
    // 4단계: 타입 캐스팅 및 필드 비교
    Person other = (Person) obj;
    return Objects.equals(name, other.name) && 
           age == other.age;
}
```

### instanceof vs getClass()

```JAVA
// instanceof 사용 (상속 허용)
if (!(obj instanceof Person)) {
    return false;
}

// getClass() 사용 (정확한 타입만 허용)
if (getClass() != obj.getClass()) {
    return false;
}
```

### 언제 어떤 것을 사용할까?

- **instanceof:** 상속 관계에서 부모-자식 간 동등성 허용

- **getClass():** 정확히 같은 클래스만 동등하다고 판단 (더 엄격)

## 🛠️ hashCode()와의 관계: 핵심 규약

`equals()`를 재정의 했다면 hashCode()도 반드시 재정의 해야한다. 이는  자바의 `Object` 클래스 규약으로 이 규약을 지키지 않으면 `HashMap`이나 `HashSet`과 같은 해시 기반 컬렉션에서 객체를 찾지 못하는 문제가 발생한다. `equals()`를 재정의할 때 반드시 `hashCode()`도 재정의하는 습관을 가지자.



