# argo-rollout
Install Argo-rollout Controller in k8s cluster:
> kubectl create namespace argo-rollouts

> kubectl apply -n argo-rollouts -f https://github.com/argoproj/argo-rollouts/releases/latest/download/install.yaml


Install plugin for argo-rollout:
> VERSION=$(curl -s https://api.github.com/repos/argoproj/argo-rollouts/releases/latest | jq -r .tag_name)

> curl -sSL https://github.com/argoproj/argo-rollouts/releases/download/$VERSION/kubectl-argo-rollouts-linux-amd64 -o /usr/local/bin/kubectl-argo-rollouts

> chmod +x /usr/local/bin/kubectl-argo-rollouts

Validate the Installation:
> kubectl argo rollouts version

Create 2 services as below:
> kubectl apply -f svc1.yaml
> kubectl apply -f svc2.yaml

Create Rollout with rollout.yaml
> kubectl apply -f rollout.yaml

At this time, image is "argoproj/rollouts-demo:blue" and both servcies serve blue pages in the browser.

now, manually update the image in the rollout object to "argoproj/rollouts-demo:blue".

1. Beginning at a fully promoted, steady-state, a revision 1 ReplicaSet is pointed to by both the activeService and previewService.
2. A user initiates an update by modifying the pod template (spec.template.spec).
3. The revision 2 ReplicaSet is created with size 0.
4. The previewService is modified to point to the revision 2 ReplicaSet. The activeService remains pointing to revision 1.
5. The revision 2 ReplicaSet is scaled to either spec.replicas or previewReplicaCount if set.
6. Once revision 2 ReplicaSet Pods are fully available, prePromotionAnalysis begins.
7. Upon success of prePromotionAnalysis, the blue/green pauses if autoPromotionEnabled is false, or autoPromotionSeconds is non-zero.
8. The rollout is resumed either manually by a user, or automatically by surpassing autoPromotionSeconds.
9. The revision 2 ReplicaSet is scaled to the spec.replicas, if the previewReplicaCount feature was used.
10. The rollout "promotes" the revision 2 ReplicaSet by updating the activeService to point to it. At this point, there are no services pointing to revision 1
11. postPromotionAnalysis analysis begins
12. Once postPromotionAnalysis completes successfully, the update is successful and the revision 2 ReplicaSet is marked as stable. The rollout is considered fully-promoted.
13. After waiting scaleDownDelaySeconds (default 30 seconds), the revision 1 ReplicaSet is scaled down



