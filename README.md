
<p align="center">
 <img width="300px" src="https://www.trustedreviews.com/wp-content/uploads/sites/54/2022/04/How-to-make-google-account-920x613.jpg" align="center" alt="GitHub Readme Stats" />
 <h2 align="center">Spring boot - Login với Google Account</h2>
</p>

Thông qua hướng dẫn Spring Boot này, bạn sẽ học cách triển khai chức năng đăng nhập một lần bằng tài khoản Google cho ứng dụng web Spring Boot hiện có, sử dụng thư viện Spring OAuth2 Client - cho phép người dùng cuối đăng nhập bằng tài khoản Google của chính họ thay vì thông tin đăng nhập do ứng dụng quản lý .
Giả sử rằng bạn có một dự án Spring Boot hiện có với chức năng xác thực đã được triển khai bằng Spring Security và thông tin người dùng được lưu trữ trong cơ sở dữ liệu MySQL (Nếu không, hãy tải xuống dự án mẫu trong hướng dẫn này ).<br>
Sau đó, chúng tôi sẽ cập nhật trang đăng nhập cho phép người dùng đăng nhập bằng tài khoản Google của chính họ như sau:<br>

<div align="center">
 <img width="600px" src="https://www.codejava.net/images/articles/frameworks/springboot/oauth-google/Login_Google.png" align="center" alt="GitHub Readme Stats" />
</div>

# 1. Tạo thông tin đăng nhập Google OAuth

Trước tiên, hãy làm theo video này để tạo ID ứng dụng khách Google OAuth nhằm nhận các khóa truy cập của Google đăng nhập một lần trên API (ID ứng dụng khách và bí mật ứng dụng khách). Lưu ý rằng bạn cần thêm một URI chuyển hướng được ủy quyền như sau:
<br> <b> <i> http: // localhost: 8080 / login / oauth2 / code / google </b> </i> 
<br> Trong trường hợp ứng dụng của bạn được lưu trữ với đường dẫn ngữ cảnh riêng, ví dụ / Shopme - thì hãy chỉ định URI chuyển hướng như sau:
<br> <b> <i> http: // localhost: 8080 / Shopme / login / oauth2 / code / google </b> </i> 

# 2. Khai báo sự phụ thuộc cho ứng dụng khách Spring Boot OAuth2

Bên cạnh phần phụ thuộc Spring Security, bạn cần thêm một phần phụ thuộc mới vào tệp dự án Maven để sử dụng Spring Boot OAuth2 Client API giúp đơn giản hóa đáng kể việc tích hợp một lần cho các ứng dụng Spring Boot.<br> <br>
Vì vậy, hãy khai báo phụ thuộc sau: <br>
 <div>
     
        <!-- https://mvnrepository.com/artifact/org.springframework.security.oauth.boot/spring-security-oauth2-autoconfigure -->
        <dependency>
            <groupId>org.springframework.security.oauth.boot</groupId>
            <artifactId>spring-security-oauth2-autoconfigure</artifactId>
            <version>2.1.3.RELEASE</version>
        </dependency>


 </div>
 
 # 3. Định cấu hình thuộc tính Spring OAuth2 cho Google
 
 Tiếp theo, mở tệp cấu hình Spring Boot ( application.yml ) và chỉ định các thuộc tính cho đăng ký Máy khách OAuth2 cho nhà cung cấp có tên google, như sau: <br>
  <h3> &nbsp; 3.1 Tạo dự án Spring Boot </h3> 
  <h3> &nbsp; Trên Eclipse, tạo một dự án Spring Boot : </h3>
   <img width="500px" src="https://s1.o7planning.com/vi/11823/images/17226826.png" align="center" alt="GitHub Readme Stats" /> <br>
  <h3> &nbsp; Cơ sở dữ liệu sử dụng trong ứng dụng này là MySQL, SQL Server, PostGres hoặc Oracle hoặc bất kỳ một cơ sở dữ liệu nào khác mà bạn quen thuộc. </h3> <br>
   <img width="500px" src="https://s1.o7planning.com/vi/11823/images/17226880.png" align="center" alt="GitHub Readme Stats" /> <br>
  <h3> &nbsp; Khai báo các thư viện Spring Social vào project của bạn: </h3>
    <ol>
       <li>spring-security-oauth2</li>
       <li>spring-social-core</li>
       <li>spring-social-config</li>
       <li>spring-social-security </li>
       <li>spring-social-facebook </li>
       <li>spring-social-twitte </li>
       <li>spring-social-github </li>
       <li>spring-social-linkedin </li>
       <li>spring-social-google </li>
    </ol>
