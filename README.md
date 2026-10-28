# Hardcore Anti-Grabber

**Verifique possíveis sinais de stealer sem precisar saber usar CMD.**

**[Baixar para Windows x64](https://github.com/luqpc/hardcore-anti-grabber/releases/latest/download/HardcoreAntiGrabber.exe)** · [Todas as versões](https://github.com/luqpc/hardcore-anti-grabber/releases)

## Como usar

1. Baixe o executável.
2. Abra `HardcoreAntiGrabber.exe` com **dois cliques**.
3. Aguarde a verificação. O andamento e o resultado aparecem na própria janela.
4. Escolha uma opção pelo número e pressione Enter.

Você não precisa abrir um terminal, digitar comandos nem escolher onde salvar
o relatório. A janela permanece aberta até selecionar **0 — Fechar**.

```text
O QUE DESEJA FAZER?
  1 - Verificar e tratar com Microsoft Defender
  2 - Ver todos os detalhes nesta janela
  3 - Abrir Segurança do Windows (histórico e quarentena)
  4 - Verificar outro arquivo (abre uma janela para escolher)
  5 - Verificar novamente o computador
  0 - Fechar
```

Para examinar um programa específico, use a opção 4 ou arraste o arquivo para
o ícone do anti-grabber. O arquivo selecionado é analisado sem ser executado.

## Verificar e tratar

A opção 1 usa o **Microsoft Defender já instalado e ativo**. O Windows poderá
pedir permissão de administrador; a etapa também pede confirmação antes de iniciar.

- Sem arquivo selecionado, faz uma verificação rápida do computador.
- Depois de selecionar um arquivo, verifica esse arquivo especificamente.
- O Defender pode colocar ameaças em quarentena ou removê-las conforme as
  configurações existentes do Windows.
- Se o Defender estiver indisponível, o programa informa e não inicia tratamento.
- Os alertas do anti-grabber, sozinhos, não provocam exclusão de arquivos.

**O programa não promete remover todos os stealers.** Um alerta não confirma
infecção e ausência de alerta não certifica máquina limpa. O retorno de sucesso
do Defender pode significar nenhuma ameaça encontrada **ou** ameaça tratada;
ele não informa sozinho quantos arquivos foram removidos. Veja o resultado no
Histórico de proteção da Segurança do Windows. Itens em quarentena podem ter
opção de restauração; nem toda remoção pode ser desfeita.

A integração segue o [comportamento documentado do Microsoft Defender](https://learn.microsoft.com/en-us/defender-endpoint/command-line-arguments-microsoft-defender-antivirus).
Não altera exclusões, políticas, configurações de nuvem ou serviços do Windows.

## Resultado e relatórios

A tela mostra os programas/arquivos envolvidos, a confiança do indício e quando
a verificação ficou incompleta. Indícios fracos não são apresentados como
confirmação de roubo.

No modo simples, os relatórios são salvos automaticamente em:

```text
%LOCALAPPDATA%\HardcoreAntiGrabber\Relatorios
```

Há uma versão TXT para leitura e JSON para análise técnica. A análise continua
visível no CMD mesmo quando não é possível salvar o relatório.

## O que é examinado

| Camada | O que procura |
|---|---|
| Processos | Programas ativos e caminhos acessíveis |
| Inicialização | Run/RunOnce e arquivos diretos de Startup |
| Assinatura | Authenticode com cache local |
| Arquivo PE | Combinações de referências e imports relacionados à coleta e envio |
| Histórico | Eventos Sysmon existentes ligados à mesma execução |
| Tratamento | Verificação e ações do Microsoft Defender, após consentimento |

É independente do Scanner principal e não instala driver, serviço ou sensor.
O Sysmon é opcional; sem ele, a camada histórica pode ficar indisponível.

## Privacidade e limites

A análise própria é somente leitura: não abre bases de senhas, cookies, tokens,
carteiras nem memória de outros processos. Não executa amostras, não armazena
comandos brutos e não envia seus relatórios a um servidor. O tratamento separado
usa o Defender e suas configurações existentes.

Relatórios podem conter caminhos e identificadores. Revise antes de compartilhar;
não envie credenciais ou dumps de perfis em issues.

- PE automático: até 64 candidatos, 32 MiB por imagem e 128 MiB totais.
- A seleção automática não inclui imagens com assinatura validada; uma seleção
  explícita permite examiná-las. Assinatura válida não garante ausência de malware.
- Histórico: últimos 60 minutos, até 10.000 eventos e orçamento de 10 segundos
  para aquisição. Cobertura depende da política Sysmon existente.
- Acesso a processo não prova leitura de bytes; conexão não prova envio de dados.
- Sem análise própria de scripts, desempacotamento, extensões de navegador,
  tarefas agendadas, serviços ou destinos de atalhos Startup.
- Strings cifradas e importação dinâmica podem impedir coincidências.
- Uma verificação rápida do Defender não substitui todas as modalidades de exame.

## Para usuários técnicos

```powershell
.\HardcoreAntiGrabber.exe --output .\relatorio.json
.\HardcoreAntiGrabber.exe --file C:\Analise\arquivo.exe --output .\arquivo.json
.\HardcoreAntiGrabber.exe --interactive
.\HardcoreAntiGrabber.exe --self-test
.\HardcoreAntiGrabber.exe --defender-info
.\HardcoreAntiGrabber.exe --protection-status
```

Com argumentos técnicos, o programa também mostra resultados legíveis no console.
`--stdin` recebe bytes PE brutos, não arquivos RAR/ZIP. A limpeza exige confirmação
em console interativo; entrada redirecionada não a autoriza.

Na análise CLI: `0` = sem alerta médio/alto, `2` = argumentos inválidos,
`3` = erro de relatório, `4` = análise incompleta, `5` = self-test falhou,
`10` = revisão média, `20` = revisão alta. Alertas têm precedência sobre o código
de cobertura incompleta. O menu simples retorna `0` ao fechar, sem certificar
o estado da máquina.

## Distribuição e validação

A v2.1.0 usa VMProtect Lite 3.8.7 nas rotinas de indicadores estáticos e correlação.
O executável **não tem assinatura Authenticode**. VMProtect não substitui assinatura
digital nem garante impossibilidade de engenharia reversa.

Builds e testes cobrem regras, controles negativos, interface no console,
cancelamento, entrada redirecionada e interpretação dos códigos do Defender.
**Não houve remoção real de malware durante a homologação.**

Repositório público contém somente documentação; o código-fonte não é distribuído.
Os arquivos automáticos “Source code” do GitHub também contêm apenas documentação.
As releases oferecem EXE, ZIP com instruções e hashes SHA-256.
