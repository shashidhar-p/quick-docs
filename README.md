# AMCOP + OAI Orchestration

## 1. Clone the repositories
- ```bash 
  git clone https://gitlab.com/aarna-networks/amcop-deployment/kpt-automation.git
  ```
- ```bash
  git clone https://github.com/nephio-project/test-infra.git
  ```

## 2. Install AMCOP
- ```bash 
  cd kpt-automation
  ```
- ```bash 
  vi defaults.env
  ```

  - Update the docker image tag in the file
    - ``` export AMCOPTAG=${AMCOPTAG:-"v4.1-2024-05-27"} ```
  - Change the K8s installation flag to true
    - ``` export INSTALL_K8s="true" ```
  - Save and close the file

- Execute the “install.sh” script as a sudo user
- ```bash
    sudo bash -x install.sh
  ```
- After the installation you will receive a message like below
  <details>
  <summary>Expand</summary>
  
  ```sh
  PLAY RECAP *********************************************************************
  127.0.0.1                  : ok=356  changed=95   unreachable=0    failed=0    skipped=22   rescued=0    ignored=0   
  
  Playbook run took 0 days, 0 hours, 19 minutes, 29 seconds
  Wednesday 29 May 2024  11:44:49 +0530 (0:00:00.056)       0:19:29.018 ********* 
  =============================================================================== 
  kpt : Apply package: distros/sandbox/metallb -------------------------- 284.26s
  install_k8s : Initialize Kubernetes cluster --------------------------- 120.25s
  kpt : Apply package: amcop-packages/porch ------------------------------ 95.77s
  kpt : Apply package: amcop-packages/nephio-operator -------------------- 84.99s
  install_k8s : Run get-docker.sh script --------------------------------- 71.84s
  install_amcop : Wait for stock repositories ---------------------------- 52.96s
  kpt : Apply package: amcop-packages/removal-controller ----------------- 32.43s
  install_k8s : Install kubelet, kubeadm, kubectl ------------------------ 30.30s
  install : Wait for deployments in namespace config-management-monitoring -- 26.13s
  kpt : Apply package: nephio/core/configsync ---------------------------- 16.63s
  Wait for packages to be applied ---------------------------------------- 12.76s
  kpt : Get package differences between local and upstream: repository ---- 8.81s
  kpt : Get package differences between local and upstream: repository ---- 8.27s
  install_amcop : Deploy Amcop -------------------------------------------- 7.71s
  install_k8s : Update apt cache ------------------------------------------ 7.70s
  kpt : Get package differences between local and upstream: rootsyncAmcop --- 7.68s
  kpt : Get package differences between local and upstream: repository ---- 7.59s
  kpt : Get package differences between local and upstream: repository ---- 7.49s
  kpt : Get package differences between local and upstream: repository ---- 7.45s
  install : Wait for stock repositories ----------------------------------- 6.94s
  Wednesday 29 May 2024  11:44:49 +0530 (0:00:00.059)       0:19:29.019 ********* 
  =============================================================================== 
  kpt ------------------------------------------------------------------- 725.51s
  install_k8s ----------------------------------------------------------- 252.37s
  install ---------------------------------------------------------------- 72.28s
  install_amcop ---------------------------------------------------------- 70.74s
  ansible.builtin.async_status ------------------------------------------- 12.76s
  andrewrothstein.kubectl ------------------------------------------------- 7.08s
  andrewrothstein.kpt ----------------------------------------------------- 6.68s
  ansible.builtin.pip ----------------------------------------------------- 5.87s
  ansible.builtin.k8s ----------------------------------------------------- 5.02s
  ansible.builtin.unarchive ----------------------------------------------- 3.80s
  gather_facts ------------------------------------------------------------ 2.18s
  andrewrothstein.unarchive-deps ------------------------------------------ 1.94s
  andrewrothstein.git ----------------------------------------------------- 1.81s
  ansible.builtin.include_role -------------------------------------------- 0.51s
  ansible.builtin.stat ---------------------------------------------------- 0.43s
  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
  total ---------------------------------------------------------------- 1168.99s
  + echo 'Done installing Nephio Sandbox Environment'
  Done installing Nephio Sandbox Environment
  
  ```
  </details>

Wait until all the pods come up, it will take around 10-20 minutes
<details>
  <summary>Example output</summary>
  
