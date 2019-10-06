# openshiftv4-workshop

Developer workshop for OCP 4.1

## Generating the lab instructions

List down the modules you want to include in the lab instructions (see `_modules.yml` for example)

Update the url placeholders in `_module_groups.yml` and `_modules.yml`

```
oc new-project labs

oc new-app --name=dev-guide osevg/workshopper -e CONTENT_URL_PREFIX="https://raw.githubusercontent.com/wohshon/openshiftv4-workshop/master" -e WORKSHOPS_URLS="https://raw.githubusercontent.com/wohshon/openshiftv4-workshop/master/_module_groups.yml"


oc expose svc/dev-guide
```
