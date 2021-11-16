# CNCF Packaging w/ Carvel webinar

Repo contains material used in https://www.youtube.com/watch?v=us3NhfUfIFk

## External to cluster deployment

1. kapp deploy + yaml

	Deploying basic Deployment+Service
	(Taken from https://github.com/vmware-tanzu/carvel-simple-app-on-kubernetes)
    ```
    kapp deploy -a simple-app -f https://raw.githubusercontent.com/vmware-tanzu/carvel-simple-app-on-kubernetes/develop/config-step-1-minimal/config.yml -c
    ```

    Something more complicated like https://github.com/jetstack/cert-manager/releases/tag/v1.6.1
    ```
    kapp deploy -a cm -f https://github.com/jetstack/cert-manager/releases/download/v1.6.1/cert-manager.yaml
    ```

2. kapp deploy + ytt

    ```
    kapp deploy -a simple-app -f <(ytt -f 2-kapp-ytt/config.yml -f 2-kapp-ytt/overlay.yml)
    ```

3. App CR with config map

    Install kapp-controller
    ```
    kapp deploy -a kc -f https://github.com/vmware-tanzu/carvel-kapp-controller/releases/download/v0.29.0/release.yml
    ```

    ```
    # Deploy RBAC
    kapp deploy -a default-ns-rbac -f 3-app-cr-config-map/rbac/

    # Deploy app
    # App CR spec -- https://carvel.dev/kapp-controller/docs/latest/app-spec/
    kapp deploy -a app -f 3-app-cr-config-map/app.yml -f 3-app-cr-config-map/config.yml

    # Check out app status
    kubectl get app -A -w -oyaml
    ```

    Consider changing ConfigMap `config` to set 4 replicas.

4. App CR with git repo

    ```
    kapp deploy -a app -f 4-app-cr-git/ -c
    ```

5. App CR with helm chart via git repo

    ```
    kapp deploy -a app -f 5-app-cr-helm-chart/ -c
    ```

6. App CR with YAML via imgpkg bundle

    ```
    kapp deploy -a app -f 6-app-cr-imgpkg-bundle/ -c
    ```

    (1) App CR with YAML via imgpkg bundle (dynamic tag selection)
    ```
    kapp deploy -a app -f 6.1-app-cr-imgpkg-bundle/ -c
    ```

7. Package + PackageInstall CR

    ```
    kapp deploy -a app -f 7-pkg-cr/ -c
    ```

    (1) PackageInstall with overlay
    ```
    kapp deploy -a app -f 7.1-pkg-cr-overlay/ -c -p
    ```

8. Package + PackageInstall CR with helm chart via git repo

    ```
    kapp deploy -a app -f 8-pkg-cr-helm-chart/ -c
    ```

9. Package Repository + PackageInstall CR

    ```
    kapp deploy -a app -f 9-pkg-repo-cr/ -c
    ```

    (1) TCE package repository
    ```
    kapp deploy -a app -f 9.1-tce/ -c
    ```
