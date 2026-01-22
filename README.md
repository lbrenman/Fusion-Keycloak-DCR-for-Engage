# Amplify Fusion/Engage OAuth 2.0 Client Provisioning For Keycloak

This Amplify Fusion project contains an integration and associated connector that handle Amplify Engage credential provisioning requests. It enables self-service onboarding for Engage subscribers consuming OAuth 2.0-secured APIs managed by Fusion, allowing them to retrieve their OAuth 2.0 credentials without manual administrator involvement.

The integration leverages Keycloak’s Dynamic Client Registration (DCR) capabilities to automatically create and configure OAuth clients at subscription time.

You should have access to your Keycloak tenant and you should have Fusion integrated with Engage as described [here](https://docs.axway.com/bundle/amplify_integration/page/docs/manager_module/manage_marketplace/index.html).

For testing you can reference these documents:
* [Amplify Integration - Use PhaseTwo Managed Keycloak for OAuth API Authentication](https://gist.github.com/lbrenman/69317b109e0db85771ae29a2fab890c8)
* [Keycloak Development Environment](https://github.com/lbrenman/keycloak-dev-codespace)

Instructions
* [Import](https://docs.axway.com/bundle/amplify_integration/page/docs/manager_module/manage_the_environments/index.html#export-or-import-a-project) the project zip file into your tenant

* [Override](https://docs.axway.com/bundle/amplify_integration/page/docs/designer_module/designer_module_artifacts/connections/index.html#configure-an-override-connection) the Keycloak API connector in Manager and enter the appropriate values for your Keycloak tenant and service account client, namely your realm url, your realm token url and the client id and secret of a service account client

  ![Imgur](https://i.imgur.com/W0AlWTy.png)
  ![Imgur](https://i.imgur.com/DUAi6rJ.png)
  ![Imgur](https://i.imgur.com/fBta2tm.png)
  ![Imgur](https://i.imgur.com/qRdbj1D.png)

* The service account client should have a realm-management -> manage-clients service account role added to it in order to create clients
  ![Imgur](https://i.imgur.com/SoerZOj.png)
  ![Imgur](https://i.imgur.com/SyINPA3.png)
  ![Imgur](https://i.imgur.com/Kzhsbz8.png)
  ![Imgur](https://i.imgur.com/YicvVTg.png)

* Link the integration to your Identity Provider in Fusion -> Manager Identity Provider as follows:
  * Open the Credential Provisioning integration, `cred-prov-flow-keycloak` in the imported project
  * Click the Credential Provision trigger artifact and select your Identity Provider and click Save
    ![Imgur](https://i.imgur.com/BTRpL7u.png)
    ![Imgur](https://i.imgur.com/zbSBI16.png)
  * In Fusion Manager, Open the Idenity provider and select the Credential Provisioning integration you just updated in the Link Integration picker and select the desired data plane and click Save
    ![Imgur](https://i.imgur.com/n4XEvGD.png)
    ![Imgur](https://i.imgur.com/xngYFy7.png)

* Now you can select OAuth 2.0 for any of your Fusion APIs and create a Governance Rule that uses the Identity Provider and activate your API

  ![Imgur](https://i.imgur.com/QR4Q6ye.png)
  ![Imgur](https://i.imgur.com/zftAmYW.png)

* Since we'll be testing the OAuth 2.0 API from Engage, you'll need to configure CORS properly for your API as shown below:

  ![Imgur](https://i.imgur.com/5VQ8KRQ.png)

* When you activate your API it will be discovered in Engage and you can create product based on it

* When Engage users discover the product and associated API, they can subscribe and register an application. Then when the user requests a credential, the credential provisioning integration will trigger and the credentials sent to Engage for the user to use the OAuth 2.0 Fusion API

  ![Imgur](https://i.imgur.com/VZ17n5G.png)
  ![Imgur](https://i.imgur.com/icUelld.png)
  ![Imgur](https://i.imgur.com/R0pibLM.png)
  ![Imgur](https://i.imgur.com/v0k2Vct.png)

  Client Credentials

  ![Imgur](https://i.imgur.com/glv3W1r.png)

  Authorization Flow

  > Note that if you are logged into Keycloak, logout first or you will not be directed to login in Keycloak during the Authorization flow

  ![Imgur](https://i.imgur.com/dbEhAyn.png)
  ![Imgur](https://i.imgur.com/vxFpSjd.png)
  ![Imgur](https://i.imgur.com/25PPOfX.png)
  ![Imgur](https://i.imgur.com/MloPqlY.png)

  Try it out

  ![Imgur](https://i.imgur.com/KuvnSfY.png)

* You will see the new client in Keycloak
  ![Imgur](https://i.imgur.com/lGC7I3l.png)
  ![Imgur](https://i.imgur.com/QQX1a1l.png)
  ![Imgur](https://i.imgur.com/Cjpqro0.png)

* You will also see the client provisioning integration transaction in the Fusion Monitor
  ![Imgur](https://i.imgur.com/xlVllYX.png)
  ![Imgur](https://i.imgur.com/0PaqKnJ.png)
