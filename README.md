# RHOAR Vert.x ConfigMap
Vert.x ConfigMap Microservice - RHOAR course

## Compile Application
```
mvn clean package
```

## Openshift Deployment
```
export CONFIGMAP_PROJECT_NAME=configmap-demo
oc new-project $CONFIGMAP_PROJECT_NAME
oc policy add-role-to-user view -n $CONFIGMAP_PROJECT_NAME -z default
oc create configmap app-config --from-file=app-config.yml
oc get configmap app-config -o yaml
mvn clean fabric8:deploy -Popenshift
```

### Deployment Test
```
export CONFIGMAP_DEMO_URL=http://$(oc get route configmap-vertx -n $CONFIGMAP_PROJECT_NAME -o template --template='{{.spec.host}}')
curl "${CONFIGMAP_DEMO_URL}/api/greeting"
```

### Edit ConfigMap
```
oc edit configmap app-config
curl "${CONFIGMAP_DEMO_URL}/api/greeting"
```
