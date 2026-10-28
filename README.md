# Hardcore Anti-Grabber

Auditoria defensiva independente para Windows x64, voltada a indícios associados
a grabbers e stealers. Este repositório distribui documentação e releases
binárias. O código-fonte da aplicação não é publicado aqui.

**[Downloads e versões](https://github.com/luqfuck/hardcore-anti-grabber/releases)**

## O que faz

| Camada | Evidência examinada |
|---|---|
| Processos | Snapshot de processos ativos e caminhos acessíveis |
| Inicialização | Run/RunOnce nas visões 32/64 bits e arquivos diretos de Startup |
| Assinatura | Validação Authenticode usando cache local |
| Arquivo PE | Combinação de referências ASCII/UTF-16 e imports reais relacionados a coleta, API de conta e transporte |
| Histórico | Correlação de eventos Sysmon existentes por ProcessGuid, imagem e janela temporal |
| Relatório | JSON local com severidade, confiança, escopo, cobertura e referências de evidência |

Não depende do Scanner principal. Não instala serviço ou driver. É uma auditoria
pontual, não um antivírus residente ou bloqueador de tráfego.

## Começar

1. Baixe o pacote Windows x64 na página de Releases e extraia para uma pasta gravável.
2. Abra um terminal nessa pasta.
3. Execute a auditoria:

```powershell
.\HardcoreAntiGrabber.exe --output .\relatorio.json
```

Sem `--output`, o arquivo `anti-grabber-report.json` fica ao lado do executável.
A execução usa as permissões atuais; falhas de acesso podem reduzir a cobertura.
O Sysmon é opcional e precisa estar previamente instalado/configurado para a
camada histórica funcionar. O programa não o instala nem altera sua política.

### Examinar somente um arquivo

```powershell
.\HardcoreAntiGrabber.exe --file C:\Analise\arquivo.exe --output .\arquivo.json
```

O arquivo é tratado como dados, sem execução ou carregamento como DLL. Essa
opção também permite analisar um PE assinado. `--stdin` aceita um stream de bytes
PE brutos; não é um extrator de RAR/ZIP nem um leitor de nomes de arquivo.

### Verificar o funcionamento

```powershell
.\HardcoreAntiGrabber.exe --self-test
.\HardcoreAntiGrabber.exe --help
```

Os testes internos usam fixtures sintéticas sem instruções executáveis. Não
executam malware nem acessam credenciais para simular um ataque.

## Interpretar os resultados

**Severidade é prioridade de revisão; confiança é força do indício.** Um alerta
não é uma confirmação de infecção. O relatório separa capacidade estática de
evidência histórica e informa as limitações da coleta.

A regra PE exige uma combinação de capacidades. A presença de um nome, um
webhook ou uma API isolada não basta para essa regra. Um resultado estático alto
tem confiança média: ele não demonstra que o arquivo foi executado ou enviou dados.

A correlação histórica verifica a mesma ProcessGuid e imagem, em até 300 segundos.
Direito de leitura sobre um processo sensível recebe prioridade média e baixa
confiança. A conexão posterior a domínio Discord ainda não comprova upload.
A combinação adicional com a criação de um artefato específico eleva a prioridade.
Ferramentas de diagnóstico, acessibilidade e segurança podem produzir sinais parecidos.

Segundo a [documentação do Sysmon](https://learn.microsoft.com/sysinternals/downloads/sysmon),
os eventos dependem da configuração existente. Acesso a processo não prova leitura
de bytes; conexão não mostra o corpo HTTP. Ausência de eventos não prova ausência de roubo.

| Código de saída | Significado |
|---|---|
| 0 | Operação concluída sem achado médio/alto; não certifica máquina limpa |
| 2 | Argumentos inválidos ou conflito entre entrada e saída |
| 3 | Falha ao gravar o relatório |
| 4 | Imagem não analisada ou cobertura incompleta, sem alerta de prioridade maior |
| 5 | Falha do self-test |
| 10 | Achado médio para revisão |
| 20 | Achado alto para revisão |

`10/20` têm precedência sobre `4`. Sempre examine também `coverage`, `warnings`
e `limitations`, mesmo quando há alertas.

## Cobertura e limites

- PE automático: imagens executáveis de processos/persistências cuja assinatura
  não foi validada; até 64 candidatos, 32 MiB por imagem e 128 MiB totais.
- Binários com assinatura validada ficam fora da seleção automática. Assinatura
  válida não garante ausência de comportamento malicioso; use `--file` quando necessário.
- Sysmon: últimos 60 minutos, até 10.000 eventos e orçamento de 10 segundos de
  aquisição. Limites e falhas são registrados explicitamente.
- Processos já encerrados podem não aparecer no snapshot. Uma consulta histórica
  concluída não comprova que a política capturava todos os eventos relevantes.
- Sem leitura genérica de arquivos de perfil, análise de scripts, desempacotamento,
  inspeção de extensões de navegador, tarefas agendadas ou serviços. Destinos de
  atalhos Startup não são resolvidos nesta versão.
- Strings cifradas e importação dinâmica podem impedir coincidências estáticas.
  `no_static_rule_match` significa apenas que a regra não coincidiu.
- Comandos de inicialização sem aspas podem ter destino ambíguo.

## Privacidade

A aquisição é somente leitura; a auditoria grava seu relatório local. Não lê bases
de senhas, cookies, tokens, carteiras nem memória de outros processos. Não armazena
linhas de comando brutas, não encerra processos, não altera persistência e não
quarentena arquivos. Não envia o relatório a um servidor.

Relatórios podem conter caminhos, nomes de arquivos, GUIDs e IDs de eventos.
Revise esses dados antes de compartilhar. Não envie credenciais, tokens ou dumps
de perfis em issues.

## Distribuição

Cada release informa seus testes, proteção aplicada e hashes SHA-256. VMProtect,
quando indicado na release, protege o binário; não equivale a assinatura digital
nem garante impossibilidade de engenharia reversa.

Confira um download com:

```powershell
Get-FileHash .\HardcoreAntiGrabber.exe -Algorithm SHA256
```

Compare com `SHA256SUMS.txt` da mesma versão. Os arquivos automáticos “Source code”
do GitHub contêm somente a documentação deste repositório, não o código da aplicação.