<h3> &nbsp; Nội dung đầy đủ của tập tin pom.xml: </h3>
<div class="df-fragment df-text-xml">
<div><h3>pom.xml </h3></div>
 <div><pre><code class="language-xml hljs">
<span class="hljs-meta">&lt;?xml version="1.0" encoding="UTF-8"?&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">project</span> <span class="hljs-attr">xmlns</span>=<span class="hljs-string">"http://maven.apache.org/POM/4.0.0"</span>
    <span class="hljs-attr">xmlns:xsi</span>=<span class="hljs-string">"http://www.w3.org/2001/XMLSchema-instance"</span>
    <span class="hljs-attr">xsi:schemaLocation</span>=<span class="hljs-string">"http://maven.apache.org/POM/4.0.0
    http://maven.apache.org/xsd/maven-4.0.0.xsd"</span>&gt;</span>
    <span class="hljs-tag">&lt;<span class="hljs-name">modelVersion</span>&gt;</span>4.0.0<span class="hljs-tag">&lt;/<span class="hljs-name">modelVersion</span>&gt;</span>
  <groupId>org.o7planning</groupId>
    <artifactId>SpringBootSocialJPA</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>SpringBootSocialJPA</name>
    <description>Spring Boot + Oauth2 Social + JPA</description>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.0.RELEASE</version>
        <relativePath /> <!-- lookup parent from repository -->
    </parent>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>


        <!-- http://mvnrepository.com/artifact/org.springframework.security/spring-security-config -->
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-config</artifactId>
        </dependency>


        <!-- https://mvnrepository.com/artifact/org.springframework.security.oauth/spring-security-oauth2 -->
        <dependency>
            <groupId>org.springframework.security.oauth</groupId>
            <artifactId>spring-security-oauth2</artifactId>
            <version>2.2.1.RELEASE</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework.social/spring-social-core -->
        <dependency>
            <groupId>org.springframework.social</groupId>
            <artifactId>spring-social-core</artifactId>
            <version>1.1.5.RELEASE</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework.social/spring-social-config -->
        <dependency>
            <groupId>org.springframework.social</groupId>
            <artifactId>spring-social-config</artifactId>
            <version>1.1.5.RELEASE</version>
        </dependency>
 

        <!-- https://mvnrepository.com/artifact/org.springframework.social/spring-social-web -->
        <dependency>
            <groupId>org.springframework.social</groupId>
            <artifactId>spring-social-web</artifactId>
            <version>1.1.5.RELEASE</version>
        </dependency>


        <!-- https://mvnrepository.com/artifact/org.springframework.social/spring-social-security -->
        <dependency>
            <groupId>org.springframework.social</groupId>
            <artifactId>spring-social-security</artifactId>
            <version>1.1.5.RELEASE</version>
        </dependency>



        <!-- https://mvnrepository.com/artifact/org.springframework.social/spring-social-facebook -->
        <dependency>
            <groupId>org.springframework.social</groupId>
            <artifactId>spring-social-facebook</artifactId>
            <version>2.0.3.RELEASE</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.social/spring-social-twitter -->
        <dependency>
            <groupId>org.springframework.social</groupId>
            <artifactId>spring-social-twitter</artifactId>
            <version>1.1.2.RELEASE</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.social/spring-social-github -->
        <!-- <dependency>
            <groupId>org.springframework.social</groupId>
            <artifactId>spring-social-github</artifactId>
            <version>1.0.0.M4</version>
        </dependency> -->

        <!-- https://mvnrepository.com/artifact/org.springframework.social/spring-social-linkedin -->
        <dependency>
            <groupId>org.springframework.social</groupId>
            <artifactId>spring-social-linkedin</artifactId>
            <version>1.0.2.RELEASE</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.social/spring-social-google -->
        <dependency>
            <groupId>org.springframework.social</groupId>
            <artifactId>spring-social-google</artifactId>
            <version>1.0.0.RELEASE</version>
        </dependency>


        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>

        <dependency>
            <groupId>org.postgresql</groupId>
            <artifactId>postgresql</artifactId>
            <scope>runtime</scope>
        </dependency>

        <dependency>
            <groupId>com.oracle</groupId>
            <artifactId>ojdbc6</artifactId>
            <version>11.2.0.3</version>
        </dependency>

        <!-- SQL Server Mssql-Jdbc Driver -->
        <dependency>
            <groupId>com.microsoft.sqlserver</groupId>
            <artifactId>mssql-jdbc</artifactId>
            <scope>runtime</scope>
        </dependency>

        <!-- SQL Server JTDS Driver -->
        <!-- https://mvnrepository.com/artifact/net.sourceforge.jtds/jtds -->
        <dependency>
            <groupId>net.sourceforge.jtds</groupId>
            <artifactId>jtds</artifactId>
            <version>1.3.1</version>
        </dependency>


        <!-- Email validator,... -->
        <!-- http://mvnrepository.com/artifact/commons-validator/commons-validator%20 -->
        <dependency>
            <groupId>commons-validator</groupId>
            <artifactId>commons-validator</artifactId>
            <version>1.5.0</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <repositories>
        <!-- Repository for ORACLE JDBC Driver -->
        <repository>
            <id>codelds</id>
            <url>https://code.lds.org/nexus/content/groups/main-repo</url>
        </repository>
        
    </repositories>


    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>    
     
