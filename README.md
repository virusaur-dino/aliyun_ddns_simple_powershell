# aliyun_ddns_simple_powershell
阿里云DNS动态更新Powershell脚本

因为脚本本身及其简单，但是需要自行定制参数，所以直接在Readme中进行说明

#前置条件
下载并配置好aliyun cli 说明文档：https://help.aliyun.com/zh/cli/

#获取本机v6地址
$MyAddr=(Get-NetIPAddress -AddressFamily IPv6 -InterfaceIndex [替换为你的网卡序号] -PrefixOrigin Dhcp | Where-Object -FilterScript { $_.ValidLifetime -Eq ([TimeSpan]::MaxValue) } | Select -ExpandProperty IPAddress)

#查询记录
$NsAddr=(Resolve-DnsName -Name [主机名].[域名] -Server 223.5.5.5 | select -ExpandProperty IPAddress)

#判断，如果本地地址发生变化则提交更新
if ($MyAddr -ne $NsAddr) { aliyun.exe alidns UpdateDomainRecord --RecordID [记录ID]  --RR winbox --Line default --Type AAAA --Value $MyAddr }

#将三行PS语句保存为后缀为.ps1通过Windows计划任务进行周期性调用

