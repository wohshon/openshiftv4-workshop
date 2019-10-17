# openshiftv4-workshop

Developer workshop for OCP 4.1

## Generating the lab instructions

List down the modules you want to include in the lab instructions (see `_modules.yml` for example)

Update the url placeholders in `_module_groups.yml` and `_modules.yml`, check in.


```
oc new-project labs

oc new-app --name=dev-guide osevg/workshopper -e CONTENT_URL_PREFIX="https://raw.githubusercontent.com/wohshon/openshiftv4-workshop/4.2" -e WORKSHOPS_URLS="https://raw.githubusercontent.com/wohshon/openshiftv4-workshop/4.2/_module_groups.yml"


oc expose svc/dev-guide
```

Use the route generated to access the labs instructions
