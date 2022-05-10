SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='us-docker.pkg.dev/fe-asaikali/iterate-apps/adib-tap-demo-source')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='default')

k8s_custom_deploy(
    'adib-tap-demo',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --live-update" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --yes >/dev/null" +
               " && kubectl get workload adib-tap-demo --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes'],
    container_selector='workload',
    live_update=[
      sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)

k8s_resource('adib-tap-demo', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'serving.knative.dev/service': 'adib-tap-demo'}])
