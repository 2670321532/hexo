---
title: VMware Workstation虚机自动开关机
date: 2023/7/8 10:46:25
comments: true
tags: 
- windows
- VMware
- bat
categories: 
- 计算机
- VMware
---

# VMware Workstation虚机自动开关机

## 虚拟机开机

```bat
@echo 以管理员身份运行
@echo off
>nul 2>&1 "%SYSTEMROOT%\system32\cacls.exe" "%SYSTEMROOT%\system32\config\system"
if \'%errorlevel%\' NEQ \'0\' (
goto UACPrompt
) else ( goto gotAdmin )
:UACPrompt
echo Set UAC = CreateObject^("Shell.Application"^) > "%temp%\getadmin.vbs"
echo UAC.ShellExecute "%~s0", "", "", "runas", 1 >> "%temp%\getadmin.vbs"
"%temp%\getadmin.vbs"
exit /B
:gotAdmin
if exist "%temp%\getadmin.vbs" ( del "%temp%\getadmin.vbs" ) 
cd /d "%~dp0"

@echo 中文无乱码
chcp 65001

@echo 通过vmrun.exe启动配置的虚拟机
"K:\VMware\VMware Workstation\vmrun.exe" -T ws start "D:\DMS7.1\DMS7.1.vmx"
```

\#“Win+R”，打开“启动任务”文件夹，把.bat放入其中 

shell:Common Startup

## 虚拟机关机

```bat
@echo 以管理员身份运行
@echo off
>nul 2>&1 "%SYSTEMROOT%\system32\cacls.exe" "%SYSTEMROOT%\system32\config\system"
if \'%errorlevel%\' NEQ \'0\' (
goto UACPrompt
) else ( goto gotAdmin )
:UACPrompt
echo Set UAC = CreateObject^("Shell.Application"^) > "%temp%\getadmin.vbs"
echo UAC.ShellExecute "%~s0", "", "", "runas", 1 >> "%temp%\getadmin.vbs"
"%temp%\getadmin.vbs"
exit /B
:gotAdmin
if exist "%temp%\getadmin.vbs" ( del "%temp%\getadmin.vbs" ) 
cd /d "%~dp0"

@echo 中文无乱码
chcp 65001


@echo 通过vmrun.exe关闭配置的虚拟机
"K:\VMware\VMware Workstation\vmrun.exe" -T ws suspend "D:\DMS7.1\DMS7.1.vmx"
```

gpedit.msc命令，打开“本地组策略编辑器”，展开“本地计算机 策略”——“计算机配置”——“Windows 设置”——“脚本（启动/关机）”。

