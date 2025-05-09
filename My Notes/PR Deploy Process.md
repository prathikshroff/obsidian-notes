Sardeie Nur

No need for a PR when merging into `master`. You simply merge the stage branches and deploy.

PR is only used for when we're merging from the dev branch to the stage branch. Once that's done, we  

1. first merge into `uat` and deploy to the UAT sandbox for testing
2. then, once we're approved for deployment to prod, we merge into `master` and deploy to production


and please merge `master` back into `uat` and the feature/stage branches as often as possible to keep everything aligned and catch conflicts early