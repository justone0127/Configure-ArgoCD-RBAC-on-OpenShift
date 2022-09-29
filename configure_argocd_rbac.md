## Configure ArgoCD RBAC on OpenShift

### 1. Configure ArgoCD RBAC

기본적으로 RHSSO를 사용하여 ArgoCD에 로그인한 모든 사용자는 읽기 전용 사용자가 됩니다.

이 부분은 `argocd-rbac-cm` ConfigMap 데이터 섹션을 업데이트하여 수정할 수 있습니다.



**1) 현재 설정 확인**

- CLI 명령어로 확인 (편집모드)

  ```bash
  oc edit cm argocd-rbac-cm -n openshift-gitops
  ```

- 기본값 : readonly

  ```yaml
  ...
  metadata
  ...
  ...
  data:
    policy.default: role:readonly
  ```

- Console에서 확인

  - `openshift-gitops`  프로젝트 > Workloads > ConfigMap > argocd-rbac-cm > data 확인

    ![01_rbac_policy](C:\Works\01_자료\01_OCP\05_OCP_Demo_hyou\brown_bag_gitops\gitops-rbac\01_rbac_policy.png)

- admin 권한으로 변경 (CLI로 명령어로 적용)

  ```bash
  oc patch cm/argocd-rbac-cm -n openshift-gitops --type=merge -p '{"data":{"policy.default":"role:admin"}}'
  ```

- Console에서 변경하는 방법

  - `openshift-gitops`  프로젝트 > Workloads > ConfigMap > argocd-rbac-cm > Edit ConfigMap > Save

  - role:readonly -> role:admin

    ![02_rbac_cm_update](C:\Works\01_자료\01_OCP\05_OCP_Demo_hyou\brown_bag_gitops\gitops-rbac\02_rbac_cm_update.png)

  - 적용 확인

    ![03_rbac_policy_confirm](C:\Works\01_자료\01_OCP\05_OCP_Demo_hyou\brown_bag_gitops\gitops-rbac\03_rbac_policy_confirm.png)

    > policy가 admin으로 수정된 이후에는 openshift 계정으로 로그인하여 ArgoCD 인스턴스에서 리소스를 생성 할 수 있습니다.

  - LOG IN VIA OPENSHFIT

    ![04_argocd_login_openshift](C:\Works\01_자료\01_OCP\05_OCP_Demo_hyou\brown_bag_gitops\gitops-rbac\04_argocd_login_openshift.png)

