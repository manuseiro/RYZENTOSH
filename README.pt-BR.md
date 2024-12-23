# BASE EFI AMD - Ryzen and Threadripper (1XXX, 2XXX, 3XXX, 4XXX, 5XXX)

Nota|Descrição
:----|:----
Suporte inicial ao macOS|macOS 10.13, High Sierra.

- Versão do Opencore: 1.0.2
- Data de lançamento: 17/11/2024

# Passos Básicos

1. [Baixe](https://github.com/manuseiro/BASE-EFI-AMD-RYZEN-THREADRIPPER/releases) a última versão disponível;
2. Inclua kexts adicionais (para ethernet, áudio, etc);
3. Inclua os patches ACPI necessários (.aml);
4. Revise as notas especiais;
5. Gere e complete suas informações do SMBIOS;
6. Ajuste sua BIOS;
7. Instale o macOS e aproveite :)

# Funcionalidades
- [x] Muito leve, muito limpo, arquivos básicos para seu Hackintosh.
- [x] Feito com as versões mais recentes do OpenCore.
- [x] Duas edições - *Release* e *Debug* Edition.
- [x] Atualizado mensalmente com versões revisadas do OpenCore.

# Kexts incluídos (**Obrigatórios**)

Kext|Descrição
:----|:----
[AMDRyzenCPUPowerManagement.kext](https://github.com/trulyspinach/SMCAMDProcessor/releases)|Para [AMD Power Gadget](https://github.com/trulyspinach/SMCAMDProcessor).
[SMCAMDProcessor.kext](https://github.com/trulyspinach/SMCAMDProcessor/releases)|Para [AMD Power Gadget](https://github.com/trulyspinach/SMCAMDProcessor).
[AppleMCEReporterDisabler.kext](https://github.com/acidanthera/bugtracker/files/3703498/AppleMCEReporterDisabler.kext.zip)|Útil a partir do Catalina para desativar o kext AppleMCEReporter que pode causar kernel panics em CPUs AMD e sistemas com dois processadores.
[Lilu.kext](https://github.com/acidanthera/Lilu/releases)|Corrige muitos processos, necessário para AppleALC, WhateverGreen, VirtualSMC e outros kexts.
[VirtualSMC.kext](https://github.com/acidanthera/VirtualSMC/releases)|Emula o chip SMC encontrado em Macs reais, sem isso o macOS não inicializa.<br>Alternativa: FakeSMC, que pode ter suporte melhor ou pior, geralmente usado em hardwares antigos.
[WhateverGreen.kext](https://github.com/acidanthera/WhateverGreen/releases)|Usado para correção gráfica, correções DRM, verificações de ID de placa, correções de framebuffer, etc; todas as GPUs se beneficiam deste kext.

# Outros Kexts (não inclusos)

Kexts para suporte de Áudio, Wifi, Ethernets e outros dispositivos.

### Audio

Kext|Descrição
:----|:----
255 / 5.000
[AppleALC.kext](https://github.com/acidanthera/AppleALC/releases)|Usado para aplicação de patches no AppleHDA, permitindo suporte para a maioria dos controladores de som integrados.<br>O AMD 15h/16h pode ter problemas com isso e os sistemas Ryzen/Threadripper raramente têm suporte para microfone.
[VoodooHDA.kext](https://sourceforge.net/projects/voodoohda/)|Áudio para sistemas FX e suporte a microfone+áudio no painel frontal para sistemas Ryzen, não misture com AppleALC.<br>A qualidade do áudio é visivelmente pior que AppleALC em CPUs Zen.

### Ethernet
Kext|Descrição
:----|:----
[IntelMausi.kext](https://github.com/acidanthera/IntelMausi/releases)|Intel's 82578, 82579, I217, I218 and I219 NICs are officially supported.
[AtherosE2200Ethernet.kext](https://github.com/Mieze/AtherosE2200Ethernet/releases)|Required for Atheros and Killer NICs.<br>**Note**: Atheros Killer E2500 models are actually Realtek based, for these systems please use RealtekRTL8111 instead.
[RealtekRTL8111.kext](https://github.com/Mieze/RTL8111_driver_for_OS_X/releases)|For Realtek's Gigabit Ethernet.<br>Sometimes the latest version of the kext might not work properly with your Ethernet. If you see this issue, try older versions.
[LucyRTL8125Ethernet.kext](https://www.insanelymac.com/forum/files/file/1004-lucyrtl8125ethernet/)|For Realtek's 2.5Gb Ethernet.
[SmallTreeIntel82576.kext](https://github.com/khronokernel/SmallTree-I211-AT-patch/releases)| Required for I211 NICs, based off of the SmallTree kext but patched to support I211.<br>Required for most AMD boards running Intel NICs.

### WiFi and Bluetooth
Kext|Descrição
:----|:----
[AirportItlwm](https://github.com/OpenIntelWireless/itlwm/releases)|Adds support for a large variety of Intel wireless cards and works natively in recovery thanks to IO80211Family integration.
[IntelBluetoothFirmware](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases)|Adds Bluetooth support to macOS when paired with an Intel wireless card.
[AirportBrcmFixup](https://github.com/acidanthera/AirportBrcmFixup/releases)|Used for patching non-Apple/non-Fenvi Broadcom cards, will not work on Intel, Killer, Realtek, etc.<br>For Big Sur see [Big Sur Known Issues](https://dortania.github.io/OpenCore-Install-Guide/extras/big-sur#known-issues) for extra steps regarding AirPortBrcm4360 drivers.
[BrcmPatchRAM](https://github.com/acidanthera/BrcmPatchRAM/releases)|Used for uploading firmware on Broadcom Bluetooth chipset, required for all non-Apple/non-Fenvi Airport cards.

### Others
Kext|Descrição
:----|:----
[NVMeFix](https://github.com/acidanthera/NVMeFix/releases)|Used for fixing power management and initialization on non-Apple NVMe.
[SATA-Unsupported](https://github.com/khronokernel/Legacy-Kexts/blob/master/Injectors/Zip/SATA-unsupported.kext.zip)|Adds support for a large variety of SATA controllers, mainly relevant for laptops which have issues seeing the SATA drive in macOS.<br>We recommend testing without this first.
[AppleMCEReporterDisabler](https://github.com/acidanthera/bugtracker/files/3703498/AppleMCEReporterDisabler.kext.zip)|Useful starting with Catalina to disable the AppleMCEReporter kext which will cause kernel panics on AMD CPUs.<br>Recommended for dual-socket systems (ie. Intel Xeons).

# Tabelas ACPI - AMD

Esses arquivos **DEVEM** ser incluídos no diretório ACPI do seu EFI. Recomendamos que você use o método **MANUAL**, mas para um primeiro teste você pode usar as versões prebuild.

Tabela|Descrição
:----|:----
SSDT-EC-USBX|[Manual](https://dortania.github.io/Getting-Started-With-ACPI/Universal/ec-methods/manual.html) \| [Prebuilt](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-EC-USBX-DESKTOP.aml) \| [Details](https://dortania.github.io/Getting-Started-With-ACPI/Universal/ec-fix.html)
SSDT-CPUR|[Prebuilt](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-CPUR.aml) <br> *Somente para B550 e A520*

### Despejando seu DSDT no ambiente Windows
[Download iASL Compiler ACPI Tools](https://acpica.org/downloads/binary-tools)
<br><br>
Open the CMD in the directory where the *ACPI Tools* was extracted. (*Command Prompt*) in **Administrator Mode**:
```
path/to/acpidump.exe -b -n DSDT -z
move dsdt.dat DSDT.aml
```

Decompile DSDT.aml:
```
path/to/iasl.exe path/to/DSDT.aml
```
*File DSDT.dsl will generated. Use this for generate YOUR ACPI Patches.* 

Compile DSDT.dsl:
```
path/to/iasl.exe path/to/DSDT.dsl
```
*File APCPI_FILE_PATCHED.aml will generated.*

# Atenção

Atualize **config.plist** em PlatformInfo > Generic com SUAS informações.
<br><br>
*1. MLB (Board Serial)
<br>
2. ROM (Mac Address)
<br>
3. SystemSerialNumber (Serial)
<br>
4. SystemUUID (SmUUID)*

Use [*genSMBIOS*](https://github.com/corpnewt/GenSMBIOS/archive/refs/heads/master.zip) para gerar valores para os itens acima.
<br>
Use [*ProperTree*](https://github.com/corpnewt/ProperTree/archive/refs/heads/master.zip) para configurar/editar seu config.plist.

# SMBIOS compatível

SMBIOS|Descrição
:----|:----
iMacPro1,1|AMD RX Polaris e mais recentes.
MacPro7,1|AMD RX Polaris e mais recentes.<br>Observe que o MacPro7,1 é exclusivo para macOS 10.15, Catalina e mais recentes.
MacPro6,1|AMD R5/R7/R9 and older.
iMac14,2|Nvidia Kepler e mais recentes.<br>Observação: o iMac14,2 é suportado apenas para macOS 10.8-10.15, para macOS 11, Big Sur e mais recentes, use o MacPro7,1.

# Catalina e versões mais antigas do macOS

- Por favor, configure `MinDate` e `MinVersion` em UEFI > APFS para `-1`;
- Por favor, configure `SecureBootModel` em Misc > Security para `j137`;

\* *Sem as configurações acima, o macOS não conseguirá inicializar.*

# Special notes

- O mapeamento da porta USB é **OBRIGATÓRIO**.
- **`XhciPortLimit`** - Necessário **`DESATIVAR`** se você usar o Big Sur 11.3+. 
	- Mapeie o USB no macOS Catalina antes de instalar o Big Sur ou mais recente para obter melhores resultados.
	- Você pode usar o utilitário USBMap.command - [USBMap](https://github.com/corpnewt/USBMap).
- **`SetupVirtualMap`** em config.plist:
	- As placas B550, A520 e TRx40 devem estar **`DESATIVADA`**;
	- As versões de BIOS mais recentes da X570 também exigem que seja **`DESATIVADA`**;
	- X470 e B450 com atualizações de BIOS do final de 2020 também exigem este seja **`DESATIVADA`**.
- **`DevirtualiseMmio`** - Usuários da TRx40, por favor, **ATIVEM`** este sinalizador.

## Special notes - Opencore 0.7.1+

Encontre os três patches `algrey - Force cpuid_cores_per_package` e altere apenas o valor `Replace` - `config.plist`.

Alterando `B8000000 0000`/`BA000000 0000`/`BA000000 0090`* to `B8 <CoreCount> 0000 0000`/`BA <CoreCount> 0000 0000`/`BA <CoreCount> 0000 0090`* substituindo `<CoreCount>` pelo valor hexadecimal correspondente à sua contagem de núcleos físicos.

**Observação:** *Os três valores diferentes refletem o patch para diferentes versões do macOS. Certifique-se de alterar todos os três se você inicializar o macOS 10.13 para o macOS 12*

Veja a tabela abaixo para os valores correspondentes à contagem de núcleos da sua CPU.

| CoreCount | Hexadecimal|
|--------|---------|
|   6 Core  | `06` |
|   8 Core  | `08` |
|   12 Core | `0C` |
|   16 Core | `10` |
|   32 Core | `20` |

Então, por exemplo, um valor de substituição do 6 Core 5600X resultaria nestes valores de substituição, `B8 06 0000 0000`/`BA 06 0000 0000`/`BA 06 0000 0090`

**Observação:** *A instalação do MacOS Monterey requer que `Misc -> Security -> SecureBootModel` esteja desabilitado na configuração.<br />Também o TPM precisa estar desabilitado no BIOS. Ambos podem ser habilitados após a instalação.*

## TRX40 Systems

Disabling the `mtrr_update_action - fix PAT` patch has shown an improvement in GPU performance on some systems that have tested. If you wish to test this it is recommended to do so on a USB with OpenCore to ensure it works first. There may be issues with different motherboard/GPU combos that we aren't aware of. Proceed at your own risk.

### Uso Geral (General Purpose) `boot-args`

Parameter|Description
:----|:----
npci=0x2000|Isso desabilita alguma depuração PCI relacionada a `kIOPCIConfiguratorPFM64`, a alternativa é `npci=0x3000` que desabilita a depuração relacionada a `gIOPCITunnelledKey` adicionalmente.<br>Necessário para quando ficar preso em `PCI Start Configuration`, pois há conflitos de IRQ relacionados às suas pistas PCI. **Não é necessário se Above4GDecoding estiver habilitado.**
npci=0x3000|Alternativa para `npci=0x2000`.

### Específico para GPU (GPU-Specific) `boot-args`

Parameter|Description
:----|:----
agdpmod=pikera|Usado para desabilitar verificações de ID de placa em GPUs Navi (série RX 5000), sem isso você terá uma tela preta.<br>**Não use se você não tiver Navi** (ou seja, placas Polaris e Vega não devem usar isso).
nvda_drv_vrl=1|Usado para habilitar drivers Nvidia Web em placas Maxwell e Pascal no Sierra e High Sierra.

# Configurações da BIOS

### Desativar
- Fast Boot
- Secure Boot
- Serial/COM Port
- Parallel Port
- Compatibility Support Module (CSM)(Must be off, GPU errors like `gIO` are common when this option in enabled)

***Nota especial para usuários do 3990X***: *o macOS atualmente não suporta mais de 64 threads no kernel, e então o kernel entrará em pânico se vir mais. A CPU 3990X tem 128 threads no total e então requer metade disso desabilitado. Recomendamos desabilitar o hyper threading no BIOS para essas situações.*

### Enable
- Above 4G decoding
	- Isso deve estar ativado, se você não encontrar a opção, adicione`npci=0x2000` a `boot-args`. 
	- Do not have both this option and `npci` in `boot-args` enabled at the same time.
	- If you are on a Gigabyte/Aorus or an AsRock motherboard, enabling this option may break certain drivers(ie. Ethernet) and/or boot failures on other OSes, if it does happen then disable this option and opt for `npci` instead.
	- 2020+ BIOS Notes: When enabling Above4G, Resizable BAR Support may become an available on some X570 and newer motherboards. Please ensure this is **`DISABLED`** instead of set to Auto.
- EHCI/XHCI Hand-off
- OS type: Windows 8.1/10 UEFI Mode
- SATA Mode: AHCI

# References
https://dortania.github.io/OpenCore-Install-Guide/AMD/zen.html
<br>
https://dortania.github.io/Getting-Started-With-ACPI/
