# AWS CLI Commands for CloudFormation

## Create CodePipeline Stack(s)

### S3 Artifact Bucket
```
aws cloudformation create-stack --stack-name ccDemoAppBucket --template-body file://2_S3ArtifactBucket.yml --parameters file://params/2_S3ArtifactBucket-params.json 
```

### CodeCommit
```
aws cloudformation create-stack --stack-name ccDemoAppRepo --template-body file://1_CodeCommit.yml --parameters file://params/1_CodeCommit-params.json
```

### CodeBuild
```
aws cloudformation create-stack --stack-name ccDemoCodeBuild --template-body file://3_CodeBuild.yml --parameters file://params/3_CodeBuild-params.json --capabilities CAPABILITY_IAM --capabilities CAPABILITY_NAMED_IAM
```

### CodeDeploy
```
aws cloudformation create-stack --stack-name ccDemoCodeDeploy --template-body file://4_CodeDeploy.yml --parameters file://params/4_CodeDeploy-params.json --capabilities CAPABILITY_IAM --capabilities CAPABILITY_NAMED_IAM
```

### CodePipeline
```
aws cloudformation create-stack --stack-name ccDemoCodePipeline --template-body file://5_CodePipeline.yml --capabilities CAPABILITY_IAM --capabilities CAPABILITY_NAMED_IAM
```
