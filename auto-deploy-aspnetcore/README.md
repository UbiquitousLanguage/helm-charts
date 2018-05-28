# GitLab's Auto-deploy Helm Chart for ASP.NET Core apps

Use provided `gitlab-ci.yml` file, don't forget to add the dot to make it work (it should be `.gitlab-ci.yml` in your repo root).

Difference with the default auto-deploy file:
 - Kubernetes version is 1.10.1
 - Sends the environment name to the new chart, so you will have it in `ASPNETCORE_ENVIRONMENT`
 - Adds this chart repository

For the sake of avoiding issues with temporary registry access tokens, this charts ignores them and exepect to have `REGISTRY_USER` and `REGISTRY_PASSWORD` variables. The best way to use it is to create a personal access token with `read_registry` rights. Upload will still happen with CI variables but Helm chart will create a secret with permanent credentials. This is because the CI token expires as soon as the pipeline finishes and when the replica set is scaled, it won't work. Also, when the pod is deleted, it will not be recovered because of expired credentials. Permanent credentials solve this issue.
