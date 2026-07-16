# Runner-Release-Orchestrator
Central control panel for releasing the Cypress, Playwright, TestCafe, and Puppeteer runners — fan out the release, wait, and see every new version in one table.

# Steps to do runner updates

1. Run 0th workflow: It gives you four PRs in each in every runner.
2. Then wait untill they are merged.
3. Run 1st workflow to release new versions of our runners.
4. After that, now we have to update these changes on mac-side-disk and window-side-disk. Run 2nd workflow to create MR for this and wait untill them get merged for next step.
5. Add the published runner version to the appropriate image. For this first run 3rd workflow to create a MR in image-sync project.
6. After merging this, run 4th workflow to add all the changes to image-metadata-service. The MR is successfully created and readfy to merge. This is the final step in the image release process. Enabling the image means that new VMs will boot with this image.
7. Now, run 5th workflow to create PR for updates in saucectl.
8. And, after merging the PR click on 6th workflow to release new version of saucectl.
9. The newly available runners need to be tested as part of devx-e2e. Update the devx-e2e tests to target the latest available version of frameworks.
Run 7th workflow for create MR for devx-e2e.
10. The new runners can't be requested until they are exposed via Test-Composer. So, run 8th workflow to create MR for this update.
11. Now, trigger 9.1 workflow to update sauce-docs. As, the newly available versions need to be referenced in the Sauce Labs docs.
12. In the end, update our example-repos by trigger workflow 9.2.

# Note
All these workflows create PRs/MRs. Please wait for merges to complete before kicking off any further workflows.

# Variables to run the pipelines are

window-side-disk : RUN_VERSION_BUMP == "true"

mac-side-disk : RUN_VERSION_BUMP == "true"

image-sync : RUN_IMAGE_SYNC_MR == "true"

image-metadata-service : UPDATE_IMAGES == "true"

devx-e2e : RUN_FRAMEWORK_UPDATE == "true"

test-composer : GENERATE_FRAMEWORK_MR == "true"

# Pipeline Trigger Tokens

For triggering the pipelines from an external GitHub orchestrator, the right credential in GitLab is a Pipeline trigger token — it's purpose-built for kicking off a pipeline from outside and doesn't grant broad API access like a regular access token.

Token names (one per repo):

MAC_SIDE_DISK_TRIGGER_TOKEN

WINDOWS_SIDE_DISK_TRIGGER_TOKEN

IMAGE_SYNC_TRIGGER_TOKEN

IMAGE_METADATA_SERVICE_TRIGGER_TOKEN

DEVX_E2E_TRIGGER_TOKEN 

TEST_COMPOSER_TRIGGER_TOKEN   

## How to create them (owner does this, per repo):

In GitLab go to the repo → Settings → CI/CD → Pipeline trigger tokens → Add new token. Enter the description/name from the list above, click Create pipeline trigger token, and copy the value. Repeat for all six repos.

Use this exact name to fill the the description.

## How we can save them:

Store them as GitHub repository secrets in the orchestrator repo: Settings → Secrets and variables → Actions → New repository secret, naming each one clearly (e.g. MAC_SIDE_DISK_TRIGGER_TOKEN, WINDOWS_SIDE_DISK_TRIGGER_TOKEN, etc.). The orchestrator service then uses each secret to trigger that repo's pipeline.
