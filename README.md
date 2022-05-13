# AWS Tools for PowerShell

## Installation and Configuration (on Windows)
Run the Windows PowerShell as Administrator.
Run the following command to install AWS Tools for PowerShell:

```powershell
Install-Module -Name AWSPowerShell
```

Answer "Y" to the interactive prompts.

Next, try to see if the application has installed correctly:

```powershell
Get-AWSPowerShellVersion
```

Incase you encounter errors stating that "... the module could not be loaded ..." or that the module "... cannot be loaded because running scripts is disabled on this system ..." you may need to change the Execution Policy.
Run the following command to change the Execution Policy:

```powershell
Set-ExecutionPolicy RemoteSigned
```

Answer "Y" to the interactive prompts.

Next, set the proper AWS Credentials. If you don't have an administrator set up for your AWS account, follow the instructions (here)[https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html].
Once the administartor user has been set up, generate access keys for that user and securely store that information.

To add a new profile to the AWS SDK store, run the command (replace the `AccessKey` and `SecretKey` codes with the administrator access keys):
```powershell
Set-AWSCredential `
                 -AccessKey AKIA0123456787EXAMPLE `
                 -SecretKey wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY `
                 -StoreAs Administrator
```

Next, save the configuration as the default profile and region for every PowerShell session:

```powershell
Initialize-AWSDefaultConfiguration -ProfileName Administrator -Region us-west-2
```

## AWS Tools for PowerShell Example Cmdlets

Here are some example cmdlets:

### Create Key-Pair

```powershell
$myPSKeyPair = New-EC2KeyPair -KeyName myPSKeyPair
```

### View Key-Pair Details

```powershell
$myPSKeyPair | Get-Member
```

### Save Key-Pair

```powershell
$myPSKeyPair.KeyMaterial | Out-File -Encoding ascii myPSKeyPair.pem
```

### Remove Key Pair from AWS

```powershell
Remove-EC2KeyPair -KeyName myPSKeyPair
```

---

### Create Security Group

```powershell
New-EC2SecurityGroup -GroupName myPSSecurityGroup -GroupDescription "EC2-Classic from PowerShell"
```

### View Security Groups

```powershell
Get-EC2SecurityGroup -GroupNames myPSSecurityGroup
```

### Remove Security Group

```powershell
Remove-EC2SecurityGroup -GroupId sg-0113e926ee099393f
```

---

### Launch an instance

```powershell
New-EC2Instance -ImageId ami-0ca285d4c2cda3300 `
    -MinCount 1 `
    -MaxCount 1 `
    -KeyName myPSKeyPair `
    -SecurityGroups myPSSecurityGroup `
    -InstanceType t2.micro
```

### View Instance

```powershell
$reservation = New-Object 'collections.generic.list[string]'
$reservation.add("r-b70a0ef1")
$filter_reservation = New-Object Amazon.EC2.Model.Filter -Property @{Name = "reservation-id"; Values = $reservation}
(Get-EC2Instance -Filter $filter_reservation).Instances
```

### Terminate Instance

```powershell
Remove-EC2Instance -InstanceId i-0453116cdface121d
```

---

### List Buckets

```powershell
Get-S3Bucket
```

### Create a bucket

```powershell
New-S3Bucket -BucketName uniquebucketname2258 -Region us-west-2
```

### Upload an object to the bucket

```powershell
Write-S3Object -BucketName uniquebucketname2258 -File .\sample.txt
```

### List Bucket Contents

```powershell
Get-S3Object -BucketName uniquebucketname2258
```

### Delete Bucket and its objects

```powershell
Remove-S3Bucket -BucketName uniquebucketname2258 -DeleteBucketContent
```

---

### List Users

```powershell
Get-IAMUserList
```

### Create a user

```powershell
New-IAMUser -UserName Bob
```

### Create a new group

```powershell
New-IAMGroup -GroupName DummyGroup
```

### Add users to group

```powershell
Add-IAMUserToGroup -UserName "Bob" -GroupName "DummyGroup"
```

### List the group, group details and its users

```powershell
$results = Get-IAMGroup -GroupName "DummyGroup"
$results
$results.Group
$results.Users
```

### Removes all users from a group and then deletes the group

```powershell
(Get-IAMGroup -GroupName DummyGroup).Users | Remove-IAMUserFromGroup -GroupName DummyGroup -Force
Remove-IAMGroup -GroupName DummyGroup -Force
```

### Deletes a user

```powershell
Remove-IAMUser -UserName Bob
```
