# Release Process

Make sure the [end-to-end test](https://github.com/Stackdriver/stackdriver-prometheus-e2e) passes:
```sh
make DOCKER_IMAGE_TAG=$USER push
( cd kube ; GCP_REGION=us-central1-a GCP_PROJECT=prometheus-to-sd KUBE_CLUSTER=integration-cluster KUBE_NAMESPACE=$USER SIDECAR_IMAGE_TAG=$USER ./deploy.sh )
( cd ../stackdriver-prometheus-e2e ; make CLUSTER_NAME=integration-cluster START_PROMETHEUS=false )
kubectl delete namespace $USER
```

`START_PROMETHEUS=false` prevents the old Prometheus integration from starting as part of the test.

If OK, then release by running `./release.sh {VERSION}`
