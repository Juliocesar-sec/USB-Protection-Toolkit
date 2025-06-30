
    <title>Kali Forensics Station & USB Protection Toolkit</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            line-height: 1.6;
            margin: 20px;
            max-width: 900px;
            margin-left: auto;
            margin-right: auto;
            color: #333;
        }
        h1 {
            color: #2c3e50;
            border-bottom: 2px solid #3498db;
            padding-bottom: 10px;
            margin-top: 30px;
        }
        h2 {
            color: #34495e;
            border-bottom: 1px solid #ccc;
            padding-bottom: 5px;
            margin-top: 25px;
        }
        h3 {
            color: #2980b9;
            margin-top: 20px;
        }
        strong {
            font-weight: bold;
        }
        ul {
            list-style-type: disc;
            margin-left: 20px;
        }
        ol {
            list-style-type: decimal;
            margin-left: 20px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 15px;
            margin-bottom: 15px;
        }
        table, th, td {
            border: 1px solid #ddd;
        }
        th, td {
            padding: 8px;
            text-align: left;
        }
        th {
            background-color: #f2f2f2;
        }
        pre {
            background-color: #ecf0f1;
            padding: 10px;
            border-radius: 5px;
            overflow-x: auto;
            font-family: "Courier New", Courier, monospace;
            white-space: pre-wrap; /* Garante quebras de linha */
            word-wrap: break-word; /* Garante quebras de palavra */
        }
        code {
            background-color: #e0e0e0;
            padding: 2px 4px;
            border-radius: 3px;
            font-family: "Courier New", Courier, monospace;
        }
        hr {
            border: 0;
            height: 1px;
            background: #eee;
            margin: 20px 0;
        }
        .warning {
            background-color: #fff3cd;
            border-left: 4px solid #ffc107;
            padding: 10px;
            margin-top: 15px;
            margin-bottom: 15px;
            color: #664d03;
        }
        a {
            color: #3498db;
            text-decoration: none;
        }
        a:hover {
            text-decoration: underline;
        }
    </style>
</head>
<body>

    <h1>&#128737; Kali Forensics Station &amp; USB Protection Toolkit</h1>

    <p>Este repositório contém instruções e boas práticas para transformar um pendrive com Kali Linux em uma <strong>estação portátil de limpeza e análise forense segura</strong>, com foco em <strong>imutabilidade e proteção antiforense</strong>.</p>

    <hr>

    <h2>&#128193; Índice</h2>

    <ul>
        <li><a href="#1-estação-de-limpeza-forense-com-kali-linux">1. Estação de Limpeza Forense com Kali Linux</a></li>
        <li><a href="#2-pendrive-anti-forense-com-ventoy--kali">2. Pendrive Anti-Forense com Ventoy + Kali</a></li>
        <li><a href="#3-proteção-física-e-lógica-do-pendrive">3. Proteção Física e Lógica do Pendrive</a></li>
    </ul>

    <hr>

    <h2>1. Estação de Limpeza Forense com Kali Linux</h2>

    <h3>Objetivo</h3>

    <p>Utilizar o <strong>Kali Live</strong> como um ambiente seguro e volátil para:</p>

    <ul>
        <li>Analisar dispositivos suspeitos (pendrives, SSDs, HDs)</li>
        <li>Escanear por malware</li>
        <li>Realizar limpeza segura de dados</li>
        <li>Conduzir investigações sem contaminar o sistema</li>
    </ul>

    <h3>Ferramentas Recomendadas</h3>

    <table>
        <thead>
            <tr>
                <th>Tarefa</th>
                <th>Ferramentas</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>Listar dispositivos</td>
                <td><code>lsblk</code>, <code>fdisk</code>, <code>blkid</code></td>
            </tr>
            <tr>
                <td>Escanear por malware</td>
                <td><code>clamscan</code>, <code>rkhunter</code>, <code>chkrootkit</code></td>
            </tr>
            <tr>
                <td>Identificar arquivos</td>
                <td><code>file</code>, <code>strings</code>, <code>binwalk</code></td>
            </tr>
            <tr>
                <td>Limpar discos com segurança</td>
                <td><code>shred</code>, <code>dd</code>, <code>wipe</code></td>
            </tr>
            <tr>
                <td>Montagem segura</td>
                <td><code>mount -o ro</code>, <code>udisksctl</code></td>
            </tr>
        </tbody>
    </table>

    <h3>Práticas Recomendadas</h3>

    <pre><code># Montar mídia como somente leitura
