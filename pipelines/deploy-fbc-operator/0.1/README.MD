# "deploy-fbc-operator pipeline"
The deploy-fbc-operator pipeline automates the provisioning of a EaaS Hypershift cluster and the deployment of an operator from a given FBC (File-Based Catalog) fragment. It automates the process of retrieving an unreleased bundle from the FBC, selecting an appropriate OpenShift version and architecture for cluster provisioning, installing the operator, and gathering cluster artifacts.

## Pipeline Flow
The pipeline consists of the following tasks:

1. **Parse Metadata (`parse-metadata`)**  
   Extracts metadata from the provided *snapshot*, including:
     * FBC fragment container image
     * Source Git URL
     * Git revision

2. **Provision EAAS Space (`provision-eaas-space`)**  
   Allocates an **Environment-as-a-Service (EaaS) space** for cluster provisioning.

3. **Get Unreleased Bundle (`get-unreleased-bundle`)**
   - Retrieves the **unreleased bundle** from the FBC fragment.
   - Processes **.tekton/images-mirror-set.yaml** to resolve mirrored image references, if available.

4. **Provision Cluster (`provision-cluster`)**  
   - Fetches a list of supported **OpenShift versions** for Hypershift.
   - Selects the **target OpenShift version** for deployment.
   - Identifies the **supported cluster architecture** (`arm64`, `amd64`).
   - Retrieves the latest OpenShift **version tag**.
   - Provisions a **Hypershift AWS cluster** using the selected parameters.

5. **Deploy Operator (`deploy-operator`)**  
   - Retrieves **Kubeconfig credentials** for cluster access.
   - Installs the **operator** on the newly provisioned cluster.
   - Gathers **cluster artifacts** for analysis and validation.
   - Verifies the cluster artifacts directory is downloaded.
   - Pushes gathered artifacts to an **OCI artifact repository** (e.g. Quay).
   - Fails the pipeline if operator installation fails.


## Parameters
|name|description|default value|required|
|---|---|---|---|
|SNAPSHOT| Snapshot of the application|| true|
|PACKAGE_NAME| An OLM package name present in the fragment or leave it empty so the step will determine the default package. If there is only one 'olm.package', it's name is returned. If multiple 'olm.package' entries contain unreleased bundles, user input is required; the PACKAGE_NAME parameter must be set by the user| ""| false|
|CHANNEL_NAME| An OLM channel name or leave it empty so the step will determine the default channel name. The default channel name corresponds to the 'defaultChannel' entry of the selected package| ""| false|
|CREDENTIALS_SECRET_NAME| Name of the secret containing the oci-storage-dockerconfigjson key with registry credentials in .dockerconfigjson format, used for pushing artifacts to an OCI registry|| true|
|OCI_REF| Full OCI artifact reference in a format "quay.io/org/repo:tag"|| true|
|REPO_TOKEN| Name of the Kubernetes Secret that contains the access token for a private GitHub or GitLab repository|| false|
|REPO_KEY| Key within the Secret's "data" field that holds the GitHub/GitLab access token|| false|
