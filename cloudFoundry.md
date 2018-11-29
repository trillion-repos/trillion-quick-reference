# CloudFoundry

### Commands

```
cf login -a https://api.run.pivotal.io
  
cf push <name> -p target
 
 
cf create-service cleardb spark mysql
cf env <"app">
cf marketplace -s elephantsql
 
cf create-service elephantsql turtle cf-spring-db
 
cf bind-service cf-spring cf-spring-db
cf service
```

### manifest
A manifest.yml is needed for CF to know what kind of resources are needed for this application.
```
---
applications:
- name: cf-spring
  memory: 768M
  instances: 1
  random-route: true
```
