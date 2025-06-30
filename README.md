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
