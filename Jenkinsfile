@Library ('jenkins-shared-library') _

def configMap = [
    project: "roboshop",
    component: "catalogue",
]

if (env.BRANCH_NAME == 'main') {
    echo "Please follow the CR process"
}
else {
    nodeJSEKSpipeline(configMap)
}