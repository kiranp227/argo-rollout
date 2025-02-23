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

Create Rollout with rollout.yaml
> kubectl apply -f rollout.yaml
