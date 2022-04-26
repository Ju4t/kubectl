# docker-kubectl
kubectl images
## 构建镜像
### docker
```bash
docker build -t ju4t/kubectl:v1.23.5 .
```

### buildctl
```bash
buildctl build --frontend dockerfile.v0 --local context=. --local dockerfile=. --output type=image,name=ju4t/kubectl:v1.23.5
```

## 用法
### jenkins && k8s
```bash
podTemplate(
    cloud: 'kubernetes',
    namespace: 'devops',
    // idleMinutes: 10, // 执行后，释放时间(分)
    // imagePullSecrets: ''
    containers: [
        containerTemplate(name: 'kubectl', image: 'registry.cn-beijing.aliyuncs.com/ju4t/kubectl', command: 'cat', ttyEnabled: true)
    ],
    volumes: [
        // configMapVolume(configMapName: 'kube-config', mountPath: '/root/.kube/'),
    ]
) { 
    node(POD_LABEL) {
        stage('运行 Kubectl') {
            // 把kube config 文件上传到 凭据 中 
            withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                container('kubectl') {
                    sh "mkdir -p ~/.kube && cp ${KUBECONFIG} ~/.kube/config"
                    sh "kubectl get pod"
                }
            }
        }
    }
}
```
