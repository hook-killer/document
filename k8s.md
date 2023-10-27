```
https://guide.ncloud-docs.com/docs/k8s-iam-auth-ncp-iam-authenticator
install tutorial

curl -o ncp-iam-authenticator -L https://github.com/NaverCloudPlatform/ncp-iam-authenticator/releases/latest/download/ncp-iam-authenticator_darwin_arm64
chmod +x ./ncp-iam-authenticator
mkdir -p $HOME/bin && cp ./ncp-iam-authenticator $HOME/bin/ncp-iam-authenticator && export PATH=$PATH:$HOME/bin
echo 'export PATH=$PATH:$HOME/bin' >> ~/.zshrc

ncp-iam-authenticator create-kubeconfig --region KR --clusterUuid 7b68142a-666f-44cb-a879-d9f45288cf31 kubeconfig.yaml
ncp-iam-authenticator create-kubeconfig --region KR --clusterUuid 7b68142a-666f-44cb-a879-d9f45288cf31 --clusterName hook-killer-k8s-pub kubeconfig.yaml

ncp-iam-authenticator update-kubeconfig --clusterName hook-killer-k8s-pub kubeconfig-7b68142a-666f-44cb-a879-d9f45288cf31.yaml

kubectl get node --kubeconfig ~/Desktop/Study/ncloud/k8s/kubeconfig.yaml
kubectl --kubeconfig ~/Desktop/Study/ncloud/k8s/kubeconfig.yaml get nodes

kubectl --kubeconfig kubeconfig-7b68142a-666f-44cb-a879-d9f45288cf31.yaml get nodes
kubectl --kubeconfig kubeconfig.yaml get nodes


kubectl cluster-info --kubeconfig=~/Desktop/Study/ncloud/k8s/kubeconfig.yaml

```
