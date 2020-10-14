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

## Steps to add user for your lab using Htpasswd 

```

    export NumUsers=3
    export password=xxxx
    for value in $( eval echo {1..$NumUsers} ) 
    do
        if [ $value -eq 1 ];
        then
            echo $value=1;
            htpasswd -c -B -b users.htpasswd user0$value $password
        else
            if [ $value -lt 10 ]
            then
                echo creating user0$value;
                htpasswd -b users.htpasswd user0$value $password
            else
                echo creating user$value;
                htpasswd -b users.htpasswd user$value $password
            fi;
        fi;
    done


    for value in $( eval echo {1..$NumUsers} )
        do
            if [ $value -lt 10 ]
            then
                uname=user0$value
            else
                uname=user$value
            fi;
                cat > $uname.yaml <<EOF
                {
                    "apiVersion": "user.openshift.io/v1",
                    "groups": null,
                    "identities": [
                        "workshop_htpasswd_provider:$uname"
                    ],
                    "kind": "User",
                    "metadata": {
                        "name": "$uname"
                    }
                }
EOF
        oc create -f $uname.yaml
        done






for value in $( eval echo {1..$NumUsers} )
    do
        if [ $value -lt 10 ]
        then
            uname=user0$value
        else
            uname=user$value
        fi;
        oc adm policy add-cluster-role-to-user basic-user $uname
done

```
Create the htpasswd login

```
oc create secret generic workshop-htpass-secret --from-file=htpasswd=./users.htpasswd -n openshift-config

oc patch Oauth/cluster --patch '{"spec":{"identityProviders": [{"htpasswd": {"fileData": {"name": "workshop-htpasswd-secret"}},"mappingMethod": "claim","name": "workshop_htpasswd_provider","type": "HTPasswd"}]}}' --type merge

```

or in yaml format, to copy and paste into the console

```
    -  htpasswd:
        fileData:
          name: workshop-htpasswd-secret
      login: true
      mappingMethod: claim
      name: workshop_htpasswd_provider
      type: HTPasswd

```
