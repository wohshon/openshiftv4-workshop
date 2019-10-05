# openshiftv4-workshop

Developer workshop for OCP 4.1


oc new-app --name=dev-guide osevg/workshopper -e CONTENT_URL_PREFIX="https://raw.githubusercontent.com/wohshon/openshiftv4-workshop/master" -e WORKSHOPS_URLS="https://raw.githubusercontent.com/wohshon/openshiftv4-workshop/master/_module_groups.yml"
