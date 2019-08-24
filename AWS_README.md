Git Clone this repository

> go to https://s3.console.aws.amazon.com/s3/home?region=eu-west-1#

1. Create bucket
2. webapp-konstancin-`your_acronym` for example `webapp-konstancin-pzm`
3. Next
4. Untick `Block all public access`
5. Next && Create bucket
6. Navigate to your bucket
7. Properties && Static website hosting
8. `Use this bucket to host a website` && index.html
    > note `Endpoint` && test in the browser
9. Permissions && Bucket Policy
    ```$xslt
    {
      "Version":"2012-10-17",
      "Statement":[{
        "Sid":"PublicReadGetObject",
            "Effect":"Allow",
          "Principal": "*",
          "Action":["s3:GetObject"],
          "Resource":["arn:aws:s3:::webapp-konstancin-pzm/*"
          ]
        }
      ]
    }
    ```
10. Save

    > go to https://eu-west-1.console.aws.amazon.com/codesuite/codecommit/repositories?region=eu-west-1

11. Create Repository
12. Name Repository `webapp`
13. Create
    > note clone command

    > go to https://console.aws.amazon.com/iam/home?region=eu-west-1#/users/admin?section=security_credentials

14. Change remote of the repository to the copied remote from Code Commit.
    > git remote add origin https://git-codecommit.eu-west-1.amazonaws.com/v1/repos/webapp

    or

    > git remote set-url origin git remote add origin https://git-codecommit.eu-west-1.amazonaws.com/v1/repos/webapp

15. Generate HTTPS Git credentials for AWS CodeCommit
16. Note or download user name and password
    `admin-at-714436996402`
    `ODh4x0XymRuPEjaL7Y3ESHLy1koBaUZUEcaN1yyIezU=`

    > go to https://eu-west-1.console.aws.amazon.com/codesuite/codepipeline/pipeline/new

17. Name the pipeline `spa-ci`
18. Choose existing service role
19. Select AWS Code Commit as source provider
20. `webapp` as repository
21. `master` as a branch name
22. Build provider `AWS CodeBuild`
23. Create project `webapp`
24. `Amazon Linux 2`
25. Runtime `Standard`
26. `Always use the latest image for this runtime version`
27. `Continue to CodePipeline`
28. `Next`
    > go to https://eu-west-1.console.aws.amazon.com/codesuite/codebuild/projects/webapp/edit/environment?region=eu-west-1

29. Add `MICROSITE_S3_BUCKET` with the name of your S3 bucket
30. Note your Service Role `codebuild-webapp-service-role`

31. Review CodePipeline
32. Review code build details

33. Edit CodeBuild ServiceRole `codebuild-webapp-service-role`
34. Edit attached policy
35. JSON view
36. Review && Save
37. Run manually the pipeline
38. Navigate in the browser to s3 endpoint