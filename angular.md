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
### Calling Rest services
Utilize the `HttpClient` component from Anuglar -
```
import { HttpClient } from '@angular/common/http';
```

Calls look like this in the app.component.ts:
```
  constructor(private http: HttpClient) {}

  ngOnInit() {
    this.getAsset('');
  }

  // Read all REST Items
  getAsset(manu): void {
    this.restItemsUrl = '/asset/' + 'dell';
    this.restItemsServiceGetRestItems()
      .subscribe(
        restItems => {
          this.restItems = restItems;
          console.log(this.restItems);
        }
      )
  }

  // Rest Items Service: Read all REST Items
  restItemsServiceGetRestItems() {
    return this.http
      .get<any[]>(this.restItemsUrl)
      .pipe(map(data => data));
  }
```

Always have to perform imports in the module.ts:
```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { HttpClientModule } from '@angular/common/http';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    HttpClientModule,
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```
