### Reflection
- JVM의 힙 영역에 로드된 class 타입의 객체를 통해, 원하는 클래스의 인스턴스를 생성할 수 있도록 지원하고, 인스턴스의 필드와 메소드를 접근 제어자와 상관없이 사용할 수 있도록 지원하는 API.
- new 키워드를 통해 만드는 객체와는 다른 것이다.
- 결론: 실행 시간에 프로그램에 대한 정보를 얻거나 수정할 수 있는 기능을 말한다.

### 장점
- java 코드가 자체 메타데이터를 엑세스하고 수정할 수 있도록 한다.
- 유연성을 크게 향상시킨다. (일반적으로 엑세스할 수 없는 메소드 및 필드(private)에 엑세스 할 수 있다.

### 단점
- 클래스의 캡슐화를 꺠뜨린다.
- 보안 위험을 초래한다.
- 타입 검사를 런타임에 하기에 일반적으로 코드가 컴파일 시간에 검사되는 것에 비해 비효율적이다.

### 할 수 있는 일
- 특정 클래스의 인스턴스 생성
```
Class<?> clazz = Class.forName("java.lang.String");
Object instance = clazz.getDeclaredConstructor().newInstance();
```
- 특정 클래스의 메소드 호출
```
Method method = clazz.getMethod("length");
Object returnValue = method.invoke(instance);
```
- 특정 클래스의 필드 값 변경 및 검색
```
Field field = clazz.getField("field");
// 만약 field의 접근자가 private라면 아래처럼 먼저 access 권한을 획득하고 변환한 다음 다시 닫아야 한다.
// field.setAccessible(true);
field.set(instance, "new value");
// field.setAccessible(false);
Object value = field.get(instance);
```
- 특정 클래스가 구현하는 인터페이스 검색
```
Class<?>[] interfaces = clazz.getInterfaces();
```
- 상위 클래스 정보 획득
```
Class<?> superclass = clazz.getSuperclass();
```
- 프록시 인스턴스 생성
```
Object proxyInstance = java.lang.reflect.Proxy.newProxyInstance(
        clazz.getClassLoader(),
        clazz.getInterfaces(),
        (proxy, method, args) -> null);
```
- 클래스 메소드 필드에 대한 어노테이션 접근
```
Annotation[] annotations = clazz.getAnnotations();
```
- 메소드 또는 생성자 호출에 필요한 인수를 동적으로 결정 및 제공
```
Method method = clazz.getMethod("method", String.class);
Object returnValue = method.invoke(instance, "argument");
```
- JVM에서 로드된 클래스 검색
```
ClassLoader classLoader = clazz.getClassLoader();
Class<?> loadedClass = classLoader.loadClass("java.lang.String");
```
