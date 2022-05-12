# AWS Tools for PowerShell

Here are some examples:

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
