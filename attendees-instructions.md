## Get the K3s configuration

 - `wget http://51.210.145.31:8080/kubeconfig/xxx`, for example _wget http://51.210.145.31:8080/kubeconfig/001_
 - `mv config-k3s<xxx> ./kubeconfig.yml`, for example _mv config-k3s001_
 - `export KUBECONFIG=./kubeconfig.yml`
 - `kubectl cluster-info`