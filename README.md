### Instructions to Install ARGOCD using copilot ###
# more info in https://github.com/argoproj-labs/argocd-autopilot #

# For linux´and WSL #
# get the latest version or change to a specific version
´´´VERSION=$(curl --silent "https://api.github.com/repos/argoproj-labs/argocd-autopilot/releases/latest" | grep '"tag_name"' | sed -E 's/.*"([^"]+)".*/\1/')´´´

# download and extract the binary
´´´curl -L --output - https://github.com/argoproj-labs/argocd-autopilot/releases/download/$VERSION/argocd-autopilot-linux-amd64.tar.gz | tar zx´´´

# move the binary to your $PATH
´´´mv ./argocd-autopilot-* /usr/local/bin/argocd-autopilot´´´

# check the installation
´´´'argocd-autopilot version´´´

# Using Docker #
´´´
docker run \
  -v ~/.kube:/home/autopilot/.kube \
  -v ~/.gitconfig:/home/autopilot/.gitconfig \
  -it quay.io/argoprojlabs/argocd-autopilot <cmd> <flags> 
´´´
Getting Started
# All of the commands need your git token with the --git-token flag,
# or the GIT_TOKEN env variable:

    export GIT_TOKEN=<YOUR_TOKEN>

# The commands will also need your repo clone URL with the --repo flag,
# or the GIT_REPO env variable:

    export GIT_REPO=<REPO_URL>

# 1. Run the bootstrap installation on your current kubernetes context.
# This will install argo-cd as well as the application-set controller.

    argocd-autopilot repo bootstrap

# Please note that this will automatically attempt to create a private repository,
# if the clone URL references a non-existing one. If the repository already exists,
# the command will just clone it.

# 2. Create your first project

    argocd-autopilot project create my-project

# 3. Install your first application on your project

    argocd-autopilot app create demoapp --app github.com/argoproj-labs/argocd-autopilot/examples/demo-app/ -p my-project
Now, if you go to your Argo-CD UI, you should see something similar to this:

![getting_started_apps_1.png](/home/jaherran/code/kubernetes/argo/getting_started_apps_1.png)

Head over to our Getting Started guide for further details.

How it works
The autopilot bootstrap command will deploy an Argo-CD manifest to a target k8s cluster, and will commit an Argo-CD Application manifest under a specific directory in your GitOps repository. This Application will manage the Argo-CD installation itself - so after running this command, you will have an Argo-CD deployment that manages itself through GitOps.

From that point on, the user can create Projects and Applications that belong to them. Autopilot will commit the required manifests to the repository. Once committed, Argo-CD will do its magic and apply the Applications to the cluster.

An application can be added to a project from a public git repo + path, or from a directory in the local filesystem.

Architecture
Argo-CD Autopilot Architecture

![achitecture.png](/home/jaherran/code/kubernetes/argo/architecture.png)

Autopilot communicates with the cluster directly only during the bootstrap phase, when it deploys Argo-CD. After that, most commands will only require access to the GitOps repository. When adding a Project or Application to a remote k8s cluster, autopilot will require access to the Argo-CD server.

You can read more about it in the official proposal doc.

Features
Opinionated way to build a multi-project multi-application system, using GitOps principles.
Create a new GitOps repository, or use an existing one.
Supports creating the entire directory structure under any path the user requires.
When adding applications from a public repo, allow committing as either a kustomization that references the public repo, or as a "flat" manifest file containing all the required resources.
Use a different cluster from the one Argo-CD is running on, as a default cluster for a Project, or a target cluster for a specific Application.# autopilotargocd



### To install you need to create a repository ###
echo "# autopilotargocd" >> README.md             
git init
git config --local user.name "jaherran"
git config --local user.email "jaherran@yahoo.com"
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:jaherran/autopilotargocd.git
git push -u origin main # autopilotargocd
