# docker-kubectl
kubectl docker images
## docker build
```bash
docker build -t ju4t/kubectl:v1.23.5 .
```

## buildctl build
```bash
buildctl build --frontend dockerfile.v0 --local context=. --local dockerfile=. --output type=image,name=ju4t/kubectl:v1.23.5
```
