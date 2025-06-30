<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Kali Forensics Station & USB Protection Toolkit</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      max-width: 900px;
      margin: auto;
      padding: 20px;
      background-color: #f5f7fa;
      color: #222;
      line-height: 1.6;
    }
    h1, h2, h3 {
      color: #2c3e50;
    }
    pre {
      background-color: #2d2d2d;
      color: #f8f8f2;
      padding: 12px;
      border-radius: 6px;
      overflow-x: auto;
    }
    code {
      font-family: monospace;
      background-color: #eaeaea;
      padding: 2px 4px;
      border-radius: 3px;
    }
    table {
      border-collapse: collapse;
      width: 100%;
      margin-bottom: 20px;
    }
    th, td {
      border: 1px solid #bbb;
      padding: 8px 12px;
      text-align: left;
    }
    th {
      background-color: #2980b9;
      color: white;
    }
    blockquote {
      border-left: 4px solid #2980b9;
      margin-left: 0;
      padding-left: 10px;
      color: #555;
      font-style: italic;
      background-color: #eaf3fb;
      border-radius: 3px;
    }
    nav a {
      text-decoration: none;
      color: #2980b9;
      margin-right: 15px;
    }
    nav a:hover {
      text-decoration: underline;
    }
  </style>
</head>
<body>

  <h1>🛡️ Kali Forensics Station &amp; USB Protection Toolkit</h1>

  <!--
  Introdução:
  Este projeto oferece boas práticas para transformar um pendrive com Kali Linux em uma estação portátil segura
  para análise forense, com foco em imutabilidade e proteção antiforense.
  -->

  <nav>
    <a href="#forensic-station">1. Estação de Limpeza Forense com Kali Linux</a>
    <a href="#ventoy-kali">2. Pendrive Anti-Forense com Ventoy + Kali</a>
    <a href="#usb-protection">3. Proteção Física e Lógica do Pendrive</a>
  </nav>

  <hr />

  <section id="forensic-station">
    <h2>1. Estação de Limpeza Forense com Kali Linux</h2>

    <h3>Objetivo</h3>
    <p>
      Utilizar o Kali Live como ambiente seguro e volátil para:
    </p>
    <ul>
      <li>Análise de dispositivos suspeitos (pendrives, SSDs, HDs)</li>
      <li>Escaneamento de malware</li>
      <li>Limpeza segura de dados</li>
      <li>Investigação sem contaminação do sistema</li>
    </ul>

    <h3>Ferramentas recomendadas</h3>
    <table>
      <thead>
        <tr>
          <th>Tarefa</th>
          <th>Ferramentas</th>
        </tr>
      </thead>
      <tbody>
        <tr><td>Listar dispositivos</td><td><code>lsblk</code>, <code>fdisk</code>, <code>blkid</code></td></tr>
        <tr><td>Escanear por malware</td><td><code>clamscan</code>, <code>rkhunter</code>, <code>chkrootkit</code></td></tr>
        <tr><td>Identificar arquivos</td><td><code>file</code>, <code>strings</code>, <code>binwalk</code></td></tr>
        <tr><td>Limpar discos com segurança</td><td><code>shred</code>, <code>dd</code>, <code>wipe</code></td></tr>
        <tr><td>Montagem segura</td><td><code>mount -o ro</code>, <code>udisksctl</code></td></tr>
      </tbody>
    </table>

    <h3>Práticas recomendadas</h3>
    <pre><code># Montar mídia como somente leitura
sudo mount -o ro /dev/sdX1 /mnt/usb

