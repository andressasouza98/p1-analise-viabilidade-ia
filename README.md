# P1 — Análise de Viabilidade para Uso de IA no Atendimento Educacional

> **Programa:** Capacitação em Sistemas Agentes — Instituto Hardware BR (HBr) / SOFTEX
> **Projeto:** P1 — Análise de viabilidade para um caso real (Competências C05–C09)
> **Autora:** Andressa Alves de Souza
> **Status:** ✅ Nível Básico concluído (`tag: p1-basico`)

---

## 1. Resumo do Caso

Uma instituição de ensino digital recebe milhares de mensagens mensais de estudantes sobre temas variados (matrícula, documentos, pagamentos, calendário acadêmico, revisão de notas, acesso ao ambiente virtual e solicitações administrativas). Diante da proposta de adotar Inteligência Artificial (LLMs) para classificar, priorizar, encaminhar, redigir rascunhos e automatizar respostas ou decisões, este projeto investiga tecnicamente **onde, como e sob quais condições a IA deve ou não ser utilizada**.

## 2. Objetivo do Projeto

Produzir uma recomendação técnica fundamentada e auditável (`MEMORANDO_TECNICO.md`) que oriente a direção da instituição sobre quais funcionalidades podem se beneficiar de LLMs, quais exigem supervisão humana (*human-in-the-loop*), quais devem ser resolvidas por mecanismos determinísticos convencionais e quais não devem ser implementadas nas condições atuais.

## 3. Estrutura dos Arquivos

```text
├── README.md              # Visão geral, estrutura e instruções de reprodução
├── MEMORANDO_TECNICO.md   # Artefato principal com análise, experimentos, riscos e recomendação
└── experimentos/          # Registros de prompts, casos de teste fictícios e resultados
```

## 4. Ferramentas Utilizadas

* **Ambiente de Experimentação de Prompts:** Google AI Studio — Modelo `gemini-3.5-flash-lite` (*Thinking level: Minimal*, ferramentas externas desativadas e 100% dados fictícios)
* **Versionamento:** Git e GitHub (com tags incrementais `p1-basico`, `p1-intermediario` e `p1-avancado`)

## 5. Instruções para Reproduzir os Testes

*(Os Prompts Protótipos V1 Baseline e os 6 cenários de teste executados no Google AI Studio estão documentados na Seção 4 do `MEMORANDO_TECNICO.md`; o roteiro expandido será detalhado na etapa `p1-intermediario`).*

## 6. Artefatos e Links da Entrega

* **Memorando Técnico:** [MEMORANDO_TECNICO.md](./MEMORANDO_TECNICO.md)
* **Vídeo de Apresentação (YouTube - Não Listado):** *Aguardando conclusão da etapa Avançado*
* **Marcos (Tags/Releases):**
  * `p1-basico` *(concluído)*
  * `p1-intermediario` *(pendente)*
  * `p1-avancado` *(pendente)*
