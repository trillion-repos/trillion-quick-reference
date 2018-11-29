# Maven
### maven-war-pluing
To specify the WAR WEB-INF contents use the maven-war-pluing:
```
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
```
`direcotry` is where the compiled js and html is located.
`targetPath` is where the files will be coppied to in the war.

### [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin)

To compile the frontend, be that Angular or React.js use the frontend-maven-plugin:
```
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
```
`workingDirectory` is where the UI source code is.
