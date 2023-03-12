SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='harbor.vrabbi.cloud/guy-lab/guy-java-example-web-app-source')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='apps1')
OUTPUT_TO_NULL_COMMAND = os.getenv("OUTPUT_TO_NULL_COMMAND", default=' > /dev/null ')

allow_k8s_contexts('tkg-tap-cls-admin@tkg-tap-cls')

k8s_custom_deploy(
    'guy-java-example-web-app',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --update-strategy replace --debug --live-update" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --yes " +
               OUTPUT_TO_NULL_COMMAND +
               " && kubectl get workload guy-java-example-web-app --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes'],
    container_selector='workload',
    live_update=[
      sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)

k8s_resource('guy-java-example-web-app', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'carto.run/workload-name': 'guy-java-example-web-app', 'app.kubernetes.io/component': 'run'}])
