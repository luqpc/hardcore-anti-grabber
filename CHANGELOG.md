# Histórico

## v2.1.0 — 28/10/2026

- Dois cliques iniciam a análise e mantêm a janela aberta com menu em português.
- Andamento, achados, caminhos, confiança e cobertura parcial aparecem no CMD.
- TXT e JSON salvos automaticamente na pasta local de relatórios do usuário.
- Seleção de arquivo por diálogo e suporte a arrastar arquivo para o EXE.
- Tratamento assistido pelo Microsoft Defender instalado/ativo, com confirmação
  interativa, elevação quando necessária e validação da assinatura Microsoft.
- Resultados do Defender não são traduzidos automaticamente como “removido”.
- Acesso à Segurança do Windows e testes contra tratamento por entrada redirecionada.
- VMProtect mantido. Código-fonte, SDK e relatórios locais fora da distribuição.

Limite de validação: não houve remoção real de malware durante a homologação.
O tratamento depende da disponibilidade e das configurações do Defender.

## v2.0.0 — 28/10/2026

Primeira distribuição pública da geração v2 do Hardcore Anti-Grabber.

- Aplicação de console independente para Windows x64.
- Snapshot de processos e entradas de inicialização.
- Avaliação Authenticode sem consultas de rede pelo verificador.
- Inspeção estática PE limitada, com validação de imports, ASCII/UTF-16 e SHA-256.
- Correlação de capacidades de coleta, API de conta e transporte no mesmo arquivo.
- Histórico Sysmon existente por identidade da execução e janela temporal.
- Relatório `hardcore-anti-grabber/v2`, com confiança e cobertura explícitas.
- Modos `--file`, `--stdin`, `--output`, `--self-test` e `--help`.
- Distribuição protegida com VMProtect Lite 3.8.7; virtualização das rotinas
  de indicadores e correlação. Verificação com `--protection-status`.
- Testes negativos para PE inválido, limites, imports ausentes/corrompidos,
  GUIDs distintos/nulos, ordenação temporal e domínios com sufixos enganosos.

Sem bloqueio automático, instalação de sensores ou coleta de credenciais.
Os limites completos estão no README. A validação com amostra de referência e
controles benignos não constitui uma medição geral de taxa de detecção.
