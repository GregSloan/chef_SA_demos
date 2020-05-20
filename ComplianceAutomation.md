# Compliance Automation with Waivers Demo

These instructions describes how to deploy an environment and run a demo that shows a pipelined DCA workflow and includes centrally managing InSpec waivers.

The necessary repos are in a GitLab group, "copmliance_automation". GitLab was chosen because of the simplicity of creating a hab pipeline with GitLabCI.

## Components

1. Deployment - <https://gitlab.com/compliance_automation/deploy>
2. Effortless Config repo in GitLab that includes waivers.rb recipe - <https://gitlab.com/compliance_automation/effortless-compliance-config>
3. An web-reachable location storing waiver files. Here is an example using a GitLab project - <https://gilab.com/compliance_automation/waivers>
4. An Effortless Audit repo with GitLabCI pipeline defined - <https://gitlab.com/compliance_automation/effortless-compliance-audit>

## Demo Story

This demo is meant to demonstrate making changes to compliance profile definitins in an Effortless Audit scenario in 2 ways. 

1. A change to the actual profile, which is applied via pipeline
2. A change to waiver lists which is applied via Effortless Config

The target audience is a security engineer that may not be comfortable with the entire Chef/pipeline stack and ecosystem, or is afraid of cli/coding. The intent is to show that with Effortless Audit and Config, a security engineer can make simple changes to the definition of compliance in their estate with very little training.

## Setup

### GitLab

The delivery method in the demo is a GitLab CI pipeline. The easiest way to operate is to create a Group in your GitLab namespace and clone the above repos into that group. Here's how!

1. Log in to <https://gitlab.com>
2. Drop down `Groups` and click on "Your Groups"
3. Click the green "New Group" button
4. Set the group name and url. Set group as Public. Click green "Create group" button
5. On the new group page, hover over "Settings" on the left and select "CI/CD"
6. Click "Expand" next to "Runners"
7. Scroll down to "Set up a group Runner manually"
8. Take note of the token value in the section. It will be used during deployment of the environment
9. Return to the CI/CD settings page of your new group
10. Expand "Variables"
11. Click green "Add Variable" button
12. Key = "auth_token", value = [your Hab origin auth token]
13. Check the "Mask Variable" box
14. Click "Add variable"

Next step is to get the necessary repos forked into your new group

1. Browse to <https://gitlab.com/compliance_automation/effortless-compliance-audit>
2. Click fork
3. Select your new group as the destination namespace for the fork

-OR-

To use your own effortless audit package, create your own gitlab project with your effortless audit source and copy .gitlab-ci.yml from the above repo

Repeat steps 1-3 for <https://gitlab.com/compliance_automation/waivers>

These are the only 2 repos that are required to be in your GitLab group for this demo. However you may want to also fork the effortless-compliance-config project as well in case you want to demonstrate making Effortless Config changes through a pipeline as well.

### Waiver Files

Before deploying the demo environment you will need to have a location to store the files that define your InSpec waivers. This can be anywhere that has an addressable URL and can support a diectory structure under that URL. The directory structure looks like this (where [waiver_source_location] is a base URL):

```[waiver_source_locaiton]/master.toml
[waiver_source_location]/Environments/
--prod.toml
--dev.toml
[waiver_source_location]/Nodes/
--node1_hostname.toml
--node2_hostname.toml
```

For example, this (<https://gitlab.com/compliance_automation/waivers>) is a GitLab repo that has the correct structure. To use it in the demo, the value of [waiver_source_location] would be <https://gitlab.com/compliance_automation/waivers/-/raw/master/>

### Build Effortless Package(s)

1. Clone the Effortless Audit repo from your compliance automation group
2. Set your origin and package name
3. Build your package and upload to Builder
4. Promote the package to both 'dev' and 'prod' channels

NOTE: Since you likely do not have a GitLab runner configured for your group yet, any changes to the repo on GitLab will result in a stalled/failed CI/CD run. This is expected.

Repeat for Effortless Config package if building your own

### Deploy

Follow deployment instructions at <https://gitlab.com/compliance_automation/deploy>. You will need the token from step 8 of the GitLab setup, the origins and package names of the Effortless packages you'll be using, and the waiver_source_location from Waiver Files above.

## Running Compliance Automation Demo

### Add new compliance control

1. Log in to Automate dashboard and show current compliance state.
2. Describe a new control that security might want to impose on the environment
3. Add the control to the Effortless Audit repo, commit and push the changes
4. Browse to CI/CD Pipeline for the Effortless Audit project to show the artifact being built
5. Use pipeline to promote to 'dev' channel
6. Show in Automate dashboard that the 'dev' nodes have picked up the new control
7. Promote to 'prod' channel via pipeline

### Create waiver

1. Explain that security has approved a waiver for the new control either for one of the environments, or for a particular server
2. Update the relevant toml file in the waiver file repository
3. On next scheduled Chef Client run, the change will be picked up and Audit will run automatically
4. Show in Automate dashboard that the control was waived.