```sh
NAMESPACE                      NAME                                                            READY   STATUS      RESTARTS        AGE
amcop-system                   aai-grpc-74945d6979-7k7fv                                       1/1     Running     0               9m32s
amcop-system                   aes-backend-6b5fb84c4-v8p27                                     1/1     Running     0               9m32s
amcop-system                   aes-frontend-55784c5c9-pwwtg                                    1/1     Running     0               9m32s
amcop-system                   amcop-minio-pool-0-0                                            2/2     Running     0               3m44s
amcop-system                   auth-gateway-7d77775675-57ntf                                   1/1     Running     0               9m32s
amcop-system                   bootstrap-cluster-controller-manager-5967f975c5-9m84d           2/2     Running     0               8m51s
amcop-system                   bootstrap-secret-controller-manager-bdcc844c9-q7dr7             2/2     Running     0               8m50s
amcop-system                   cassandra-0                                                     1/1     Running     0               8m54s
amcop-system                   console-7755785744-j66kl                                        1/1     Running     0               8m53s
amcop-system                   cortex-alertmanager-6f4d766c75-g2xf4                            1/1     Running     6 (4m12s ago)   8m53s
amcop-system                   cortex-compactor-0                                              1/1     Running     0               8m52s
amcop-system                   cortex-distributor-56f7759cb4-gckk8                             1/1     Running     0               8m52s
amcop-system                   cortex-distributor-56f7759cb4-qk6vf                             1/1     Running     0               8m52s
amcop-system                   cortex-ingester-858b77b474-7ml6g                                1/1     Running     0               8m52s
amcop-system                   cortex-ingester-858b77b474-bvssw                                1/1     Running     0               8m52s
amcop-system                   cortex-ingester-858b77b474-q74rj                                1/1     Running     0               8m52s
amcop-system                   cortex-nginx-7c64f9bd87-btsgh                                   1/1     Running     0               8m52s
amcop-system                   cortex-nginx-7c64f9bd87-n7tdz                                   1/1     Running     0               8m52s
amcop-system                   cortex-querier-84f8c975fb-pqknz                                 1/1     Running     0               8m52s
amcop-system                   cortex-querier-84f8c975fb-t9z8j                                 1/1     Running     0               8m52s
amcop-system                   cortex-query-frontend-859b94dc9b-4fncz                          1/1     Running     0               8m52s
amcop-system                   cortex-query-frontend-859b94dc9b-8qplw                          1/1     Running     0               8m52s
amcop-system                   cortex-ruler-fccc45cc5-s5klh                                    1/1     Running     0               8m52s
amcop-system                   cortex-store-gateway-0                                          1/1     Running     4 (4m19s ago)   8m52s
amcop-system                   dev-aai-57799cd667-qkl4w                                        0/1     Init:0/1    0               8m55s
amcop-system                   dev-aai-graphadmin-6ff6dcdd49-hpflw                             0/1     Running     0               8m55s
amcop-system                   dev-aai-graphadmin-create-db-schema-fg5k2                       0/1     Completed   0               8m54s
amcop-system                   dev-aai-resources-597958b955-shl8b                              0/1     Running     0               8m55s
amcop-system                   dev-aai-schema-service-649dbf865-zn7xq                          1/1     Running     0               8m54s
amcop-system                   dev-aai-traversal-6555df8f8-nblpp                               0/1     Running     0               8m54s
amcop-system                   dev-aai-traversal-update-query-data-lcch2                       0/1     Init:0/1    0               8m54s
amcop-system                   filenotifier-5877555cf4-r9ldj                                   1/1     Running     0               8m49s
amcop-system                   grafana-operator-77c95d8d99-vtpdj                               2/2     Running     0               8m52s
amcop-system                   infra-manager-7cc689d9c5-l6q2p                                  1/1     Running     3 (65s ago)     9m6s
amcop-system                   minio-operator-6d5d794bc6-64j9b                                 1/1     Running     0               8m51s
amcop-system                   minio-operator-6d5d794bc6-7rtf5                                 1/1     Running     0               8m51s
amcop-system                   mongo-0                                                         1/1     Running     0               10m
amcop-system                   postgres-postgresql-0                                           1/1     Running     0               10m
amcop-system                   prometheus-operator-6697d7d6c6-94lc2                            1/1     Running     0               8m51s
backend-system                 resource-backend-controller-6cd776fc8d-wvgbx                    2/2     Running     0               24m
config-management-monitoring   otel-collector-666789794-qmd2r                                  1/1     Running     0               14m
config-management-system       config-management-operator-6fdf9cdd5f-whxms                     1/1     Running     0               14m
config-management-system       reconciler-manager-7954576797-rm7hs                             2/2     Running     0               14m
config-management-system       root-reconciler-aai-5c4b68b87c-8xln6                            4/4     Running     0               10m
config-management-system       root-reconciler-bootstrap-cluster-controller-7bd56bf6bb-slgdw   4/4     Running     0               10m
config-management-system       root-reconciler-bootstrap-secret-controller-7b9fc76fdb-b5n4l    4/4     Running     0               10m
config-management-system       root-reconciler-file-notifier-7f8f4d6fc9-7mdb4                  4/4     Running     0               10m
config-management-system       root-reconciler-inframgr-d75576b9d-c8m7g                        4/4     Running     0               10m
config-management-system       root-reconciler-mgmt-5df66ffd6-g6dc6                            4/4     Running     0               12m
config-management-system       root-reconciler-observability-stack-67d9969866-b24rn            4/4     Running     0               10m
config-management-system       root-reconciler-policy-5d96f8cbc4-n49fb                         4/4     Running     0               11m
config-management-system       root-reconciler-saasdb-856b585956-zpxmx                         4/4     Running     0               10m
config-management-system       root-reconciler-saasui-596bb78c56-wgsdz                         4/4     Running     0               10m
default                        removal-controller-controller-manager-54fddbf946-l2fj6          2/2     Running     0               15m
gitea                          gitea-0                                                         1/1     Running     0               24m
gitea                          gitea-memcached-658796775c-bkdn4                                1/1     Running     0               24m
gitea                          gitea-postgresql-0                                              1/1     Running     0               24m
kube-system                    coredns-76f75df574-br9gz                                        1/1     Running     0               24m
kube-system                    coredns-76f75df574-qmsfw                                        1/1     Running     0               24m
kube-system                    etcd-e2e-88-228                                                 1/1     Running     0               24m
kube-system                    kube-apiserver-e2e-88-228                                       1/1     Running     0               24m
kube-system                    kube-controller-manager-e2e-88-228                              1/1     Running     0               24m
kube-system                    kube-proxy-d42mq                                                1/1     Running     0               24m
kube-system                    kube-scheduler-e2e-88-228                                       1/1     Running     0               24m
kube-system                    weave-net-d7wxb                                                 2/2     Running     2 (23m ago)     24m
local-path-storage             local-path-provisioner-6d9d9b57c9-frvz4                         1/1     Running     0               24m
metallb-system                 controller-6db6d84dc7-2cn6h                                     1/1     Running     0               24m
metallb-system                 speaker-f9429                                                   1/1     Running     0               24m
nephio-system                  nephio-controller-5c7cc9c998-g8q2v                              2/2     Running     0               16m
nephio-system                  token-controller-84d8c469b8-bkb6h                               2/2     Running     0               16m
network-config                 network-config-controller-b78cb5fb6-nhr5w                       2/2     Running     0               14m
porch-fn-system                apply-replacements-85913d4e                                     1/1     Running     0               18m
porch-fn-system                apply-setters-4d429572                                          1/1     Running     0               18m
porch-fn-system                create-setters-0220cc87                                         1/1     Running     0               18m
porch-fn-system                enable-gcp-services-a56558d5                                    1/1     Running     0               18m
porch-fn-system                ensure-name-substring-33ff7d9a                                  1/1     Running     0               18m
porch-fn-system                export-terraform-571fedd3                                       1/1     Running     0               18m
porch-fn-system                gatekeeper-a1c19318                                             1/1     Running     0               18m
porch-fn-system                generate-folders-4511c517                                       1/1     Running     0               18m
porch-fn-system                kubeval-bb88cbcc                                                1/1     Running     0               18m
porch-fn-system                remove-local-config-resources-2825914e                          1/1     Running     0               18m
porch-fn-system                search-replace-6864b06b                                         1/1     Running     0               18m
porch-fn-system                set-annotations-76d65ebd                                        1/1     Running     0               18m
porch-fn-system                set-enforcement-action-aea09be3                                 1/1     Running     0               18m
porch-fn-system                set-image-49c7a466                                              1/1     Running     0               18m
porch-fn-system                set-labels-0dfc3c8a                                             1/1     Running     0               18m
porch-fn-system                set-namespace-f930d924                                          1/1     Running     0               18m
porch-fn-system                set-project-id-966c9e51                                         1/1     Running     0               18m
porch-fn-system                starlark-270a394f-z5w22                                         1/1     Running     0               10m
porch-fn-system                starlark-6ba3971c                                               1/1     Running     0               18m
porch-fn-system                upsert-resource-77196bdb                                        1/1     Running     0               18m
porch-system                   function-runner-67d4c7c7b-jlpkx                                 1/1     Running     0               18m
porch-system                   function-runner-67d4c7c7b-sc6cf                                 1/1     Running     0               18m
porch-system                   porch-controllers-76d67fd966-t574s                              1/1     Running     0               18m
porch-system                   porch-server-6f7c4c5684-l6rmv                                   1/1     Running     0               18m
resource-group-system          resource-group-controller-manager-659f6b9d4b-xv976              3/3     Running     0               14m
```
</details>

<details>
  <summary>Example output</summary>

edit porch images 
```sh
root@e2e-88-228:~# kubectl get pods -n porch-system
NAME                                 READY   STATUS    RESTARTS        AGE
function-runner-67d4c7c7b-jlpkx      1/1     Running   0               28m
function-runner-67d4c7c7b-sc6cf      1/1     Running   0               28m
porch-controllers-76d67fd966-t574s   1/1     Running   1 (2m36s ago)   28m
porch-server-6f7c4c5684-l6rmv        1/1     Running   1 (21s ago)     28m
```
</details>

edit nephio controller image
<details>
  <summary>Example output</summary>
  
```sh
NAME                                 READY   STATUS    RESTARTS      AGE
nephio-controller-5c7cc9c998-g8q2v   1/2     Running   1 (15s ago)   30m
token-controller-84d8c469b8-bkb6h    2/2     Running   0             30m
```
</details>
