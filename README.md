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
