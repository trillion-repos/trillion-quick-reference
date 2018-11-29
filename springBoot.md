# SpringBoot

### Basic Stucutre
```
project
└───src
    └───main
	└───java
	|   └───com.trillion.example
	|	└───domain
        |           |	ExampleEntity
	|	└───repository
        |           |	ExampleRepository(Interface)
	|	└───service
        |           |	ExampleService
	|	└───web
        |           |	ExampleException
        |           |	ExampleController
        |        ExampleApplicaion
	└───resources
   └───test
	└───java
	    └───com.trillion.example

```

### Commonly used Annotations

| Annotation                        | Description
|-------------------------------    |--------------
|`@RunWith(SpringRunner.class)`     | @SpringJUnit4ClassRunner alias; add for junit test support
|   	                              |
|`@SpringBootTest`  	              | Bootstrap test with SpringBoot support, load application.properties; specify random or specific port to start app; TestRestTemplate bean made available;
|                                   |
|`@WebMvcTest`   	                  | Use in combination with SpringRunner to load context relevant spring mvc components
|                                   |
|`@RunWith(MockitoJUnitRunner.class)`| Initializes mocks so no need to initMocks(this); automatic validation of framework usage
|                                   |
|`@DataJpaTest`   	                | Loads jpa relevant config; uses in-memory db by default, override with `@AutoConfigureTestDatabase`
|   	                            |
|`@AutoConfigureTestDatabase`   	  | If you do not want to use auto-configured test database, use this to configure a test db
|  	                                |
|`@MockBean`                        | Use with SpringRunner class to mock components in test
|                                   |
|`@Mock`    	                      | Similar to @MockBean but without spring support; use with MockitoJUnitRunner
|  	                                |
|`@AutoConfigureMockMvc`            | More control of mock-mvc, disable spring security bits etc
|                                   |
|`@WebFluxTest`                     | Use in combination with SpringRunner to load context relevant spring WebFlux components
|                                   |
|`@DataMongoTest`                   | Use in combination with SpringRunner for testing MongoDB components; uses in-memory MongoDB by default

### Testing Resources
A `resources` directory can be created within the `test` directory of a project to list out different resources for testing.  One important one is a specific test `application.properties`.  Where you can set up different database connections only for testing usage.

### Testing restAPI
```
@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class ApplicationTests {

	@Autowired
	private TestRestTemplate restTemplate;
	
	@Test
	public void test() {

		ResponseEntity<Test> assetResponseEntity = restTemplate.getForEntity("/test/{test}",Test.class,"test");
		assertThat(assetResponseEntity.getStatusCode()).isEqualTo(HttpStatus.OK);
		assertThat(assetResponseEntity.getBody().getTest()).isEqualTo("test");


	}
```

### Testing Controller
```
@RunWith( SpringRunner.class )
@WebMvcTest
public class ControllerTests {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private YourService yourService;

    @Test
    public void test() throws Exception {
        when(this.assetTrackerService.methodCalled()).thenReturn(new Entity);

        this.mockMvc.perform(get("/test/{test}","test"))
                .andExpect(status().isOk())
                .andExpect(jsonPath("test").value("test"));
    }

}
```

### Application Properites
Sample:
```
## Spring DATASOURCE (DataSourceAutoConfiguration & DataSourceProperties)
spring.datasource.url= jdbc:postgresql://localhost:5432/assetDB
spring.datasource.username=postgres
spring.datasource.password=postgres@123

# The SQL dialect makes Hibernate generate better SQL for the chosen database
spring.jpa.properties.hibernate.dialect = org.hibernate.dialect.PostgreSQLDialect

spring.jpa.properties.hibernate.jdbc.lob.non_contextual_creation=true

# Hibernate ddl auto (create, create-drop, validate, update)
spring.jpa.hibernate.ddl-auto = update
```

### Sample SpringBoot POM with frontend
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.trillion</groupId>
	<artifactId>assetTacker</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>war</packaging>

	<name>assetTacker</name>
	<description>asset</description>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.1.0.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
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
			<artifactId>spring-boot-starter-data-rest</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-hateoas</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>com.h2database</groupId>
			<artifactId>h2</artifactId>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.postgresql</groupId>
			<artifactId>postgresql</artifactId>

		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
			<plugin>
				<artifactId>maven-war-plugin</artifactId>
				<configuration>
					<webResources>
						<resource>
							<directory>src/main/ui/dist/ui</directory>
							<targetPath>WEB-INF</targetPath>
						</resource>
					</webResources>
				</configuration>
			</plugin>

			<plugin>
				<groupId>com.github.eirslett</groupId>
				<artifactId>frontend-maven-plugin</artifactId>

				<configuration>
					<workingDirectory>./src/main/ui</workingDirectory>
				</configuration>
				<executions>

					<execution>
						<!-- optional: you don't really need execution ids,
                        but it looks nice in your build log. -->
						<id>install node and npm</id>
						<goals>
							<goal>install-node-and-npm</goal>
						</goals>
						<configuration>
							<nodeVersion>v10.11.0</nodeVersion>
							<npmVersion>6.4.1</npmVersion>
						</configuration>
					</execution>

					<execution>
						<id>npm install</id>
						<goals>
							<goal>npm</goal>
						</goals>

						<!-- optional: default phase is "generate-resources" -->
						<phase>generate-resources</phase>

						<configuration>
							<!-- optional: The default argument is actually
                            "install", so unless you need to run some other npm command,
                            you can remove this whole <configuration> section.
                            -->
							<arguments>install</arguments>
						</configuration>
					</execution>

					<execution>
						<id>npm run build</id>
						<goals>
							<goal>npm</goal>
						</goals>
						<phase>generate-resources</phase>
						<configuration>
							<arguments>run build</arguments>
						</configuration>
					</execution>

				</executions>
			</plugin>
		</plugins>
	</build>
</project>
```