<span class="hljs-tag">&lt;/<span class="hljs-name">project</span>&gt;</span>
</code></pre>
</div>
  <div>  &nbsp; <h3>SpringBootSocialJpaApplication.java</h3></div>
 <img width="600px" src="https://user.oc-static.com/upload/2019/11/21/15742940638228_pasted%20image%200%20%2810%29.png" align="center" alt="GitHub Readme Stats" />
<h3> &nbsp; 3.2 Cấu hình DataSource </h3> 
<h4> &nbsp; Các thông tin về cơ sở dữ liệu cần được cấu hình trong tập tin application.properties: </h4> 
<div>
        <div> <h4> &nbsp; 3.2.1 : application.properties (MySQL) </h4></div>

        spring.thymeleaf.cache=false
        # ===============================
        # DATABASE
        # ===============================

        spring.datasource.driver-class-name=com.mysql.jdbc.Driver
        spring.datasource.url=jdbc:mysql://localhost:3306/social
        spring.datasource.username=root
        spring.datasource.password=12345

        # ===============================
        # JPA / HIBERNATE
        # ===============================

        spring.jpa.show-sql=true
        spring.jpa.hibernate.ddl-auto=update
        spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5Dialect
 </div>
 <div>
        <div >  <h4> &nbsp; 3.2.2 : application.properties (Mssql-Jdbc Driver) </h4></div>
        
        spring.thymeleaf.cache=false      
        # ===============================
        # DATABASE
        # ===============================

        spring.datasource.driver-class-name=com.microsoft.sqlserver.jdbc.SQLServerDriver
        spring.datasource.url=jdbc:sqlserver://tran-vmware-pc\\SQLEXPRESS:1433;databaseName=social
        spring.datasource.username=sa
        spring.datasource.password=12345


        # ===============================
        # JPA / HIBERNATE
        # ===============================

        spring.jpa.show-sql=true
        spring.jpa.hibernate.ddl-auto=update
        spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.SQLServer2012Dialect
</div>
<div>
 <div class="df-fragment df-text-properties">
        <div> <h4> 3.2.3 : application.properties (PostGres) </h4></div>
        
        spring.thymeleaf.cache=false

        # ===============================
        # DATABASE CONNECTION
        # ===============================

        spring.datasource.driver-class-name=org.postgresql.Driver
        spring.datasource.url=jdbc:postgresql://tran-vmware-pc:5432/social
        spring.datasource.username=postgres
        spring.datasource.password=12345

        # ===============================
        # JPA / HIBERNATE
        # ===============================

        spring.jpa.show-sql=true
        spring.jpa.hibernate.ddl-auto=update
        spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect


        # Fix Postgres JPA Error:
        # Method org.postgresql.jdbc.PgConnection.createClob() is not yet implemented.
        spring.jpa.properties.hibernate.temp.use_jdbc_metadata_defaults=false
</div>
<div>
        <div><h4> &nbsp; 3.2.4 : application.properties (Oracle) </h4></div>
        
        spring.thymeleaf.cache=false

        # ===============================
        # DATABASE
        # ===============================

        spring.datasource.driver-class-name=oracle.jdbc.driver.OracleDriver

        spring.datasource.url=jdbc:oracle:thin:@localhost:1521:db12c
        spring.datasource.username=social
        spring.datasource.password=12345


        # ===============================
        # JPA / HIBERNATE
        # ===============================

        spring.jpa.show-sql=true
        spring.jpa.hibernate.ddl-auto=update
        spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.Oracle10gDialect