# Verificar tipo real dos arquivos
file /mnt/usb/*

# Escanear com ClamAV
clamscan -r --infected /mnt/usb

# Limpeza segura (3 passagens)
shred -vzn 3 /dev/sdX
</code></pre>
  </section>

  <hr />

  <section id="ventoy-kali">
    <h2>2. Pendrive Anti-Forense com Ventoy + Kali</h2>

    <h3>Objetivo</h3>
    <p>
      Utilizar o Ventoy para criar um pendrive com múltiplas ISOs (incluindo Kali),
      sem persistência e sem riscos de contaminação.
    </p>

    <h3>Etapas</h3>
    <ol>
      <li>Baixe o Ventoy e instale no pendrive:
        <pre><code>sudo ./Ventoy2Disk.sh -i /dev/sdX</code></pre>
      </li>
      <li>Copie a ISO do Kali para o pendrive.</li>
      <li>Durante o boot, selecione a opção:
        <pre><code>Live (amd64)</code></pre>
      </li>
      <li>(Opcional) Crie um arquivo <code>ventoy.json</code> com as personalizações:
        <pre><code>{
  "control_legacy": [
    { "VTOY_DEFAULT_MENU_MODE": "1" },
    { "VTOY_TREE_VIEW_MENU_STYLE": "1" }
  ]
}</code></pre>
      </li>
    </ol>

    <h3>Vantagens</h3>
    <ul>
      <li>Nenhuma alteração após reboot (imutável)</li>
      <li>Suporte a múltiplas ISOs</li>
      <li>Leitura 100% na RAM</li>
      <li>Fácil atualização (só substituir ISO)</li>
    </ul>
  </section>

  <hr />

  <section id="usb-protection">
    <h2>3. Proteção Física e Lógica do Pendrive</h2>

    <h3>Proteção via <code>hdparm</code> (modo somente leitura por software)</h3>
    <pre><code># Ativar somente leitura
sudo hdparm -r1 /dev/sdX

# Verificar status
sudo hdparm -r /dev/sdX

# Desativar (volta a permitir escrita)
sudo hdparm -r0 /dev/sdX
</code></pre>
    <blockquote>
      ⚠️ Essa proteção é temporária e funciona em alguns dispositivos USB. Perde após reboot.
    </blockquote>

    <h3>Proteção física (pendrives com chave de gravação)</h3>
    <p>
      Use pendrives com interruptor físico de proteção contra gravação (exemplo: modelos antigos da Kingston, Transcend).<br />
      Ideal para evitar modificações acidentais ou maliciosas.
    </p>

    <h3>Script para controle do modo somente leitura com <code>hdparm</code></h3>
    <p>
      Script para ativar, desativar e verificar o modo somente leitura de forma simples.
    </p>

    <pre><code>#!/bin/bash

# Script para controlar o modo somente leitura via hdparm
# Uso: ./hdparm-readonly.sh /dev/sdX on|off|status

DEVICE="$1"  # Dispositivo USB (exemplo: /dev/sdb)
ACTION="$2"  # Ação: on, off ou status

# Verifica se os parâmetros foram informados
if [[ -z "$DEVICE" || -z "$ACTION" ]]; then
  echo "Uso: $0 /dev/sdX on|off|status"
  exit 1
fi

# Verifica se o dispositivo informado existe
if [[ ! -b "$DEVICE" ]]; then
  echo "Erro: dispositivo $DEVICE não encontrado."
  exit 1
fi

# Executa a ação conforme o parâmetro
case "$ACTION" in
  on)
    echo "Ativando modo somente leitura em $DEVICE ..."
    sudo hdparm -r1 "$DEVICE"
    ;;
  off)
    echo "Desativando modo somente leitura em $DEVICE ..."
    sudo hdparm -r0 "$DEVICE"
    ;;
  status)
    echo "Status do modo somente leitura em $DEVICE:"
    sudo hdparm -r "$DEVICE"
    ;;
  *)
    echo "Ação inválida. Use on, off ou status."
    exit 1
    ;;
esac
</code></pre>

    <h4>Como usar:</h4>
    <ol>
      <li>Salve o script acima em um arquivo chamado <code>hdparm-readonly.sh</code>.</li>
      <li>Dê permissão de execução:
        <pre><code>chmod +x hdparm-readonly.sh</code></pre>
      </li>
      <li>Execute o script com o dispositivo e a ação desejada:
        <pre><code>
./hdparm-readonly.sh /dev/sdX on     # Ativa somente leitura
./hdparm-readonly.sh /dev/sdX off    # Desativa somente leitura
./hdparm-readonly.sh /dev/sdX status # Verifica status
        </code></pre>
      </li>
    </ol>

    <blockquote>
      ⚠️ Essa proteção é temporária e pode não funcionar em todos os dispositivos USB.
    </blockquote>

  </section>

</body>
</html>
