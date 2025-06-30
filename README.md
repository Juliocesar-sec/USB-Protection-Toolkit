# 🛡️ Kali Forensics Station & USB Protection Toolkit

Este repositório contém instruções e boas práticas para transformar um pendrive com Kali Linux em uma **estação portátil de limpeza e análise forense segura**, com foco em **imutabilidade e proteção antiforense**.

---

## 📁 Índice

1. [Estação de Limpeza Forense com Kali Linux](#1-estação-de-limpeza-forense-com-kali-linux)  
2. [Pendrive Anti-Forense com Ventoy + Kali](#2-pendrive-anti-forense-com-ventoy--kali)  
3. [Proteção Física e Lógica do Pendrive](#3-proteção-física-e-lógica-do-pendrive)  

---

## ✅ 1. Estação de Limpeza Forense com Kali Linux

### Objetivo
Utilizar o Kali Live como ambiente seguro e volátil para:

- Análise de dispositivos suspeitos (pendrives, SSDs, HDs)  
- Escaneamento de malware  
- Limpeza segura de dados  
- Investigação sem contaminação do sistema  

### Ferramentas recomendadas

| Tarefa                        | Ferramentas                            |
|------------------------------|--------------------------------------|
| Listar dispositivos           | `lsblk`, `fdisk`, `blkid`            |
| Escanear por malware          | `clamscan`, `rkhunter`, `chkrootkit`|
| Identificar arquivos          | `file`, `strings`, `binwalk`         |
| Limpar discos com segurança   | `shred`, `dd`, `wipe`                |
| Montagem segura               | `mount -o ro`, `udisksctl`           |

### Práticas recomendadas

```bash
# Montar mídia como somente leitura
sudo mount -o ro /dev/sdX1 /mnt/usb

# Verificar tipo real dos arquivos
file /mnt/usb/*

# Escanear com ClamAV
clamscan -r --infected /mnt/usb

# Limpeza segura (3 passagens)
shred -vzn 3 /dev/sdX
## 🔐 2. Pendrive Anti-Forense com Ventoy + Kali

### Objetivo
Utilizar o [Ventoy](https://www.ventoy.net) para criar um pendrive com múltiplas ISOs (incluindo Kali), **sem persistência e sem riscos de contaminação**.

### Etapas:

1. Baixe o Ventoy e instale no pendrive:

   ```bash
   sudo ./Ventoy2Disk.sh -i /dev/sdX

    Copie a ISO do Kali para o pendrive.

    Durante o boot, selecione:

    Live (amd64)

    (Opcional) Crie um ventoy.json com personalizações:

{
  "control_legacy": [
    { "VTOY_DEFAULT_MENU_MODE": "1" },
    { "VTOY_TREE_VIEW_MENU_STYLE": "1" }
  ]
}

Vantagens:

    Nenhuma alteração após reboot (imutável)

    Suporte a múltiplas ISOs

    Leitura 100% na RAM

    Fácil atualização (só substituir ISO)


---

```markdown
## 🛡️ 3. Proteção Física e Lógica do Pendrive

### Proteção via `hdparm` (somente leitura por software)

```bash
# Ativar somente leitura
sudo hdparm -r1 /dev/sdX

# Verificar status
sudo hdparm -r /dev/sdX

# Desativar (volta a permitir escrita)
sudo hdparm -r0 /dev/sdX

    ⚠️ Funciona em alguns dispositivos USB. A proteção é temporária (perde após reboot).

Proteção física (pendrives com chave de gravação)

    Use pendrives com interruptor de proteção (ex: modelos antigos da Kingston, Transcend).

    Ideal para evitar qualquer modificação acidental ou maliciosa.


---

Se quiser, posso ajudar também a criar um script simples para ativar/desativar o modo somente leitura com `hdparm`. Quer?