sudo mount -o ro /dev/sdX1 /mnt/usb

# Verificar tipo real dos arquivos
file /mnt/usb/*

# Escanear com ClamAV
clamscan -r --infected /mnt/usb

# Limpeza segura (3 passagens)
shred -vzn 3 /dev/sdX
</code></pre>

    <hr>

    <h2>2. Pendrive Anti-Forense com Ventoy + Kali</h2>

    <h3>Objetivo</h3>

    <p>Utilizar o <strong>Ventoy</strong> para criar um pendrive com múltiplas ISOs (incluindo Kali), sem persistência e sem riscos de contaminação.</p>

    <h3>Etapas</h3>

    <ol>
        <li>Baixe e instale o <strong>Ventoy</strong> no pendrive:
            <pre><code>sudo ./Ventoy2Disk.sh -i /dev/sdX</code></pre>
        </li>
        <li>Copie a ISO do Kali para o pendrive.</li>
        <li>Durante o boot, selecione: <code>Live (amd64)</code></li>
        <li>(Opcional) Crie um arquivo <code>ventoy.json</code> com as seguintes personalizações:
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
        <li><strong>Imutabilidade:</strong> Nenhuma alteração após o reboot.</li>
        <li><strong>Flexibilidade:</strong> Suporte a múltiplas ISOs.</li>
        <li><strong>Desempenho:</strong> Leitura 100% na RAM.</li>
        <li><strong>Facilidade:</strong> Fácil atualização (basta substituir a ISO).</li>
    </ul>

    <hr>

    <h2>3. Proteção Física e Lógica do Pendrive</h2>

    <h3>Proteção via <code>hdparm</code> (modo somente leitura por software)</h3>

    <pre><code># Ativar somente leitura
sudo hdparm -r1 /dev/sdX

# Verificar status
sudo hdparm -r /dev/sdX

# Desativar (volta a permitir escrita)
sudo hdparm -r0 /dev/sdX
</code></pre>

    <p class="warning">&#9888;&#65039; <strong>Importante:</strong> Essa função funciona em alguns dispositivos USB e a proteção é <strong>temporária</strong> (perde após o reboot).</p>

    <h3>Proteção Física (pendrives com chave de gravação)</h3>

    <p>Use pendrives que possuam um <strong>interruptor de proteção contra gravação</strong> (exemplos: modelos antigos da Kingston, Transcend). Essa é a forma mais eficaz de evitar qualquer modificação acidental ou maliciosa.</p>

    <h3>Script para Controle do Modo Somente Leitura com <code>hdparm</code></h3>

    <p>Para facilitar o uso do <code>hdparm</code> para ativar, desativar e verificar o modo somente leitura, você pode utilizar este script:</p>

    <pre><code>#!/bin/bash

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
</code></pre>

    <h4>Como usar:</h4>

    <pre><code>chmod +x hdparm-readonly.sh
./hdparm-readonly.sh /dev/sdX on     # Ativa somente leitura
./hdparm-readonly.dev/sdX off    # Desativa somente leitura
./hdparm-readonly.sh /dev/sdX status # Verifica status
</code></pre>

    <p class="warning">&#9888;&#65039; <strong>Atenção:</strong> Lembre-se que essa proteção é temporária e pode não funcionar em todos os dispositivos USB.</p>

</body>
</html>
