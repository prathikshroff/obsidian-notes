Sardeie Nur

No need for a PR when merging into `master`. You simply merge the stage branches and deploy.

PR is only used for when we're merging from the dev branch to the stage branch. Once that's done, we  

1. first merge into `uat` and deploy to the UAT sandbox for testing
2. then, once we're approved for deployment to prod, we merge into `master` and deploy to production


and please merge `master` back into `uat` and the feature/stage branches as often as possible to keep everything aligned and catch conflicts early

Deploy Steps:

After merging stage branch into UAT branch:

1. Change to master branch and run this command to list the files that have been changed:
```
   git diff --name-only master...DIG-52149/stage
```

2. Merge stage branch to master
```
   git merge DIG-52149/stage
```

3. Update package.xml with the changed files
```
   sf sgd source delta -f origin/master -t master -o .
```

4. Convert the changed files to metadata format and store in a directory
```
sf project convert source -x package/package.xml -d deploy/2025-05-27_DIG-52149_prod
```

5. Validate deployment for Quick Deploy
```
sf project deploy validate -o "NPR PROD" -d deploy/2025-05-27_DIG-52149_prod -l RunSpecifiedTests --tests SS_TicketAttachmentControllerTest SS_TicketAttachmentsInputControllerTest SS_TicketDetailControllerTest
```

6. Add tags to the Master branch
```
git tag -a rel-2025-05-27-DIG-52149 -m "Studio Cases returning Ticket was not found"
```

7. Push the commits and tags on the master to remote
```
git push origin master --follow-tags
```

8. Delete dev and stage branches (local and remote)
```
git branch -d branch_name && git push origin --delete branch_name
```
