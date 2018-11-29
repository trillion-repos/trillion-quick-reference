# Angular

## Installaion
It is assumed that nodejs/npm is installed in the system:
```
npm install -g @angular/cli
```

## Common commands
### Generate new Application
```
ng new <appname>
```
### Generate new Component
```
ng generate component <component name>

//shortcut
ng g c <component name> 
ng g s <service name>
```
### Run development server
```
ng serve
```

### Production builds
In package.json add a scrip section for prod:
```
  "scripts": {
    "ng": "ng",
    "start": "ng serve",
    "build": "ng build",
    "test": "ng test",
    "lint": "ng lint",
    "e2e": "ng e2e",
    "prod": "ng build --prod"
  },
```
Then either call from command line or if calling from the maven frontend builder make sure to call it instead of normal buidl:
```
<execution>
						<id>npm run build</id>
						<goals>
							<goal>npm</goal>
						</goals>
						<phase>generate-resources</phase>
						<configuration>
							<arguments>run prod</arguments>
						</configuration>
					</execution>
```
