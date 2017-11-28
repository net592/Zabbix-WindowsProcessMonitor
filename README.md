# Zabbix-WindowsProcessMonitor
Zabbix监控windwos 进程 和 IIS进程 CPU 内存 连接数 等数据 服务插件
待上传数据 ，2017-11-24后更新

#工作原理
WindowsProcessMonitor服务---获取windows服务列表---然后查找对应同同名服务名.EXE进程---C盘生成对应的采集文件<---ZabbixAgent脚本-模板采集数据 

#环境要求 .Net 版本4.5以上，需要类库支持
#部署脚本就是 安装服务即可
Write-Host  '##############Restart ################'
#Copy-Item \\10.1.1.1\e$\opstools\zabbixtool\zabbix\ZabbixAppService\*  c:/opstools/ -recurse -Force
Write-Host  '##############Copy################'
#CMD /C "sc config wuauserv start= disabled"
new-service -Name Zabbix.WinSentinel.WindowsService -DisplayName Zabbix.WinSentinel.WindowsService -BinaryPathName “C:\opstools\Zabbix.WinSentinel.WindowsService\Zabbix.WinSentinel.WindowsService.exe” -StartupType Automatic
Start-Service "Zabbix.WinSentinel.WindowsService"
Get-Service "Zabbix.WinSentinel.WindowsService"
Write-Host  'Zabbix.WinSentinel.WindowsService...'
#
Write-Host  '##############Install abbix.WebSentinel.WindowsService################'
#CMD /C "sc config wuauserv start= disabled"
new-service -Name Zabbix.WebSentinel.WindowsService -DisplayName Zabbix.WebSentinel.WindowsService -BinaryPathName “C:\opstools\Zabbix.WebSentinel.WindowsService\Zabbix.WebSentinel.WindowsService.exe” -StartupType Automatic
Start-Service "Zabbix.WebSentinel.WindowsService"
Get-Service "Zabbix.WebSentinel.WindowsService"
Write-Host  'Zabbix.WebSentinel.WindowsService...'


#注意事项
1.默认匹配Beisen或beisen的开通的 自用服务
如果需要匹配其他服务请之前修改Sentinel.xml 增加你的名称


2.poweshell脚本采集，会带来%5左右CPU波动消耗，目前没有经历优化，先满足监控需求

