### 08 Basic Single Page App hosting on AWS with Code Pipeline support.

The goal of this exercise is to learn how to host static webapp on S3 as well as put CD solution in place. 

1. Clone this repository to your local file system.

   `Make sure, you are always in eu-west-2 (Ireland) region`
2. Go to S3 (https://s3.console.aws.amazon.com/s3/home?region=eu-west-1#)
3. Create a bucket 
4. webapp-konstancin-`your_acronym` for example `webapp-konstancin-pzm`
5. Next
6. Untick `Block all public access`
7. Next && Create bucket
8. Navigate to your bucket
9. Go to Properties && Static website hosting
10. `Use this bucket to host a website` && index.html
    > note `Endpoint`
11. Go to Permissions && Bucket Policy
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
12. Click `Save`
12. Test noted endpoint.
13. Go to CodeCommit (https://eu-west-1.console.aws.amazon.com/codesuite/codecommit/repositories?region=eu-west-1)
14. Create Repository
15. Name Repository `webapp`
16. Click `Create`
    > note clone command
17. Go to IAM (https://console.aws.amazon.com/iam/home?region=eu-west-1)
19. Go to Users
20. Open Security Credentials.
21. Scroll down to `HTTPS Git credentials for AWS CodeCommit`.
22. Press generate and store the file.

    `training-user-at-XXXXXXX`
    `ODh4x0XymRuPEjaL7Y3ESHLy1koBaUZUEcaN1yyIezU=`
    
18. Change remote of the repository to the copied remote from Code Commit.
    > git remote set-url origin https://git-codecommit.eu-west-1.amazonaws.com/v1/repos/webapp

23. Go to CodePipeline (https://eu-west-1.console.aws.amazon.com/codesuite/codepipeline/pipeline/new)
24. Name the pipeline `spa-ci`.
25. Choose existing service role.
26. Select AWS Code Commit as source provider.
27. `webapp` as repository.
28. `master` as a branch name.
29. Build provider `AWS CodeBuild`.
30. Create project `webapp`.
31. `Amazon Linux 2`.
32. Runtime `Standard`.
33. `Always use the latest image for this runtime version`.
34. `Continue to CodePipeline`.
35. `Next`.
36. Go to CodeBuild and modify project. (https://eu-west-1.console.aws.amazon.com/codesuite/codebuild/projects/webapp/edit/environment?region=eu-west-1)
37. Add `MICROSITE_S3_BUCKET` Environment Variable with the name of your S3 bucket i.e. webapp-konstancin-`your_acronym`.
38. Note your Service Role `codebuild-webapp-service-role`
39. Review CodePipeline
40. Review CodeBuild details
41. Edit CodeBuild ServiceRole `codebuild-webapp-service-role`
42. Edit attached policy
    ```$xslt
    ,
            {
                "Effect": "Allow",
                "Resource": [
                    "arn:aws:s3:::webapp-konstancin-pzm*"
                ],
                "Action": [
                    "*"
                ]
            }
    ```
43. JSON view.
44. Click `Review` && `Save`.
45. Run manually the pipeline.
46. Navigate in the browser to s3 endpoint.