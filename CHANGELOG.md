# Histórico

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
- Testes negativos para PE inválido, limites, imports ausentes/corrompidos,
  GUIDs distintos/nulos, ordenação temporal e domínios com sufixos enganosos.

Sem bloqueio automático, instalação de sensores ou coleta de credenciais.
Os limites completos estão no README. A validação com amostra de referência e
controles benignos não constitui uma medição geral de taxa de detecção.
