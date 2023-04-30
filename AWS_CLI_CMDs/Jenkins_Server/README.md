# AWS CLI CMDs for Provisioning Jenkins Server

## IAM Roles
```
aws iam create-role --role-name EC2JenkinsRole --assume-role-policy-document file://EC2TrustPolicy.json

aws iam attach-role-policy --role-name EC2JenkinsRole  --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess

aws iam attach-role-policy --role-name EC2JenkinsRole  --policy-arn arn:aws:iam::aws:policy/AWSCodePipeline_FullAccess
```

## Security Group, Ingress Rules
```
aws ec2 describe-vpcs
YOUR_VPC_ID

aws ec2 describe-subnets
subnet-0f7321702b9b05c5f

aws ec2 create-security-group \
   --group-name ccJenkinsSG \
   --description "ccJenkinsSG" \
   --tag-specifications 'ResourceType=security-group,Tags=[{Key=Name,Value=cc-jenkins-sg}]' \
   --vpc-id "YOUR_VPC_ID"
YOUR_SECURITY_GROUP_ID

aws ec2 authorize-security-group-ingress \
  --group-id  YOUR_SECURITY_GROUP_ID \
  --protocol tcp \
  --port 22 \
  --cidr 0.0.0.0/0 
  
aws ec2 authorize-security-group-ingress \
  --group-id  YOUR_SECURITY_GROUP_ID \
  --protocol tcp \
  --port 80 \
  --cidr 0.0.0.0/0 

aws ec2 authorize-security-group-ingress \
  --group-id  YOUR_SECURITY_GROUP_ID \
  --protocol tcp \
  --port 8080 \
  --cidr 0.0.0.0/0 
```

## EC2 Instances
```
aws iam create-instance-profile --instance-profile-name jenkins-server-profile
aws iam add-role-to-instance-profile --role-name EC2JenkinsRole --instance-profile-name jenkins-server-profile

aws ec2 create-key-pair --key-name ccJenkinsDemoKP --output text > ~/.ssh/cc-key

// Jenkins Server (Ubuntu)
aws ec2 run-instances \
--image-id ami-007855ac798b5175e \
--count 1 \
--instance-type t3.micro \
--key-name ccJenkinsDemoKP \
--iam-instance-profile Name=jenkins-server-profile \
--security-group-ids YOUR_SECURITY_GROUP_ID \
--subnet-id subnet-0f7321702b9b05c5f \
--block-device-mappings "[{\"DeviceName\":\"/dev/sdf\",\"Ebs\":{\"VolumeSize\":10,\"DeleteOnTermination\":false}}]" \
--tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=jenkins-server}]' \
--user-data file://jenkins_server_user_data.txt 
```

## Validation CMD(s)
```
java -version
sudo systemctl status jenkins
mvn -v
```

## Jenkins Config
- Jenkins Initial Admin Password
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

- JAVA_HOME: /usr/lib/jvm/java-11-openjdk-amd64
- MAVEN_HOME: /usr/share/maven
