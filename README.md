# P1 — Análise de Viabilidade para Uso de IA no Atendimento Educacional

> **Programa:** Capacitação em Sistemas Agentes — Instituto Hardware BR (HBr) / SOFTEX  
> **Projeto:** P1 — Análise de viabilidade para um caso real (Competências C05–C09)  
> **Autora:** Andressa Alves de Souza  
> **Status:** ✅ Nível Intermediário concluído (`tag: p1-intermediario`)  

---

## 1. Resumo do Caso

Uma instituição de ensino digital recebe milhares de mensagens mensais de estudantes sobre temas variados (matrícula, documentos, pagamentos, calendário acadêmico, revisão de notas, acesso ao ambiente virtual e solicitações administrativas). Diante da proposta de adotar Inteligência Artificial (LLMs) para classificar, priorizar, encaminhar, redigir rascunhos e automatizar respostas ou decisões, este projeto investiga tecnicamente **onde, como e sob quais condições a IA deve ou não ser utilizada**.

## 2. Objetivo do Projeto

Produzir uma recomendação técnica fundamentada e auditável (`MEMORANDO_TECNICO.md`) que oriente a direção da instituição sobre quais funcionalidades podem se beneficiar de LLMs, quais exigem supervisão humana (*human-in-the-loop*), quais devem ser resolvidas por mecanismos determinísticos convencionais e quais não devem ser implementadas nas condições atuais.

## 3. Estrutura dos Arquivos

```text
├── README.md                          # Visão geral, estrutura, métricas e instruções de reprodução
├── MEMORANDO_TECNICO.md               # Artefato principal com análise, experimentos, riscos e recomendação
└── experimentos/
    ├── DATASET_TESTES.md              # Suíte padronizada de 10 casos de teste fictícios (CT-01 a CT-10)
    └── DIARIO_EXPERIMENTOS.md         # Evolução iterativa (V1–V3), matrizes de auditoria e análise de erros
```

## 4. Ferramentas Utilizadas

* **Ambiente de Experimentação de Prompts:** Google AI Studio — Modelo `gemini-3.5-flash-lite` (*Thinking level: Minimal*, ferramentas externas desativadas e 100% dados fictícios)
* **Versionamento:** Git e GitHub (com tags incrementais `p1-basico`, `p1-intermediario` e `p1-avancado`)

## 5. Resultados Consolidados e Reprodução dos Testes

Os experimentos completos — incluindo os prompts das versões **V1, V2 e V3**, os **10 casos de teste (`CT-01` a `CT-10`)** e as matrizes de avaliação nas quatro dimensões (`D1_FID`, `D2_COM`, `D3_SIN`, `D4_ESC`) — estão documentados em [`experimentos/DIARIO_EXPERIMENTOS.md`](./experimentos/DIARIO_EXPERIMENTOS.md) e [`experimentos/DATASET_TESTES.md`](./experimentos/DATASET_TESTES.md).

| Funcionalidade | Versão Final | Taxa de Aprovação Integral (`1/1/1/1`) | Evolução Empírica |
| :--- | :---: | :---: | :--- |
| **F1 — Triagem e Classificação** | **V3** | **90%** *(100% com schema/normalização)* | V1 (40%) $\rightarrow$ V2 (60%) $\rightarrow$ V3 (90% em prompt puro; 100% com `enum`/sanitização de acentos) |
| **F2 — Copiloto de Resposta** | **V2** | **100%** | V1 (70%) $\rightarrow$ V2 (100% de fidelidade auditada sem alucinações) |

### Como Reproduzir no Google AI Studio

1. Abra o **Google AI Studio** e selecione o modelo `gemini-3.5-flash-lite` com `Thinking level: Minimal` e todas as ferramentas (*Tools*) desativadas.
2. Copie as *System instructions* da versão desejada (F1-V3 ou F2-V2) disponíveis em [`experimentos/DIARIO_EXPERIMENTOS.md`](./experimentos/DIARIO_EXPERIMENTOS.md).
3. Execute individualmente os 10 cenários descritos em [`experimentos/DATASET_TESTES.md`](./experimentos/DATASET_TESTES.md) em sessões limpas (*New chat*).

## 6. Artefatos e Links da Entrega

* **Memorando Técnico:** [MEMORANDO_TECNICO.md](./MEMORANDO_TECNICO.md)
* **Dataset de Testes Fictícios:** [experimentos/DATASET_TESTES.md](./experimentos/DATASET_TESTES.md)
* **Diário de Experimentos (V1–V3):** [experimentos/DIARIO_EXPERIMENTOS.md](./experimentos/DIARIO_EXPERIMENTOS.md)
* **Vídeo de Apresentação (YouTube - Não Listado):** *Aguardando conclusão da etapa Avançado*
* **Marcos (Tags/Releases):**
  * `p1-basico` *(concluído)*
  * `p1-intermediario` *(concluído)*
  * `p1-avancado` *(pendente)*
