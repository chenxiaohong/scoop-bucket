# Scoop Bucket

```powershell
scoop bucket add my-bucket 'https://github.com/chenxiaohong/scoop-bucket.git'
```

## Scoop Bucket List

* https://github.com/ACooper81/scoop-apps

## Scoop Hash

```powershell
certutil -hashfile [file] sha256
certutil -hashfile [file] md5
certutil -hashfile [file] sha1
```

## Test Manifest

新建或修改 `bucket/<app>.json` 后，可以先在本地直接测试这个 manifest 文件。

```powershell
# JSON 语法检查
jq empty .\bucket\<app>.json

# Scoop schema 校验
$scoopHome = scoop prefix scoop
Add-Type -Path "$scoopHome\supporting\validator\bin\Scoop.Validator.dll"
$validator = New-Object Scoop.Validator("$scoopHome/schema.json", $true)
$validator.Validate((Resolve-Path .\bucket\<app>.json).Path)
$validator.ErrorsAsString

# 检查版本更新规则
& "$scoopHome\bin\checkver.ps1" <app> -Dir .\bucket

# 测试下载并校验 hash
scoop download .\bucket\<app>.json

# 直接安装本地 manifest
scoop install .\bucket\<app>.json

# 卸载测试安装
scoop uninstall <app>
```

也可以运行 bucket 测试脚本：

```powershell
Invoke-Pester .\Bucket.Tests.ps1
```

如果提示缺少 `BuildHelpers` 或 `Pester` 指定版本，需要先安装对应 PowerShell 模块。
