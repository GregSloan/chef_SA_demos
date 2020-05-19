# Compliance Automation with Waivers Demo

These instructions describes how to deploy an environment and run a demo that shows a pipelined DCA workflow and includes centrally managing InSpec waivers.

The necessary repos are in a GitLab group, "copmliance_automation". GitLab was chosen because of the simplicity of creating a hab pipeline with GitLabCI.

## Components

1. Deployment - https://gitlab.com/compliance_automation/deploy
2. Effortless Config repo in GitLab that includes waivers.rb recipe - https://gitlab.com/compliance_automation/effortless-compliance-config
3. An web-reachable location storing waiver files. Here is an example using a GitLab project - https://gilab.com/compliance_automation/waivers
4. An Effortless Audit repo with GitLabCI pipeline defined - https://gitlab.com/compliance_automation/effortless-compliance-audit

## Demo Story

This demo is meant to demonstrate making changes to compliance profile definitins in an Effortless Audit scenario in 2 ways. 

1. A change to the actual profile, which is applied via pipeline
2. A change to waiver lists which is applied via Effortless Config

The target audience is a security engineer that may not be comfortable with the entire Chef/pipeline stack and ecosystem, or is afraid of cli/coding. The intent is to show that with Effortless Audit and Config, a security engineer can make simple changes to the definition of compliance in their estate with very little training.

## Setup

### GitLab

The delivery method in the demo is a GitLab CI pipeline. The easiest way to operate is to create a Group in your GitLab namespace and clone the above repos into that group. Here's how!

1. Log in to https://gitlab.com
2. Drop down `Groups` and click on "Your Groups"
3. Click the green "New Group" button
4. Set the group name and url. Set group as Public. Click green "Create group" button
5. On the new group page, hover over "Settings" on the left and select "CI/CD"
6. Click "Expand" next to "Runners" 
7. Scroll down to "Set up a group Runner manually"
8. Take note of the token in the section. It will be used during deployment of the environment

Next step is to get the necessary repos forked into your new group

1. Browse to https://gitlab.com/compliance_automation/effortless-compliance-audit
2. Click fork
3. Select your new group as the destination namespace for the fork
4. Repeat the 1-3 for https://gitlab.com/compliance_automation/waivers

These are the only 2 repos that are required to be forked for this demo. However you may want to also fork the effortless-compliance-config project as well in case you want to demonstrate making Effortless Config changes through a pipeline as well.

### 

