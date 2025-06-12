

전체 JSON 읽고 Java 코드에서 페이징

```java
ObjectMapper mapper = new ObjectMapper();
List<LogEntry> logs = mapper.readValue(new File("logs.json"), new TypeReference<List<LogEntry>>() {});
int page = 1;
int size = 10;
int fromIndex = (page - 1) * size;
int toIndex = Math.min(fromIndex + size, logs.size());
List<LogEntry> pageData = logs.subList(fromIndex, toIndex);

```