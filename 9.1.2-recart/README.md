# Sentry

## Push new version to ECR
```bash
docker build -t sentry -f Dockerfile .
docker tag sentry:latest 281674856106.dkr.ecr.us-east-1.amazonaws.com/recart/sentry:<tag>
docker rmi sentry:latest
$(aws ecr get-login --no-include-email --region us-east-1)
docker push  281674856106.dkr.ecr.us-east-1.amazonaws.com/recart/sentry:<tag>
```

## Notes
 - If you use a new Sentry Docker image (>=9.1.2), pay attention to install **sentry-auth-github** as it is not included in the base image (Altough they claim it is).
 - If you use a new Sentry Docker image (>=9.1.2), double check the supported ENV vars beacuse they like to deprecate variables without notice.
 - In the kubernetes cluster, Traefik does the ingress for Sentry in **public-production**
 - If after a new deployment you see random Cloudflare 502, or error messages starting with "OOps", then it is possible that K8s nodes has cached an old or wrong version of the image. In that case, edit the deployment and set `imagePullPolicy` to `Always`.