</div>
   <h3> &nbsp; 3.3  Cấu hình Security & Spring Social </h3> 
   <br> <img width="700px" src="https://s1.o7planning.com/vi/11823/images/17246902.png" align="center" alt="GitHub Readme Stats" /> <br>
  
  # 4.  Cập nhật lớp thực thể người dùng và bảng người dùng
  Khi người dùng đăng nhập bằng tài khoản Google của chính mình, ứng dụng sẽ lưu trữ thông tin của người dùng (email và nhà cung cấp xác thực) trong cơ sở dữ liệu - vì vậy chúng tôi cần cập nhật lớp thực thể Người dùng - thêm một trường mới cùng với getter và setter như sau: <br>
  <div>

      package net.codejava;

      import javax.persistence.EnumType;
      import javax.persistence.Enumerated;

      @Entity
      @Table(name = "users")
      public class User {

          ...

          @Enumerated(EnumType.STRING)
          private Provider provider;

          public Provider getProvider() {
              return provider;
          }

          public void setProvider(Provider provider) {
              this.provider = provider;
          }

          ...
      }

  </div>
   <p>Nhà cung cấp là một loại enum, đơn giản như sau: </p>
  <div>
  
    package net.codejava;
 
    public enum Provider {
        LOCAL, GOOGLE
    }
  </div>
   <p>Sau đó, trong cơ sở dữ liệu, chúng ta có một nhà cung cấp cột mới với kiểu dữ liệu là varchar như sau:</p>
   <br> <img width="700px" src="https://www.codejava.net/images/articles/frameworks/springboot/oauth-google/provider_in_users_table.png" alt="GitHub Readme Stats" /> <br>
<br> <p>Các giá trị có thể có cho cột này là LOCAL và GOOGLE (hằng số enum).</p><br>

  # 5.  Cập nhật trang đăng nhập
  Tiếp theo, thêm siêu liên kết Đăng nhập bằng Google vào trang đăng nhập tùy chỉnh của bạn với URL sau:<br>
 <div id="highlighter_191398" class="syntaxhighlighter  xml  "><table border="0" cellpadding="0" cellspacing="0"><tbody><tr><td class="gutter"><div class="line number1 index0 alt2"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">1</font></font></div></td><td class="code"><div class="container"><div class="line number1 index0 alt2"><code class="xml plain">&lt;</code><code class="xml keyword">a</code> <code class="xml color1">th:href</code><code class="xml plain">=</code><code class="xml string">"/@{/oauth2/authorization/google}"</code><code class="xml plain">&gt;Login with Google&lt;/</code><code class="xml keyword">a</code><code class="xml plain">&gt;</code></div></div></td></tr></tbody></table></div>
 Sau đó, trang đăng nhập trông như thế này:<br>
 <br>
 <div align="center">
 <img width="500px" src="https://www.codejava.net/images/articles/frameworks/springboot/oauth-google/Login_Google.png" align="center" alt="GitHub Readme Stats" />
