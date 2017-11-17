`oc cluster up    --public-hostname=IP_ADDR`
`cd /opt/openshift-cd-demo/`
`#git branch -a`
`#vim cicd-template.yaml `
`oc new-project dev --display-name="Tasks - Dev"`
`oc new-project stage --display-name="Tasks - Stage"`
`oc new-project cicd --display-name="CI/CD"`
`oc policy add-role-to-user edit system:serviceaccount:cicd:jenkins -n dev`
`oc policy add-role-to-user edit system:serviceaccount:cicd:jenkins -n stage`
`oc process -f cicd-template.yaml | oc create -f - -n cicd`
`oc login -u system:admin`
`oc create -f https://raw.githubusercontent.com/jboss-openshift/application-templates/master/jboss-image-streams.json -n openshift`
`oc login -u developer`
`#oc create -f https://raw.githubusercontent.com/OpenShiftDemos/nexus/master/nexus2-template.yaml`
`#oc create -f https://raw.githubusercontent.com/OpenShiftDemos/nexus/master/nexus2-persistent-template.yaml`
`#oc new-app nexus2 -p NEXUS_VERSION=2.14.3`
`oc new-app sonatype/nexus`
`oc expose svc/nexus`
`oc set probe dc/nexus \`
  `--liveness \`
  `--failure-threshold 3 \`
  `--initial-delay-seconds 30 \`
  `-- echo ok`
`oc set probe dc/nexus \`
  `--readiness \`
  `--failure-threshold 3 \`
  `--initial-delay-seconds 30 \`
  `--get-url=http://:8081/nexus/content/groups/public  `
`oc set resources dc/jenkins --limits=memory=1Gi --requests=memory=512Mi -n cicd`

`To View FullList of Matches `
`oc new-app -S nexus`
`oc new-app --image-stream=cicd/nexus:2.14.3`

