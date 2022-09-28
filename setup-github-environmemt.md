## Environment Protection Rules:
Environment protection rules require specific conditions to pass before a job referencing the environment can proceed. You can use environment protection rules to require a manual approval, delay a job, or restrict the environment to certain branches.
## Required reviewers:
Use required reviewers to require a specific person or team to approve workflow jobs that reference the environment. You can list up to six users or teams as reviewers. The reviewers must have at least read access to the repository. Only one of the required reviewers needs to approve the job for it to proceed.
For more information on reviewing jobs that reference an environment with required reviewers, see "Reviewing deployments."
## Wait timer:
Use a wait timer to delay a job for a specific amount of time after the job is initially triggered. The time (in minutes) must be an integer between 0 and 43,200 (30 days).
## Deployment branches:
Use deployment branches to restrict which branches can deploy to the environment. Below are the options for deployment branches for an environment:
- All branches: All branches in the repository can deploy to the environment.
- Protected branches: Only branches with branch protection rules enabled can deploy to the environment. If no branch protection rules are defined for any branch in the repository, then all branches can deploy. For more information about branch protection rules, see "About protected branches."
- Selected branches: Only branches that match your specified name patterns can deploy to the environment.
For example, if you specify releases/* as a deployment branch rule, only branches whose name begins with releases/ can deploy to the environment. (Wildcard characters will not match /. To match branches that begin with release/ and contain an additional single slash, use release/*/*.) If you add main as a deployment branch rule, a branch named main can also deploy to the environment. For more information about syntax options for deployment branches, see the Ruby File.fnmatch documentation.
## Environment secrets:
Secrets stored in an environment are only available to workflow jobs that reference the environment. If the environment requires approval, a job cannot access environment secrets until one of the required reviewers approves it. For more information about secrets, see "Encrypted secrets."
## Creating an environment
To configure an environment in a personal account repository, you must be the repository owner. To configure an environment in an organization repository, you must have admin access.
- On GitHub.com, navigate to the main page of the repository.
- Under your repository name, click           Settings.
![](https://github.com/prashant-demo110/helm_charts/tree/main/images/image18.jpg)
- In the left sidebar, click Environments.
- Click New environment.
- Enter a name for the environment, then click Configure environment. Environment names are not case sensitive. An environment name may not exceed 255 characters and must be unique within the repository.
- Optionally, specify people or teams that must approve workflow jobs that use this environment.
    - Select Required reviewers.
    - Enter up to 6 people or teams. Only one of the required reviewers needs to approve the job for it to proceed.
    - Click Save protection rules.
- Optionally, specify the amount of time to wait before allowing workflow jobs that use this environment to proceed.
    - Select Wait timer.
    - Enter the number of minutes to wait.
    - Click Save protection rules.
- Optionally, specify what branches can deploy to this environment. Select the desired option in the Deployment branches dropdown.
If you chose Selected branches, enter the branch name patterns that you want to allow.
Optionally, add environment secrets. These secrets are only available to workflow jobs that use the environment. Additionally, workflow jobs that use this environment can only access these secrets after any configured rules (for example, required reviewers) pass. For more information about secrets, see "Encrypted secrets."
    - Under Environment secrets, click Add Secret.
    - Enter the secret name.
    - Enter the secret value.
    - Click Add secret.
Running a workflow that references an environment that does not exist will create an environment with the referenced name. The newly created environment will not have any protection rules or secrets configured. Anyone that can edit workflows in the repository can create environments via a workflow file, but only repository admins can configure the environment.
## Using an environment:
Each job in a workflow can reference a single environment. Any protection rules configured for the environment must pass before a job referencing the environment is sent to a runner. The job can access the environment's secrets only after the job is sent to a runner.
When a workflow references an environment, the environment will appear in the repository's deployments. You can specify an environment for each job in your workflow. To do so, add a jobs.<job_id>.environment key followed by the name of the environment.
For example, this workflow will use an environment called production.
![](https://github.com/prashant-demo110/helm_charts/tree/main/images/image17.png)
When the above workflow runs, the deployment job will be subject to any rules configured for the production environment. For example, if the environment requires reviewers, the job will pause until one of the reviewers approves the job.
You can also specify a URL for the environment. The specified URL will appear on the deployments page for the repository (accessed by clicking Environments on the home page of your repository) and in the visualization graph for the workflow run. If a pull request triggered the workflow, the URL is also displayed as a View deployment button in the pull request timeline.
![](https://github.com/prashant-demo110/helm_charts/tree/main/images/image7.png)
![](https://github.com/prashant-demo110/helm_charts/tree/main/images/image15.png)
## Deletion of an environment:
To configure an environment in a personal account repository, you must be the repository owner. To configure an environment in an organization repository, you must have admin access.
Deleting an environment will delete all secrets and protection rules associated with the environment. Any jobs currently waiting because of protection rules from the deleted environment will automatically fail.
   - On GitHub.com, navigate to the main page of the repository.
   - Under your repository name, click           Settings.
![](https://github.com/prashant-demo110/helm_charts/tree/main/images/image18.jpg)
   - In the left sidebar, click Environments.
   - Next to the environment that you want to delete, click        .
   - Click I understand, delete this environment.
## Proof Of Concept:
1. Development (no approver, no waitime)
Selected Branch: develop
   - To create the Development Environment.
   - On GitHub.com, navigate to the main page of the repository.
   - Under your repository name, click        Settings.
   - In the left sidebar, click Environments.
![](https://github.com/prashant-demo110/helm_charts/tree/main/images/image9.png)
   - Click Configure environment to configure the environment.
![](https://github.com/prashant-demo110/helm_charts/tree/main/images/image2.png)
   - In Development environment No reviewer is added and No wait timer is given.
   - This rule is enabled for develop branch.
   - Click Save protection rules to create the Development environment.
2. QA (no approver, no waitime)
Selected Branch: releases/**
To create the QA Environment.
   - On GitHub.com, navigate to the main page of the repository.
   - Under your repository name, click        Settings.
   - In the left sidebar, click Environments.
![](https://github.com/prashant-demo110/helm_charts/tree/main/images/image20.png)
   - Click Configure environment to configure the environment.
![](https://github.com/prashant-demo110/helm_charts/tree/main/images/image13.png)
   - In QA environment No reviewer is added and No wait timer is given.
   - This rule is enabled for all releases/** branch.
   - Click Save protection rules to create the QA environment.
3. Stage (atleast 1 approver)
Selected Branch: main
   - To create the Development Environment.
   - On GitHub.com, navigate to the main page of the repository.
   - Under your repository name, click        Settings.
   - In the left sidebar, click Environments.
![](https://github.com/prashant-demo110/helm_charts/tree/main/images/image16.png)
   - Click Configure environment to configure the environment.
![](https://github.com/prashant-demo110/helm_charts/tree/main/images/image8.png)
![](https://github.com/prashant-demo110/helm_charts/tree/main/images/image1.png)
   - In Stage environment 1 reviewer is added.
   - This rule is enabled for main branch.
   - Click Save protection rules to create the Stage environment.
4. Prod (atleast 1 approver, 5 minutes waitime)
Selected Branch: main
To create the Development Environment.
   - On GitHub.com, navigate to the main page of the repository.
   - Under your repository name, click        Settings.
   - In the left sidebar, click Environments.
![](https://github.com/prashant-demo110/helm_charts/tree/main/images/image10.png)
   - Click Configure environment to configure the environment.
![](https://github.com/prashant-demo110/helm_charts/tree/main/images/image6.png)
![](https://github.com/prashant-demo110/helm_charts/tree/main/images/image1.png)
   - In Prod environment 1 reviewer and 5 mins wait timer is added.
   - This rule is enabled for main branch.
   - Click Save protection rules to create the Prod environment.
5. For each workflow templates (dev, qa ,stage ,prod) update it to use environment keyword.
  (i). Workflow template file for dev environment.
![](https://github.com/prashant-demo110/helm_charts/tree/main/images/image11.png)
  (ii). Workflow template file for qa environment.
![](https://github.com/prashant-demo110/helm_charts/tree/main/images/image5.png)
  (iii). Workflow template file for stage environment.
![](https://github.com/prashant-demo110/helm_charts/tree/main/images/image4.png)
  (iv). Workflow template file for prod environment.
![](https://github.com/prashant-demo110/helm_charts/tree/main/images/image12.png)
