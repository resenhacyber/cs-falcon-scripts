# Falcon Powershell Installation Scripts

Scripts Powershell para instalar/desinstalar o Falcon Sensor por meio das APIs do Falcon em um endpoint do Windows.

Permissões da API Falcon
Os clientes de API recebem um ou mais escopos de API. Os escopos permitem o acesso a APIs CrowdStrike específicas e descrevem as ações que um cliente de API pode executar.

Verifique se os seguintes escopos de API estão ativados:

Instalar:
Baixar Sensor [ler]
Políticas de atualização de sensores [ler]
Desinstalar:
Anfitrião [escrever]
Políticas de atualização de sensores [escrever]
Configuração
Instalar
Usa as APIs CrowdStrike Falcon para verificar a versão do sensor atribuída a uma política Windows Sensor Update, baixa essa versão e a instala na máquina local. Por padrão, depois de concluído, o script exclui a si mesmo e o pacote do instalador baixado. As etapas individuais e quaisquer mensagens de erro relacionadas são registradas em 'Windows\Temp\csfalcon_install.log', a menos que especificado de outra forma.

O script deve ser executado como administrador na máquina local para que a instalação do Falcon Sensor seja concluída.

As opções de script podem ser passadas como parâmetros ou definidas no bloco param(). Os valores padrão estão listados nas descrições dos parâmetros:

```pwsh
.PARAMETER FalconCloud
CrowdStrike Falcon OAuth2 API Hostname ['https://api.crowdstrike.com' if left undefined]
.PARAMETER FalconClientId
CrowdStrike Falcon OAuth2 API Client Id [Required]
.PARAMETER FalconClientSecret
CrowdStrike Falcon OAuth2 API Client Secret [Required]
.PARAMETER MemberCid
Member CID, used only in multi-CID ("Falcon Flight Control") configurations and with a parent management CID.
.PARAMETER SensorUpdatePolicyName
Sensor Update Policy name to check for assigned sensor version ['platform_default' if left undefined]
.PARAMETER InstallParams
Sensor installation parameters, without your CID value ['/install /quiet /noreboot' if left undefined]
.PARAMETER LogPath
Script log location ['Windows\Temp\csfalcon_install.log' if left undefined]
.PARAMETER DeleteInstaller
Delete sensor installer package when complete [default: $true]
.PARAMETER DeleteScript
Delete script when complete [default: $false]
.PARAMETER Uninstall
Uninstall the sensor from the host [default: $false]
.PARAMETER ProvToken
Provisioning token to use for sensor installation [default: $null]
.PARAMETER ProvWaitTime
Time to wait, in seconds, for sensor to provision [default: 1200]
.PARAMETER Tags
A comma-separated list of tags to apply to the host after sensor installation [default: $null]
```

Exemplo:
```pwsh
PS>.\falcon_windows_install.ps1 -FalconClientId <string> -FalconClientSecret <string>
```

### Desinstalar

Desinstala o Sensor CrowdStrike Falcon para Windows. Por padrão, depois de concluído, o script
exclui a si mesmo e o pacote de desinstalação baixado (se necessário). As etapas individuais e quaisquer mensagens de erro relacionadas são registradas em `'Windows\Temp\csfalcon_uninstall.log'`, a menos que especificado de outra forma.

O script deve ser executado como administrador na máquina local para a instalação do Falcon Sensor
completar.

As opções de script podem ser passadas como parâmetros ou definidas no bloco param(). Os valores padrão são listados em
as descrições dos parâmetros:

```pwsh
.PARAMETER MaintenanceToken
Sensor uninstall maintenance token. If left undefined, the script will attempt to retrieve the
token from the API assuming the FalconClientId|FalconClientSecret are defined.
.PARAMETER UninstallParams
Sensor uninstall parameters ['/uninstall /quiet' if left undefined]
.PARAMETER UninstallTool
Sensor uninstall tool, local installation cache or CS standalone uninstaller ['installcache' if left undefined]
.PARAMETER LogPath
Script log location ['Windows\Temp\csfalcon_uninstall.log' if left undefined]
.PARAMETER DeleteUninstaller
Delete sensor uninstaller package when complete [default: $true]
.PARAMETER DeleteScript
Delete script when complete [default: $true]
.PARAMETER RemoveHost
Remove host from CrowdStrike Falcon [default: $false]
.PARAMETER FalconCloud
CrowdStrike Falcon OAuth2 API Hostname [default: autodiscover]
.PARAMETER FalconClientId
CrowdStrike Falcon OAuth2 API Client Id [Required if RemoveHost is $true]
.PARAMETER FalconClientSecret
CrowdStrike Falcon OAuth2 API Client Secret [Required if RemoveHost is $true]
.PARAMETER MemberCid
Member CID, used only in multi-CID ("Falcon Flight Control") configurations and with a parent management CID.
```

Exemplos:
Exemplo básico que desinstalará o sensor com o token de manutenção fornecido
```pwsh
PS>.\falcon_windows_uninstall.ps1 -MaintenanceToken <string>
```

Um exemplo usando a API do Falcon para recuperar o token de manutenção e remover o host do console do Falcon
após a desinstalação.
```pwsh
PS>.\falcon_windows_uninstall.ps1 -FalconClientId <string> -FalconClientSecret <string> -RemoveHost $true
```
