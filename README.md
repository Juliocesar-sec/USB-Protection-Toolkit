🛡️ Kali Forensics Station & USB Protection Toolkit

Este repositório contém instruções e boas práticas para transformar um pendrive com Kali Linux em uma estação portátil de limpeza e análise forense segura, com foco em imutabilidade e proteção antiforense.

📁 Índice

    1. Estação de Limpeza Forense com Kali Linux

    2. Pendrive Anti-Forense com Ventoy + Kali

    3. Proteção Física e Lógica do Pendrive

1. Estação de Limpeza Forense com Kali Linux

Objetivo

Utilizar o Kali Live como um ambiente seguro e volátil para:

    Analisar dispositivos suspeitos (pendrives, SSDs, HDs)

    Escanear por malware

    Realizar limpeza segura de dados

    Conduzir investigações sem contaminar o sistema

Ferramentas Recomendadas

Tarefa
	

Ferramentas

Listar dispositivos
	

lsblk, fdisk, blkid

Escanear por malware
	

clamscan, rkhunter, chkrootkit

Identificar arquivos
	

file, strings, binwalk

Limpar discos com segurança
	

shred, dd, wipe

Montagem segura
	

mount -o ro, udisksctl

Práticas Recomendadas

Bash

# Montar mídia como somente leitura
sudo mount -o ro /dev/sdX1 /mnt/usb

# Verificar tipo real dos arquivos
file /mnt/usb/*

# Escanear com ClamAV
clamscan -r --infected /mnt/usb

# Limpeza segura (3 passagens)
shred -vzn 3 /dev/sdX

2. Pendrive Anti-Forense com Ventoy + Kali

Objetivo

Utilizar o Ventoy para criar um pendrive com múltiplas ISOs (incluindo Kali), sem persistência e sem riscos de contaminação.

Etapas

    Baixe e instale o Ventoy no pendrive:
    Bash

sudo ./Ventoy2Disk.sh -i /dev/sdX

Copie a ISO do Kali para o pendrive.

Durante o boot, selecione: Live (amd64)

(Opcional) Crie um arquivo ventoy.json com as seguintes personalizações:
JSON

    {
      "control_legacy": [
        { "VTOY_DEFAULT_MENU_MODE": "1" },
        { "VTOY_TREE_VIEW_MENU_STYLE": "1" }
      ]
    }

Vantagens

    Imutabilidade: Nenhuma alteração após o reboot.

    Flexibilidade: Suporte a múltiplas ISOs.

    Desempenho: Leitura 100% na RAM.

    Facilidade: Fácil atualização (basta substituir a ISO).

3. Proteção Física e Lógica do Pendrive

Proteção via hdparm (modo somente leitura por software)

Bash

# Ativar somente leitura
sudo hdparm -r1 /dev/sdX

# Verificar status
sudo hdparm -r /dev/sdX

# Desativar (volta a permitir escrita)
sudo hdparm -r0 /dev/sdX

⚠️ Importante: Essa função funciona em alguns dispositivos USB e a proteção é temporária (perde após o reboot).

Proteção Física (pendrives com chave de gravação)

Use pendrives que possuam um interruptor de proteção contra gravação (exemplos: modelos antigos da Kingston, Transcend). Essa é a forma mais eficaz de evitar qualquer modificação acidental ou maliciosa.

Script para Controle do Modo Somente Leitura com hdparm

Para facilitar o uso do hdparm para ativar, desativar e verificar o modo somente leitura, você pode utilizar este script:
Bash

#!/bin/bash

# Script para controlar o modo somente leitura via hdparm
# Uso: ./hdparm-readonly.sh /dev/sdX on|off|status

DEVICE="$1"
ACTION="$2"

if [[ -z "$DEVICE" || -z "$ACTION" ]]; then
  echo "Uso: $0 /dev/sdX on|off|status"
  exit 1
fi

if [[ ! -b "$DEVICE" ]]; then
  echo "Erro: dispositivo $DEVICE não encontrado."
  exit 1
fi

case "$ACTION" in
  on)
    echo "Ativando somente leitura em $DEVICE ..."
    sudo hdparm -r1 "$DEVICE"
    ;;
  off)
    echo "Desativando somente leitura em "$DEVICE" ..."
    sudo hdparm -r0 "$DEVICE"
    ;;
  status)
    echo "Status de somente leitura em "$DEVICE":"
    sudo hdparm -r "$DEVICE"
    ;;
  *)
    echo "Ação inválida. Use on, off ou status."
    exit 1
    ;;
esac

Como usar:

Bash

chmod +x hdparm-readonly.sh
./hdparm-readonly.sh /dev/sdX on     # Ativa somente leitura
./hdparm-readonly.sh /dev/sdX off    # Desativa somente leitura
./hdparm-readonly.sh /dev/sdX status # Verifica status

⚠️ Atenção: Lembre-se que essa proteção é temporária e pode não funcionar em todos os dispositivos USB.
