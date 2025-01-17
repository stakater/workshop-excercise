# Lets deploy our application 
> Forecastle : https://forecastle-stakater-forecastle.apps.devtest.vxdqgl7u.kubeapp.cloud/

In this section, we will deploy the `Nordmart Review UI` Application. `Nordmart Review UI` is a light weight application for management of product reviews. This application also requires backend `Nordmart Review` which is already deployed in your Tenant. This application implements review functionality for the products; it provides CRUD API for reviews.

1. Log In to the cluster & open `<TENANT_NAME>-dev` project from projects. Navigate to `Networking > Routes` from sidebar and copy the `review` route.

    ![console-review-route](images/console-review-route.png)

    Alternatively, you can run the following command in your devspace terminal. 

        REVIEW_API=$(oc get route review --template='{{ .spec.host }}' -n <TENANT>-dev)

3. Make a curl request on the URL copied in the previous step. You should get a similar response as below.

        curl https://$REVIEW_API/api/review/329199 | jq '.body'
    ![curl-response](images/curl-response.png)

Great! Now that we know our `Nordmart Review` backend is working, lets deploy the `Nordmart Review UI`

## Deploy Nordmart Review UI

1. Open terminal on your DevSpace by pressing `` Ctrl+` `` or clicking `Terminal > New Terminal` from top menu, Select `stack-tl500` as container, Select working directory `stakater-nordmart-review-ui`.

2. Make you are in `/projects/stakater-nordmart-review-ui` by running `pwd` 

3. Open the value file `deploy/values.yaml` in the editor and update the `application.deployment.env.REVIEW_API` value with the URL you copied in above section for the curl request.

    ![devspace-deploy-value-update-review-api](images/devspace-deploy-value-update-review-api.png)

4. Before we deploy the application, lets build dependencies of Helm chart in deploy/ folder.

        helm repo add stakater https://stakater.github.io/stakater-charts
        helm dependency build deploy/

    ![devspace-helm-output](images/devspace-helm-output.png)

5. Lets deploy the application by running the following command. 

        helm template deploy/ -n <TENANT>-dev | oc apply -f -

    ![devspace-helm-install-oc-apply](images/devspace-helm-install-oc-apply.png)

## View the application (UI)

6. Navigate to `Networking > Routes` and copy route named `review-web` and open it in your browser. Make sure the project is `<TENANT_NAME>-dev`

    ![console-routes](./images/console-routes.png)

    Alternatively, you can run the following command in your devspace terminal and open it in your browser.

        REVIEW_UI=https://$(oc get route review-web --template='{{ .spec.host }}' -n <TENANT>-dev)/

        echo $REVIEW_UI

    ![nortmart-review-ui](./images/nordmart-review-ui.png)

## 🖼️ Big Picture

Great Work, Now that we have deployed our application, we can move on to our main topic secrets management.


