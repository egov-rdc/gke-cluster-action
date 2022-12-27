# gke-cluster-action

Automating Kubernetes Cluster creation and Bootstrapping using Github Actions.
Github Actions allows you to design your CI/CD workflows directly in your Github repositories.

This repository contains terraform code. It would create a GKE cluster and then add a node pool to the created cluster. 
through our terraform. It would:

 - Ensures that our terraform code doesn't have any errors.
 - Ensures that our terraform code present in the master branch doesn't have any formatting issues.
 - Ensures that any pull request raised to the master branch are properly tested before merging.
 - And finally, automates the provisioning of the GKE cluster and the node pool.

Github Actions are entirely integrated with Github. Build, Test, and Deploy can be done directly from Github. 
So the CI/CD workflows is at the same place where we source code. 

The CI/CD pipeline can be triggered with events like pull requests or merging of branches... 

So our GitHub action does the following steps:

1. Whenever a pull request is raised
   * Checks out the code from the master branch into the ubuntu-latest machine. ( name: Checkout )
   * Checks if our terraform code is formatted properly. ( name: Terraform Format )
   * Initializes the terraform working directory. ( name: Terraform Init )
   * Generates a speculative execution plan, showing what actions Terraform would take to apply the current configuration. ( name: Terraform Plan )

2. Whenever the pull request is merged
   * Creates or updates infrastructure according to Terraform configuration files in the current directory ( name: Terraform Apply )
   * Bootstrapping the cluster by Installing flagger and Istio operator ( name: bootstrap-cluster ). This job waits for the terraform job to be completed before it starts.