<h1 align=center>Kubernetes 基礎</1>

<h2>什麼是Kubernetes</h2>

Kubernetes是由google所釋出的一種開源系統, 用於管理容器的工作負載及服務, 並容易自動化工作。容器比VM是為更輕量級的, 它具有自己的檔案系統、CPU、Memory、Process等, 並且與基礎架構分開, 因此可以跨雲和不同OS 發行版(註: 因為容器是共享操作業系統, 只能是基於Linux的容器在Linux作業系統上)之間移植。 


<h2>Kubernetes 元件</h2>

一個Kubernetes Cluster是由稱為nodes的一組工作機器所組成, 用來跑容器應用程式, 每一個Cluster至少要有一個工作節點。

![Kubernetes cluster 元件](../img/components-of-kubernetes.svg)
<h4 align=center>Kubernetes cluster 元件圖</h4>

* <h3>Control Plane</h3>
  Control plane會為Cluster做出全域決策, 例如資源的調度、檢查及反應cluster事件, 像是無法满足部署的replicas時, 要啟動新pod。

  基本上Control plane元件可在cluster任何一台機器上進行, 為了方便, 通常會將所有control plane元件放於同一台機器上, 並且不會在上面跑容器。
  
  ***kube-apiserver***
 
  API server是用來曝露Kubernetes API的, API server是Kubernetes control plane的前端, kube-apiserver是Kubernetes API server的實作, 同時它也被設計為可水平擴充, 可以部署多個實例(instances)來平衡流量。

   ***etcd***

   etcd作為Kubernetes所有cluster資料並以Key-value方式的後端儲存。

   ***kube-scheduler***

   監控新建的Pods而未指定node, 會自動幫它找台node讓它跑在上面。

   ***kube-controller-manager***

   這個元件負責運行controller的processes。

   每個controller都是一個單獨的process, 但是為了減低複雜性, 它們都被編譯同一個可執行檔, 並在同一個process運行。
    
   controller有很多不同類型, 以下是一些例子:
    * Node controller: 負責在節點出現故障時進行通知和反應。
    * Job controller: 監控那些一次性任務的工作物件, 並建立pods來運行那些任務直到完成。
    * EndpointSlice controller: 提供服務和pods之間的聯結。
    * ServiceAccount controller: 對新的命名空間建立預設的服務帳號。

   ***Cloud-controller-manager*** 
   與kube-controller-manager類似, 允許你將你的cluster連接到雲服提供商的API上, 在本地環境中不需要它。
  
* <h3>Node</h3>
  節點組件在每個節點上運行, 負責維護運行Pod並提供Kubernetes運行環境。

  ***kubelet***

  kubelet會在cluster每個節點上運行, 它保證容器都運行在Pod中。
  kubelet接收一組通過各類機制提供給它的PodSpec, 確保這些PodSpec中所描述的容器處於運行狀態且健康, kubelet不會管理不是由Kubernetes創建的容器。

  ***kube-proxy***

  kube-proxy維護節點上的一些網路規則, 這些網路規則會允許從cluster內部或外部的網路會話與Pod進行網路通訊。

  ***Container Runtime***

  這個元件使Kubernetes能夠有效運行容器, 它負責管理Kubernetes環境中容器的執行和生命週期, 只要是基於Kubernetes CRI(Container Runtime Interface)的實作就可以, 例如: containerd、CRI-O。

* <h3>Addons</h3>

  插件使用Kubernetes資源([DaemonSet](https://kubernetes.io/zh-cn/docs/concepts/workloads/controllers/daemonset/)、[Deployments](https://kubernetes.io/zh-cn/docs/concepts/workloads/controllers/deployment/))實現cluster 功能, 因為這些插件提供集群級別的功能, 插件中命名空間域的資源屬於kube-system命名空間。

  下列列出幾種插件 完整列表, 請參閱([Addons](https://kubernetes.io/zh-cn/docs/concepts/cluster-administration/addons/))
  * DNS: 為Kubernetes服務提供DNS記錄。
  * Web界面(儀表板): 基於web的用戶界面, 讓用戶可以管理cluster中運行的應用程式及cluster本身, 並進行故障排除。
  * Container Resource Monitoring: 記錄有關container時間序的計量到中央資料庫上, 並提供UI來瀏覽資料。
  * Cluster-level Logging: 將容器的log保存到一個集中的log store中, 並提供搜索和瀏覽。
  * Network Plugins: 是實作容器網路介面(CNI)規範的元件, 它們負責為Pod分配IP 位址, 並使這些Pod能在cluster內部相互通訊。

[Next -> minikube](playground.md)