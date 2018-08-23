###web环境设置
运行使用maven命令tomcat7:run
**pom基本配置**


```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">

  <modelVersion>4.0.0</modelVersion>
  <packaging>war</packaging>

  <name>tryweb</name>
  <groupId>brook.com</groupId>
  <artifactId>tryweb</artifactId>
  <version>1.0-SNAPSHOT</version>

  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.2</version>
        <configuration>
          <url>http://localhost:8080/manager/text</url>
          <server>tomcat</server>
          <username>admin</username>
          <password>admin</password>
        </configuration>
      </plugin>

    </plugins>
  </build>

  <dependencies>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.0.1</version>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <groupId>javax.servlet.jsp</groupId>
      <artifactId>jsp-api</artifactId>
      <version>2.2</version>
      <scope>provided</scope>
    </dependency>
  </dependencies>

</project>

```


### java web
### 模拟客户端发起请求示例


```
  private String getCookieHeader() {
    Map<Integer, String> headerValues = new HashMap<Integer, String>();
    HttpClient httpClient = HttpClientBuilder.create().build();
    HttpPost request = new HttpPost(BASE_URL + COCKPIT_API);
    request.addHeader(CONTENT_TYPE, APPLICATION_X_WWW_FORM_URLENCODED);
    request.addHeader(CONNECTION, KEEP_ALIVE);
    StringEntity params;
    try {
      params = new StringEntity(USERNAME_PASSWORD);
      request.setEntity(params);
      HttpResponse response = httpClient.execute(request);
      Header[] cookieHeader = response.getHeaders(SET_COOKIE);

      for (Header h : cookieHeader) {
        String[] str = h.getValue().split(";");
        for (String s : str) {
          if (s.startsWith(JSESSIONID_HEAD)) {
            return s;
          }
        }
      }
    } catch (Exception e) {
      e.printStackTrace();
    }
    return "";
  }

  private String createDeployment(String cookieHeader, String deploymentName, String tenantId, String filePath) {
    HttpClient httpclient = HttpClientBuilder.create().build();
    HttpPost request = new HttpPost(BASE_URL + CREAT_DEPLOYMENT_API);
    request.addHeader(COOKIE, cookieHeader);

    MultipartEntityBuilder multipartEntityBuilder = MultipartEntityBuilder.create();
    multipartEntityBuilder.addPart(DEPLOYMENT_NAME, new StringBody(deploymentName, ContentType.TEXT_PLAIN));
    multipartEntityBuilder.addPart(DEPLOYMENT_SOURCE, new StringBody("local", ContentType.TEXT_PLAIN));
    multipartEntityBuilder.addPart(TENANT_ID, new StringBody(tenantId, ContentType.TEXT_PLAIN));
    File file = new File(filePath);
    if (!file.exists()) {
      return "File not exist! Path=" + filePath;
    }
    multipartEntityBuilder.addPart(file.getName(), new FileBody(file));

    try {
      request.setEntity(multipartEntityBuilder.build());
      HttpResponse response = httpclient.execute(request);
      StatusLine statusLine = response.getStatusLine();
      if (statusLine.getStatusCode() != 200) {
        return statusLine.toString();
      }
    } catch (Exception e) {
      return e.getMessage();
    }
    return "";
  }
```


### HttpServletResponse对象
