# Auditoria de Testes de Software e Plano de Evolução da Qualidade 

## 📖 Descrição

Este repositório contém o desenvolvimento da **Atividade 3** da disciplina de Engenharia de Software, cujo objetivo foi realizar uma auditoria da estratégia de testes do projeto open source **Screenpipe**, avaliando seu nível de maturidade em qualidade de software e propondo melhorias para evolução do processo de testes.

O trabalho foi desenvolvido a partir da análise do repositório oficial, documentação técnica, workflows de integração contínua e estrutura dos testes existentes.

---

## 🎯 Objetivos

- Avaliar a estratégia atual de testes do projeto.
- Identificar pontos fortes e oportunidades de melhoria.
- Analisar a qualidade dos testes implementados.
- Identificar lacunas de cobertura.
- Elaborar um plano de evolução da qualidade alinhado às boas práticas de Engenharia de Software.

---

## 📂 Projeto Analisado

- **Projeto:** Screenpipe
- **Repositório Oficial:** https://github.com/screenpipe/screenpipe

---

## 📋 Estrutura da Auditoria

### Eixo A — Estratégia Atual de Testes

Análise da estrutura de testes do projeto, organização da suíte de testes, integração contínua (CI/CD) e documentação existente.

---

### Eixo B — Qualidade dos Testes

Avaliação da organização dos testes, legibilidade, isolamento, reutilização de código, uso de mocks e boas práticas adotadas.

---

### Eixo C — Lacunas e Riscos

Identificação de funcionalidades críticas sem cobertura adequada de testes, riscos de regressão e impactos na manutenção do sistema.

---

### Eixo D — Plano de Evolução da Qualidade

Proposição de melhorias para a estratégia de testes, incluindo:

- Priorização de funcionalidades críticas;
- Ampliação da cobertura de testes;
- Automação da medição de cobertura de código;
- Evolução da integração contínua;
- Definição de critérios para aumento da maturidade do processo de testes.

---

## 🔎 Evidências Utilizadas

Durante a auditoria foram analisados:

- Estrutura do repositório;
- Arquivos de testes;
- Workflows do GitHub Actions;
- Arquivo `TESTING.md`;
- Issues e Pull Requests relacionados à qualidade do software;
- Organização dos módulos do projeto.

---

## 📌 Principais Resultados

A análise demonstrou que o Screenpipe possui uma base consistente de testes e integração contínua, porém ainda apresenta oportunidades de melhoria relacionadas à cobertura de funcionalidades críticas, automação de testes de regressão e monitoramento sistemático da qualidade.

Como resultado, foi elaborado um plano de evolução incremental visando aumentar a confiabilidade do software e reduzir riscos de regressão.

---

## 👥 Equipe

- Gian Glauberty Santos Nascimento – 202300061616 (Eixo C)
- Pedro César Figueiredo Carneiro – 202300061732 (Eixo A)
- Pedro Miguel Castro França – 202300061741 (Eixo D)
- João Marcelo Silva da Conceição – 202300095820 (Slide)
- Renner do Nascimento Brito – 202300061797 (Conclusão)
- Paulo Gabriel de Oliveira Cardoso – 202000047735 (Eixo B)

---

## 📚 Tecnologias e Ferramentas Analisadas

- Rust
- GitHub
- GitHub Actions
- SQLite (FTS5)
- OCR
- Integração Contínua (CI/CD)

---

## 📄 Licença

Este repositório possui finalidade exclusivamente acadêmica e foi desenvolvido como parte das atividades da disciplina de Engenharia de Software.



## Resumo do Projeto

O trabalho analisou a estratégia de testes do **Screenpipe**, um sistema desktop multiplataforma que integra captura de tela/áudio, OCR, Accessibility API e processamento local com IA — um domínio tecnicamente complexo por depender fortemente de recursos nativos do sistema operacional.

**O que foi encontrado (Eixos A/B/C):**
- Uma estrutura de testes **multicamadas**: testes unitários em Rust (`#[cfg(test)]`), testes de integração, suíte E2E via Tauri/WebDriver (Windows, macOS, Linux) e uma checklist de **regressão manual** robusta em `TESTING.md`.
- Forte integração com CI/CD (GitHub Actions), com pipelines para lint, segurança (CodeQL, trufflehog) e cobertura (`cargo llvm-cov`), além de hooks de pré-commit via Lefthook para validações rápidas locais.
- Apesar dessa maturidade aparente, a análise identificou **lacunas reais**: a Issue #3274 mostrou que uma falha crítica na captura híbrida (OCR + Accessibility API) em apps como Zoom, Meet e Teams só gerou testes *depois* do bug reportado pela comunidade — evidência de que a proteção contra regressões é, em parte, reativa.
- Módulos de captura de tela e OCR concentram a maior dívida técnica (comentários "HACK", mecanismos de retry), sendo os pontos de maior risco de regressão.
- Dependências externas (OpenAI, Anthropic, Ollama, APIs nativas do SO) carecem de isolamento consistente via mocks/stubs, o que compromete a previsibilidade dos testes.
- Cobertura de código é monitorada, mas oscila bastante em fluxos de exceção (timeouts, falhas parciais de OCR, picos de CPU), justamente os cenários mais críticos em produção.

**Plano de evolução (Eixo D):**
A partir desses achados, foi proposto um plano de melhoria gradual, priorizando:
1. Testes de contrato para o pipeline de captura/OCR;
2. Testes de regressão para operações críticas do banco de dados;
3. Testes unitários para processamento de áudio;
4. Integração de gates de cobertura de código no CI;
5. Política obrigatória de "bug fix = teste automatizado associado".

## Conclusão

O Screenpipe demonstra uma **maturidade de testes acima da média** para um projeto de código aberto dessa complexidade: possui automação em múltiplas camadas, documentação de processo e integração contínua consolidada. No entanto, essa robustez estrutural convive com uma fragilidade de fundo — **grande parte da cobertura de cenários críticos foi construída reativamente**, após falhas reais reportadas pela comunidade, e não de forma preventiva.

O maior risco do projeto está concentrado exatamente onde está seu diferencial de produto: a captura híbrida multimodal (tela + áudio + OCR + IA local), que depende de APIs de sistema operacional difíceis de simular e testar de forma determinística. Isso torna a suíte atual capaz de pegar regressões pontuais, mas ainda **insuficiente para garantir, de forma proativa, a integridade de fluxos que envolvem múltiplos componentes simultaneamente**.

O plano de evolução proposto no Eixo D endereça diretamente esse gap ao priorizar testes de contrato, isolamento de dependências externas e políticas de qualidade vinculadas a correções — um caminho coerente para transformar uma estratégia de testes reativa em uma estratégia verdadeiramente preventiva, essencial para sustentar o crescimento do Screenpipe sem comprometer a confiabilidade do produto.
