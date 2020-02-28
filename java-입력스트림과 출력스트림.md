### 입력스트림 & 출력 스트림

---

스트림 클래스의 종류는 크게 **바이트 기반 스트림**과 **문자 기반 스트림**으로 나누어진다.

- **바이트** 기반 스트림
  - 입력 스트림
    - 최상위 클래스 : InputStream
    - 하위 클래스 (예) : XXXX**InputStream**
  - 출력 스트림
    - 최상위 클래스 : OutputStream
    - 하위 클래스 (예) : XXXX**OutputStream**

- **문자** 기반 스트림
  - 입력 스트림
    - 최상위 클래스 : Reader
    - 하위 클래스 (예) : XXXX**Reader**
  - 출력 스트림
    - 최상위 클래스 : Writer
    - 하위 클래스 (예) : XXXX**Writer**
    
#### 입력스트림 출력스트림 모두 사용 후에 **반드시** 자원 해제 시켜 주어야 한다.

```java
out.close();
in.close();
```

**😇 매번 close 해주기 번거롭다. try-with-resoures문을 사용하자.**

[try-with-resources 참조 문서](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html)

`try-with-resources`문은 JDK1.7에서부터 사용 가능하며, 자원 이용 끝과 동시에 해제 가능하다.

- 기존
```java
static String readFirstLineFromFileWithFinallyBlock(String path) throws IOException {
    BufferedReader br = new BufferedReader(new FileReader(path));
    try {
        return br.readLine();
    } finally {
        if (br != null) br.close();
    }
}
```

- JDK 1.7 ~
```java
static String readFirstLineFromFile(String path) throws IOException {
    try (BufferedReader br =
                   new BufferedReader(new FileReader(path))) {
        return br.readLine();
    }
}
```

또한, `try` 에 여러 자원을 사용한다면 `;`을 이용하여 여러 자원을 선언해줄 수 있다.

**반복적인 finally, close를 해주지 않아도 된다. 👍🏻**


#### 출력스트림의 `flush`

출력 스트림 내부에는 작은 버퍼가 있어서 데이터가 출력되기 전에 버퍼에 쌓여있다가 순서대로 출력된다.
`flush()` 메소드는 버퍼에 잔류하고 있는 데이터를 모두 출력시키고 버퍼를 비우는 역할을 한다.
프로그램에서 더 이상 출력할 데이터가 없다면 `flush()` 메소드를 마지막으로 호출하여 버퍼에 잔류하는 모든 데이터가 출력되도록 해야 한다.

```java
os.write(data);
os.flush();
os.close();
```

**🤔 `close`를 해주면 `flush`를 따로 선언해주지 않아도 된다 ?**

javadocs에서는 `close`를 호출하면 `flush`가 먼저 호출된다고 적혀있다.
**따라서, 명시적으로 선언할 필요는 없다.**

[javadocs](https://docs.oracle.com/javase/8/docs/api/java/io/Writer.html#close--)

> Closes the stream, flushing it first. Once the stream has been closed, further write() or flush() invocations will cause an IOException to be thrown. Closing a previously closed stream has no effect.