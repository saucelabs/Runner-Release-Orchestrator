# Runner-Release-Orchestrator
Central control panel for releasing the Cypress, Playwright, TestCafe, and Puppeteer runners — fan out the release, wait, and see every new version in one table.

# Variables to run the pipelines are

window-side-disk : RUN_VERSION_BUMP == "true"

mac-side-disk : RUN_VERSION_BUMP == "true"

image-sync : RUN_IMAGE_SYNC_MR == "true"

image-metadata-service : UPDATE_IMAGES == "true"

devx-e2e : RUN_FRAMEWORK_UPDATE == "true"

test-composer : GENERATE_FRAMEWORK_MR == "true"
