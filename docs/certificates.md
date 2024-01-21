<h2>憑證管理</h2>
由kubeadm生成的憑證會在一年內到期, 因此需要對憑證進行管理。

預設情況下, 憑證存放目錄會在/etc/kubernetes/pki。

<h3>憑證到期檢查</h3>

```shell 
###登入master主機, 以root身份執行
kubeadm certs check-expiration
/*
root@k8s-master:~# kubeadm certs check-expiration
[check-expiration] Reading configuration from the cluster...
[check-expiration] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -o yaml'

CERTIFICATE                EXPIRES                  RESIDUAL TIME   CERTIFICATE AUTHORITY   EXTERNALLY MANAGED
admin.conf                 Dec 30, 2024 08:19 UTC   362d            ca                      no
apiserver                  Dec 30, 2024 08:19 UTC   362d            ca                      no
apiserver-etcd-client      Dec 30, 2024 08:19 UTC   362d            etcd-ca                 no
apiserver-kubelet-client   Dec 30, 2024 08:19 UTC   362d            ca                      no
controller-manager.conf    Dec 30, 2024 08:19 UTC   362d            ca                      no
etcd-healthcheck-client    Dec 30, 2024 08:19 UTC   362d            etcd-ca                 no
etcd-peer                  Dec 30, 2024 08:19 UTC   362d            etcd-ca                 no
etcd-server                Dec 30, 2024 08:19 UTC   362d            etcd-ca                 no
front-proxy-client         Dec 30, 2024 08:19 UTC   362d            front-proxy-ca          no
scheduler.conf             Dec 30, 2024 08:19 UTC   362d            ca                      no
super-admin.conf           Dec 30, 2024 08:19 UTC   362d            ca                      no

CERTIFICATE AUTHORITY   EXPIRES                  RESIDUAL TIME   EXTERNALLY MANAGED
ca                      Dec 28, 2033 08:19 UTC   9y              no
etcd-ca                 Dec 28, 2033 08:19 UTC   9y              no
front-proxy-ca          Dec 28, 2033 08:19 UTC   9y              no
*/
```
<h2>自動更新憑證</h2>
如果你有定期更新kubernetes版本的話, 那麼在更新control plane時就會更新所有憑證, 這也是最建議的作法。

