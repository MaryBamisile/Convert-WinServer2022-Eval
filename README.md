# Convert-WinServer2022-Eval

If your server is running an evaluation version of Windows Server Standard or Datacenter edition, you can convert it to an available retail version. Run the following commands in an elevated command prompt or PowerShell session.

- Determine the current edition name by running the following command. The output is an abbreviated form of the edition name. For example, Windows Server Datacenter (Desktop Experience) Evaluation edition is ServerDatacenterEval.

#### Windows Command Prompt

```
DISM /online /Get-CurrentEdition
```
- Verify which editions the current installation can be converted to by running the following command. From the output, make a note of the edition name you want to convert to.

```
DISM /online /Get-TargetEditions
```
- Run the following command to save the Microsoft Software License Terms for Windows Server, which you can then review. Replace the <target edition> placeholder with the edition name you noted from the previous step.

```
DISM /online /Set-Edition:<target edition> /GetEula:C:\license.rtf
```
- Enter the new edition name and corresponding retail product key in the following command. The set edition process requires you to accept the Microsoft Software License Terms for Windows Server that you saved previously.
```
DISM /online /Set-Edition:<target edition> /ProductKey:<product key> /AcceptEula
```

###### For Example
```
DISM /online /Set-Edition:ServerDatacenter /ProductKey:ABCDE-12345-ABCDE-12345-ABCDE /AcceptEula
```