</div> <br>
Để bạn tham khảo, dưới đây là mã của trang đăng nhập: <br>
 <div>
 
       <div>
          <h2>Please Login</h2>
          <br/>
      </div>
      <div>
          <h4><a th:href="/@{/oauth2/authorization/google}">Login with Google</a></h4>   
      </div>
      <div><p>OR</p></div>

      <form th:action="@{/login}" method="post" style="max-width: 400px; margin: 0 auto;">
      <div class="border border-secondary rounded p-3">
          <div th:if="${param.error}">
              <p class="text-danger">Invalid username or password.</p>
          </div>
          <div th:if="${param.logout}">
              <p class="text-warning">You have been logged out.</p>
          </div>
          <div>
              <p><input type="email" name="email" required class="form-control" placeholder="E-mail" /></p>
          </div>
          <div>
              <p><input type="password" name="pass" required class="form-control" placeholder="Password" /></p>
          </div>
          <div>
              <p><input type="submit" value="Login" class="btn btn-primary" /></p>
          </div>
      </div>
      </form>
      
 </div>
 
 
  # 6.  Mã người dùng OAuth tùy chỉnh và các lớp dịch vụ người dùng OAuth
  Tiếp theo, chúng ta cần mã hóa một lớp triển khai giao diện OAuth2User được xác định bởi Spring OAuth2, như sau:<br>
  <div>
  
    package net.codejava;

    import java.util.Collection;
    import java.util.Map;

    import org.springframework.security.core.GrantedAuthority;
    import org.springframework.security.oauth2.core.user.OAuth2User;

    public class CustomOAuth2User implements OAuth2User {

        private OAuth2User oauth2User;

        public CustomOAuth2User(OAuth2User oauth2User) {
            this.oauth2User = oauth2User;
        }

        @Override
        public Map<String, Object> getAttributes() {
            return oauth2User.getAttributes();
        }

        @Override
        public Collection<? extends GrantedAuthority> getAuthorities() {
            return oauth2User.getAuthorities();
        }

        @Override
        public String getName() {  
            return oauth2User.getAttribute("name");
        }

        public String getEmail() {
            return oauth2User.<String>getAttribute("email");     
        }
    }
  </div>
  Ở đây, lớp này bao bọc một thể hiện của lớp OAuth2User, lớp này sẽ được Spring OAuth chuyển qua khi xác thực OAuth thành công. Và lưu ý rằng chúng tôi ghi đè <strong> getName () </strong> và viết mã các phương thức <strong> getEmail () </strong> để trả về tên người dùng và email tương ứng.<br>
  Và tạo một lớp con của <strong> DefaultOAuth2UserService </strong> như sau: <br>
  <div>
  
      package net.codejava;

      import org.springframework.security.oauth2.client.userinfo.DefaultOAuth2UserService;
      import org.springframework.security.oauth2.client.userinfo.OAuth2UserRequest;
      import org.springframework.security.oauth2.core.OAuth2AuthenticationException;
      import org.springframework.security.oauth2.core.user.OAuth2User;
      import org.springframework.stereotype.Service;

      @Service
      public class CustomOAuth2UserService extends DefaultOAuth2UserService  {

          @Override
          public OAuth2User loadUser(OAuth2UserRequest userRequest) throws OAuth2AuthenticationException {
              OAuth2User user =  super.loadUser(userRequest);
              return new CustomOAuth2User(user);
          }

      }
  </div>
  
 Ở đây, chúng tôi ghi đè phương thức <strong> loadUser () </strong> sẽ được Spring OAuth2 gọi khi xác thực thành công và nó trả về một đối tượng<strong> CustomOAuth2User </strong> mới . <br>
 
  # 7.  Định cấu hình bảo mật mùa xuân cho xác thực OAuth2
  Tiếp theo, chúng ta cần cập nhật lớp cấu hình Spring Security để kích hoạt xác thực OAuth cùng với đăng nhập biểu mẫu thông thường. Vì vậy, hãy cập nhật phương thức <strong> cấu hình (HttpSecurity) </strong> như sau:
  <div>
  
        package net.codejava;

        @Configuration
        @EnableWebSecurity
        public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

            ...

            @Override
            protected void configure(HttpSecurity http) throws Exception {
                http.authorizeRequests()
                    .antMatchers("/", "/login", "/oauth/**").permitAll()
                    .anyRequest().authenticated()
                    .and()
                    .formLogin().permitAll()
                    .and()
                    .oauth2Login()
                        .loginPage("/login")
                        .userInfoEndpoint()
                            .userService(oauthUserService);
            }

            @Autowired
            private CustomOAuth2UserService oauthUserService;

        }
        
  </div>
  Lưu ý rằng bắt buộc phải cho phép truy cập công khai vào <strong> URL / oauth / **  </strong>để có thể truy cập vào Google API khi chuyển hướng:
  <div id="highlighter_259123" class="syntaxhighlighter  java"><table border="0" cellpadding="0" cellspacing="0"><tbody><tr><td class="gutter"><div class="line number1 index0 alt2"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">1</font></font></div></td><td class="code"><div class="container"><div class="line number1 index0 alt2"><code class="java plain">.antMatchers(</code><code class="java string">"/oauth/**"</code><code class="java plain">).permitAll()</code></div></div></td></tr></tbody></table></div>
  Như vậy là đủ cho cấu hình OAuth2 cơ bản với Spring Boot. Bây giờ bạn có thể kiểm tra đăng nhập bằng Google, nhưng chúng ta hãy đi xa hơn - hãy đọc các phần bên dưới.<br>
  
  # 8.  Triển khai Trình xử lý Thành công Xác thực
  Bởi vì chúng tôi cần xử lý một số lôgic sau khi đăng nhập thành công bằng Google, ví dụ: cập nhật thông tin người dùng trong cơ sở dữ liệu - vì vậy hãy thêm mã sau để định cấu hình trình xử lý thành công xác thực:<br>
  <div>
  
            http.oauth2Login()
              .loginPage("/login")
              .userInfoEndpoint()
                  .userService(oauthUserService)
              .and()
              .successHandler(new AuthenticationSuccessHandler() {

                  @Override
                  public void onAuthenticationSuccess(HttpServletRequest request, HttpServletResponse response,
                          Authentication authentication) throws IOException, ServletException {

                      CustomOAuth2User oauthUser = (CustomOAuth2User) authentication.getPrincipal();

                      userService.processOAuthPostLogin(oauthUser.getEmail());

                      response.sendRedirect("/list");
                  }
              })
  </div>
  Phương thức<strong> onAuthenticationSuccess () </strong>sẽ được Spring OAuth2 gọi khi đăng nhập thành công bằng Google, vì vậy tại đây chúng tôi có thể thực hiện các lôgic tùy chỉnh của mình - bằng cách sử dụng lớp <strong> UserService </strong> - được mô tả trong phần tiếp theo.<br>
  
  # 9. Đăng ký người dùng mới khi xác thực OAuth thành công
  Chúng tôi triển khai phương thức <strong> processOAuthPostLogin () </strong> trong lớp <strong> UserService</strong> như sau:<br>
  <div>
  
          package net.codejava;

          import org.springframework.beans.factory.annotation.Autowired;
          import org.springframework.stereotype.Service;

          @Service
          public class UserService {

              @Autowired
              private UserRepository repo;

              public void processOAuthPostLogin(String username) {
                  User existUser = repo.getUserByUsername(username);

                  if (existUser == null) {
                      User newUser = new User();
                      newUser.setUsername(username);
                      newUser.setProvider(Provider.GOOGLE);
                      newUser.setEnabled(true);          

                      repo.save(newUser);        
                  }

              }

          }
  </div>
  Tại đây, chúng tôi kiểm tra nếu không tìm thấy người dùng nào trong cơ sở dữ liệu với email đã cho (được truy xuất sau khi đăng nhập thành công với Google), sau đó chúng tôi duy trì một đối tượng Người dùng mới với tên nhà cung cấp là GOOGLE. Bạn cũng có thể viết mã bổ sung để cập nhật thông tin chi tiết của người dùng trong trường hợp người dùng tồn tại trong cơ sở dữ liệu.<br>
Để bạn tham khảo, dưới đây là mã của lớp <strong> UserRepository </strong>:<br>
<div>

        package net.codejava;

        import org.springframework.data.jpa.repository.Query;
        import org.springframework.data.repository.CrudRepository;
        import org.springframework.data.repository.query.Param;

        public interface UserRepository extends CrudRepository<User, Long> {

            @Query("SELECT u FROM User u WHERE u.username = :username")
            public User getUserByUsername(@Param("username") String username);
        }
</div>
  
  # 10. Kiểm tra Đăng nhập bằng Tài khoản Google
  Tải xuống dự án mẫu trong phần Tệp đính kèm bên dưới. Chạy SpringBootSocialJpaApplication và truy cập ứng dụng tại<strong> URL http: // localhost: 8080 </strong>. Nhấp vào Xem tất cả sản phẩm và trang đăng nhập xuất hiện. Nhấp vào Đăng nhập bằng Google, và bạn sẽ thấy biểu mẫu Đăng nhập bằng Google, giống như sau:<br>
  <div align="center">
   <br> <img width="400px" src="https://www.codejava.net/images/articles/frameworks/springboot/oauth-google/Sign_in_with_Google.png"  alt="GitHub Readme Stats" /> <br>
   </div>
   Nhập email và mật khẩu của tài khoản Google của bạn, sau đó bạn sẽ được chuyển hướng đến trang của bạn :<br>
   <div align="center">
   <br> <img width="600px" src="https://www.codejava.net/images/articles/frameworks/springboot/oauth-google/Product_List_page.png"  alt="GitHub Readme Stats" /> <br>
   </div>
   Nếu bạn thấy trang này, xin chúc mừng! Bạn đã triển khai thành công đăng nhập Google trong ứng dụng Spring Boot với API ứng dụng Spring OAuth2. Lưu ý rằng tên tài khoản Google của bạn được hiển thị trong thông báo chào mừng và hãy kiểm tra bảng người dùng để xem một hàng mới đã được chèn.
   
   
   
   
   
   
