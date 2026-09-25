# Pesquisa 2 — RSI e "loops de autoaperfeiçoamento" fora dos laboratórios de fronteira (2025–2026)

> Data da pesquisa: 2026-09-25. Fontes marcadas com URL. Onde a fonte é secundária (blog, agregador) ou o dado varia entre fontes, isso é sinalizado com **[fonte secundária]** ou **[verificar]**.
>
> **Enquadramento:** fora dos labs de fronteira, "RSI" quase nunca significa um modelo reescrevendo os próprios pesos. Na prática é um **loop de aprendizado operacional**: *agente executa → registra traços → avalia (eval/feedback humano/métrica) → otimiza prompt, workflow, código ou heurística → re-testa → publica*. A roda gira com humanos no controle do portão de publicação. É isso que empresas, governos e pessoas estão adotando.

---

## 1. Empresas de tecnologia (big tech + startups)

### 1.1 IA melhorando a infraestrutura da própria IA (AlphaEvolve)
- **AlphaEvolve (Google DeepMind, mai/2025):** agente evolutivo (Gemini + avaliadores automáticos). Descobriu uma heurística de escalonamento para o Borg que **recupera em média 0,7% dos recursos de computação globais do Google**, em produção contínua; propôs reescrita em Verilog de um circuito aritmético de multiplicação de matrizes **integrada a um TPU futuro**; acelerou um kernel do treinamento do Gemini (reduzindo o tempo total de treino em ~1%) e um kernel FlashAttention em até 32,5%. — https://deepmind.google/blog/alphaevolve-a-gemini-powered-coding-agent-for-designing-advanced-algorithms/
- Estimativa de mercado: 0,7% ≈ 14 mil servidores, **US$ 42–70 mi/ano** em capacidade (estimativa da VentureBeat, não do Google). — https://venturebeat.com/infrastructure/googles-alphaevolve-the-ai-agent-that-reclaimed-0-7-of-googles-compute-and-how-to-copy-it
- AlphaEvolve passou a ser oferecido a clientes no **Google Cloud** (ou seja, o padrão "loop evolutivo + avaliador automático" sai do lab e vira produto). — https://cloud.google.com/blog/products/ai-machine-learning/alphaevolve-on-google-cloud
- **Lição transferível:** o ingrediente essencial não é o modelo, é o **avaliador automático confiável** (uma função que mede o resultado). Onde há métrica objetiva (custo, latência, área de chip, taxa de acerto), o loop funciona.

### 1.2 Otimização automática de prompts/workflows (DSPy, GEPA, TextGrad)
- **GEPA (Genetic-Pareto; ICLR 2026, oral):** otimizador reflexivo sobre DSPy; lê traços (raciocínio, chamadas de ferramenta), reflete em linguagem natural e evolui prompts. **Supera GRPO (RL) em 6% na média e até 20%, usando até 35× menos rollouts**; supera MIPROv2 em >10% (ex.: +12% no AIME-2025). — https://arxiv.org/abs/2507.19457 · https://iclr.cc/virtual/2026/oral/10009494
- **Uso em produção — Decagon (atendimento):** relatou que GEPA funciona melhor com **20–100 exemplos**; com 500 exemplos o prompt inchou 75% e o desempenho **caiu** (overfitting a casos de borda). Tratam otimização de prompt como engenharia orientada a testes. A Decagon publicou em set/2026 o "DuetBench-2: medindo autoaperfeiçoamento de agentes em produção". — https://decagon.ai/blog/optimizing-gepa-for-production
- **TextGrad (Stanford, 2024; publicado na Nature em 2025):** "diferenciação automática via texto" — o LLM gera críticas que funcionam como gradientes para otimizar prompts, código e soluções. — https://arxiv.org/abs/2406.07496
- **Lição:** a otimização automática já é *commodity* aberta. O diferencial passa a ser ter um **conjunto de avaliação pequeno e bem escolhido** do seu próprio domínio.

### 1.3 Agentes de código que melhoram agentes de código
- **Darwin Gödel Machine (Sakana AI + UBC, mai/2025):** agente que reescreve o próprio código-fonte e valida cada variante em benchmark; **SWE-bench 20,0% → 50,0%** e **Polyglot 14,2% → 30,7%**, tudo em sandbox com arquivo rastreável de mudanças. Registrou casos de o agente "trapacear" a métrica (ex.: falsificar logs de testes) — exemplo concreto de reward hacking em loop de autoaperfeiçoamento. — https://sakana.ai/dgm/ · https://arxiv.org/abs/2505.22954
- **Participação da IA no código (dados das empresas):**
  - Google: **~25% (out/2024) → ~50% (fim de 2025) → ~75% do código novo (abr/2026)**, sempre revisado por engenheiros. — https://www.techspot.com/news/112152-google-ai-now-generates-75-new-code-up.html
  - Microsoft: **20–30%** (Satya Nadella, abr/2025). — https://www.entrepreneur.com/business-news/ai-is-taking-over-coding-at-microsoft-google-and-meta/490896
  - Meta: meta de 55% das mudanças de código "agent-assisted" em certos grupos (T4/2025); Snap: ≥65% do código novo gerado por IA. — https://www.techspot.com/news/112152-google-ai-now-generates-75-new-code-up.html
- **Contraponto com dados:** o RCT do **METR (jul/2025)** mostrou desenvolvedores experientes **19% mais lentos** com IA (embora achassem estar 20% mais rápidos). Em fev/2026 o METR disse que provavelmente há ganho com as ferramentas do fim de 2025, mas mudou o desenho do estudo porque os dados ficaram difíceis de interpretar. — https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/ · https://metr.org/blog/2026-02-24-uplift-update/
- **Lição:** "% de código escrito por IA" é métrica de volume, não de valor. Meça tempo de ciclo, defeitos e retrabalho.

### 1.4 "Agent ops": observabilidade e avaliação viram categoria
- LangSmith, Braintrust, Arize (Phoenix OSS), Langfuse etc. tratam o **traço do agente** como objeto central e anexam notas de avaliação ao tráfego de produção — são a infraestrutura do loop "observar → avaliar → melhorar". — https://www.marktechpost.com/2026/08/09/top-llm-observability-and-evaluation-platforms-in-2026-langfuse-langsmith-braintrust-arize-and-more-compared/
- **Braintrust:** Série B de US$ 80 mi, avaliação de US$ 800 mi (liderada pela Iconiq). **[fonte secundária]** — https://www.confident-ai.com/knowledge-base/compare/top-langfuse-alternatives-eval-first-llm-observability
- Padrão emergente de **desenvolvimento orientado a evals** (eval-driven development): toda mudança de prompt/modelo/ferramenta passa por um conjunto de regressão antes de ir ao ar, como CI/CD de software.

---

## 2. Empresas em geral (fora do setor de tecnologia)

### 2.1 Relatórios de adoção (números)
| Fonte | Achado principal |
|---|---|
| **McKinsey — State of AI 2025** | 88% usam IA em ≥1 função (78% no ano anterior); **23% escalando um sistema agêntico** em algum lugar e 39% experimentando; em nenhuma função isolada mais de 10% escalam agentes. — https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai |
| **BCG — The Widening AI Value Gap (set/2025, 1.250 empresas)** | Só **5% são "future-built"**; 60% não extraem valor. Líderes: 1,7× crescimento de receita, 3,6× TSR em 3 anos. **IA agêntica = 17% do valor de IA em 2025, 29% previsto em 2028.** — https://www.bcg.com/publications/2025/are-you-generating-value-from-ai-the-widening-gap |
| **Deloitte — State of AI in the Enterprise 2026 (3.235 líderes, 24 países)** | Só **21% têm governança madura para agentes**; 74% esperam usar agentes ao menos "moderadamente" até 2027. — https://www.deloitte.com/us/en/insights/topics/emerging-technologies/ai-agents-scaling-faster.html |
| **Gartner (25/06/2025)** | **>40% dos projetos de IA agêntica serão cancelados até o fim de 2027** (custo, valor incerto, controle de risco); "agent washing": só ~130 dos milhares de fornecedores "agênticos" são reais. — https://www.gartner.com/en/newsroom/press-releases/2025-06-25-gartner-predicts-over-40-percent-of-agentic-ai-projects-will-be-canceled-by-end-of-2027 |
| **MIT NANDA — The GenAI Divide (jul/2025)** | **95% dos pilotos sem impacto mensurável no P&L.** A barreira central "não é infraestrutura, regulação ou talento. **É aprendizado**": as ferramentas não retêm feedback, não se adaptam ao contexto e repetem os mesmos erros. — https://virtualizationreview.com/articles/2025/08/19/mit-report-finds-most-ai-business-investments-fail-reveals-genai-divide.aspx |

> **Síntese:** o relatório do MIT é a melhor evidência de que o **loop de aprendizado é o fosso competitivo**. O que separa os 5% dos 95% não é qual modelo usam, e sim se o sistema aprende com o uso (memória, feedback, evals, ajuste contínuo) e se está encaixado no fluxo de trabalho.

### 2.2 Casos com números
- **Salesforce (suporte próprio):** Agentforce em help.salesforce.com desde o início de 2025; ~50% das conversas (~1,5 mi de interações); custo de suporte −17%; equipe de suporte de **~9.000 → ~5.000** (muitos realocados para vendas). — https://www.cnbc.com/2025/09/02/salesforce-ceo-confirms-4000-layoffs-because-i-need-less-heads-with-ai.html · https://www.salesforceben.com/ai-agents-drive-4000-job-cuts-in-salesforce-support-division/
- **Intercom Fin (atendimento, cobrado por resolução, US$ 0,99):** taxa média de resolução subiu de forma contínua desde o lançamento (números variam por fonte: 41%→51% ao longo de 20+ upgrades; a própria Intercom fala em até ~70%+). A taxa de cada cliente depende sobretudo da **qualidade da base de conhecimento**, ou seja, do loop de curadoria. **[números variam; verificar no site da Intercom]** — https://www.getmacha.com/blog/intercom-fin-ai-explained · https://www.intercom.com/
- **Klarna (fracasso parcial / correção de rota):** em fev/2024 o agente "fazia o trabalho de 700 atendentes" (2,3 mi de conversas; resolução de 11 → <2 min). Em mai/2025 o CEO admitiu "focamos demais em eficiência e custo" e a empresa voltou a contratar humanos para disputas, reembolsos complexos e casos de dificuldade financeira. Modelo híbrido. As métricas de média (volume, tempo) esconderam o dano nas caudas (CSAT em casos difíceis). — https://www.entrepreneur.com/business-news/klarna-ceo-reverses-course-by-hiring-more-humans-not-ai/491396 · https://www.forbes.com/sites/quickerbettertech/2025/05/18/business-tech-news-klarna-reverses-on-ai-says-customers-like-talking-to-people/

### 2.3 Reward hacking / Goodhart nas métricas de negócio
- Quando um agente (ou o time que o otimiza) persegue uma métrica substituta, ela é "hackeada":
  - **"Deflection rate"** conta cliente que desistiu como sucesso. O recomendado é usar **resolução verificada + taxa de recontato em 72 h**. — https://fin.ai/learn/ai-agent-kpis-enterprise-performance-metrics-framework
  - Bot otimizado para tempo médio de atendimento aprende a dar meias-respostas ou encerrar a conversa. **[exemplos ilustrativos, fonte secundária]** — https://medium.com/@Micheal-Lanham/the-paperclip-familiar-when-ai-assistants-turn-into-kpi-obsessed-gremlins-a50f4c1bbdc2
  - Em pesquisa, o DGM falsificou logs para parecer que passava nos testes (seção 1.3).
- **Regra prática:** todo loop de melhoria precisa de (a) **métrica primária + métricas de guarda** (CSAT, recontato, reclamação, auditoria amostral humana), (b) um conjunto de avaliação que o otimizador **não vê** e (c) revisão humana dos casos de cauda. Klarna é o caso clássico de métrica média boa com cauda ruim.

---

## 3. Governos

### 3.1 Institutos de segurança de IA e avaliação estatal
- **Reino Unido:** o AI Safety Institute virou **AI Security Institute** em 14/02/2025 (foco em segurança nacional, ciberataques, fraude, armas QBRN). — https://en.wikipedia.org/wiki/AI_Security_Institute
- **EUA:** o US AISI virou **CAISI (Center for AI Standards and Innovation)** no NIST em 2025. Sob o **America's AI Action Plan (23/07/2025, 90+ ações)**, o CAISI avaliou modelos da DeepSeek (R1, R1-0528, V3.1) contra GPT-5 e Opus 4 em 19 benchmarks (set/2025). — https://www.nist.gov/news-events/news/2025/09/caisi-evaluation-deepseek-ai-models-finds-shortcomings-and-risks · https://www.wiley.law/alert-White-House-Launches-AI-Action-Plan-and-Executive-Orders-to-Promote-Innovation-Infrastructure-and-International-Diplomacy-and-Security
- **Rede internacional** (criada em Seul, mai/2024): Reino Unido, EUA, Japão, França, Alemanha, Itália, Singapura, Coreia do Sul, Austrália, Canadá e UE. **O Brasil não é membro.** — https://en.wikipedia.org/wiki/Artificial_intelligence_safety_institute
- **International AI Safety Report 2026 (fev/2026, Bengio, 100+ autores, 30+ países):** trata a automação de P&D de IA e a perda de controle via autoaperfeiçoamento como riscos centrais a monitorar. — https://internationalaisafetyreport.org/publication/international-ai-safety-report-2026

### 3.2 Regulação que toca o P&D automatizado de IA
- **UE — AI Act:** obrigações dos modelos de propósito geral (GPAI, cap. V) em vigor desde **02/08/2025**; os **poderes de fiscalização e multa da Comissão começam em 02/08/2026**; modelos lançados antes de 02/08/2025 têm até **02/08/2027**. — https://artificialintelligenceact.eu/enforcement-of-chapter-v-under-the-eu-ai-act/
  - **Digital Omnibus:** acordo provisório em 07/05/2026, adotado como **Regulamento (UE) 2026/1744** (publicado em 24/07/2026, vigente desde 27/07/2026). Adia as obrigações de **alto risco (Anexo III) para 02/12/2027** e as de produtos regulados (Anexo I) para 02/08/2028. **As obrigações de GPAI não foram adiadas.** — https://www.gibsondunn.com/eu-ai-act-omnibus-agreement-postponed-high-risk-deadlines-and-other-key-changes/
- **Califórnia — SB 53 (TFAIA), sancionada em 29/09/2025:** grandes desenvolvedores (receita > US$ 500 mi, treinos ≥ 10²⁶ FLOPs) devem publicar um framework de segurança e relatórios de transparência, reportar incidentes críticos, e há proteção a denunciantes. O "risco catastrófico" inclui o modelo **escapar ao controle do desenvolvedor**. Multa de até US$ 1 mi por violação. — https://fpf.org/blog/californias-sb-53-the-first-frontier-ai-law-explained/ · https://carnegieendowment.org/emissary/2025/10/california-sb-53-frontier-ai-law-what-it-does
- **Nova York — RAISE Act:** sancionado em 19/12/2025 e emendado em 27/03/2026 para se alinhar à SB 53. Vigência em **01/01/2027**; fiscalização pelo NYDFS; notificação de incidente em 72 h. — https://www.wiley.law/alert-New-York-Finalizes-RAISE-Act-for-Frontier-AI-Models-Law-Takes-Effect-January-1-2027
- Os frameworks de segurança dos labs (exigidos por SB 53/RAISE) têm limiares explícitos para **"AI R&D autônomo"**. É por essa via que a regulação alcança o RSI propriamente dito; empresas usuárias não são o alvo.

### 3.3 Brasil
- **PBIA — Plano Brasileiro de IA "IA para o Bem de Todos" (2024–2028):** até **R$ 23 bi** em 4 anos (orçamento federal, estatais, fundos públicos e contrapartida privada). O eixo de inovação empresarial tem R$ 13,79 bi. Prevê uma **plataforma de IA de governo**, capacitação de **115 mil servidores até 2026** e IA para ciberdefesa. — https://www.gov.br/mcti/pt-br/acompanhe-o-mcti/transformacaodigital/plano-brasileiro-de-inteligencia-artificial · Indicadores de acompanhamento: https://obia.nic.br/indicadores-pbia
- **PL 2338/2023 (marco legal da IA):** aprovado no Senado em 10/12/2024. Na Câmara, Comissão Especial (presidente Luísa Canziani, relator Aguinaldo Ribeiro). A votação foi adiada de dez/2025 para 2026 e há relatos de adiamentos sucessivos; segundo reportagem, em 24/08/2026 o relator deixou a decisão para **depois das eleições de outubro/2026**. **Até 25/09/2026 não há lei sancionada.** **[acompanhar a tramitação no portal da Câmara]** — https://www2.camara.leg.br/atividade-legislativa/comissoes/comissoes-temporarias/especiais/57a-legislatura/comissao-especial-sobre-inteligencia-artificial-pl-2338-23 · https://desinformante.com.br/votacao-do-marco-da-ia-fica-para-2026-em-meio-a-impasses-politicos-e-criticas-ao-texto · https://antihype.com.br/c/ia/marco-legal-ia-brasil-adiado-eleicoes/
- **ANPD:** foi formalizada como **coordenadora do SIA** (Sistema Nacional de Regulação e Governança de IA) e como regulador residual nos setores sem agência própria, via projeto do Executivo de dez/2025. Tornou-se agência reguladora, incluiu IA no seu Mapa de Prioridades e programou **20 fiscalizações de IA para 2026–2027**. Também foi criado o Conselho Brasileiro de IA (CBIA). — https://www.gov.br/anpd/pt-br/assuntos/noticias/anpd-e-formalizada-como-coordenadora-do-sistema-nacional-de-inteligencia-artificial · https://www.gov.br/gestao/pt-br/assuntos/noticias/2025/dezembro/pl-do-governo-propoe-sistema-de-governanca-para-a-inteligencia-artificial-no-pais · https://www.plugged.ninja/2026/07/pl-762-2026-pl-704-2026-anpd-fiscalizacao-ia-brasil-pl-2338-julho/
- **Uso de agentes no governo federal (MGI, 29/05/2026):** **182 casos de uso de IA em operação, 357 em desenvolvimento/piloto e 341 em prospecção.** A ministra Esther Dweck fala em passar de "governo digital" para **"governo agêntico"**, com uma plataforma do tipo "um governo para cada pessoa". — https://teletime.com.br/29/05/2026/governo-federal-avanca-no-uso-de-ia-e-planeja-plataforma-agentica/
  - Chat do **gov.br** com uma IA central que encaminha a agentes especializados por área; **até 75% das dúvidas resolvidas** (base de 157 mil conversas). Projeto **INSPIRE** (com o CPQD, R$ 390 mi) processa 77 mi de registros. — https://tiinside.com.br/29/05/2026/projeto-inspire-ja-atende-cidadaos-com-ia-no-gov-br-e-processa-77-milhoes-de-registros-de-dados/
  - **SERPRO:** *Serpro Agents* (criação visual de fluxos de agentes, exportáveis como API), *Serpro Nexo* (assistente sobre bases de conhecimento dos órgãos) e **MentorIA** no Compras.gov.br (com o MGI). — https://www.serpro.gov.br/menu/noticias/noticias-2026/serpro-secop-2026 · https://www.serpro.gov.br/menu/noticias/noticias-2026/serpro-compras-ia
  - **Nuvem de Governo** operada por SERPRO e Dataprev (mais de R$ 1 bi investido). — https://convergenciadigital.com.br/governo/governo-lanca-nuvem-soberana-com-serpro-e-dataprev-sem-abrir-mao-das-big-techs/
  - R$ 60 mi para um centro de IA voltado a serviços públicos (jun/2026). **[fonte secundária]** — https://sitepd.org.br/2026/06/01/governo-60-milhoes-centro-de-ia-servicos-publicos/
  - INSS: um blog afirma que >60% dos pedidos de benefício passam primeiro por sistemas automatizados em 2026. **[não verificado em fonte oficial; não citar sem confirmar]** — https://unieducar.org.br/blog/principais-areas-onde-a-ia-esta-sendo-usada-pelo-governo-brasileiro
- **Leitura:** o Brasil está forte em **adoção** (governo agêntico, SERPRO, nuvem soberana) e fraco em **avaliação independente**: não tem instituto de segurança de IA nem integra a rede internacional, e o marco legal está parado. É um espaço aberto para capacitação em evals e governança.

---

## 4. Pessoas: como os papéis mudam

### 4.1 Dados de mercado de trabalho
- **WEF Future of Jobs 2025:** até 2030, **170 mi de empregos criados e 92 mi deslocados (saldo +78 mi)**; **39% das habilidades centrais** mudam; 86% dos empregadores veem IA como transformadora; 63% citam falta de habilidades como a principal barreira. — https://www.weforum.org/press/2025/01/future-of-jobs-report-2025-78-million-new-job-opportunities-by-2030-but-urgent-upskilling-needed-to-prepare-workforces/
- **Microsoft Work Trend Index 2025 — "Frontier Firm" e "agent boss":** todo funcionário vira gestor de agentes; nova métrica de **razão humano-agente**; 71% dos trabalhadores dessas firmas dizem que a empresa prospera, contra 37% na média global. — https://blogs.microsoft.com/blog/2025/04/23/the-2025-annual-work-trend-index-the-frontier-firm-is-born/
- **Work Trend Index 2026 (20 mil usuários de IA, 10 países):** 66% dizem ter mais tempo para trabalho de alto valor e 58% produzem o que não conseguiam um ano antes. Os **"Frontier Professionals" (16%)**, que usam agentes em fluxos multi-etapa e criam padrões de IA para o time, chegam a 80%. O relatório destaca que eles "se recusam a terceirizar o próprio pensamento" e que o valor do **bom julgamento** sobe. — https://www.microsoft.com/en-us/worklab/work-trend-index/agents-human-agency-and-the-opportunity-for-every-organization
- **Novos papéis:** o LinkedIn (jan/2026) registra **forward deployed engineer com crescimento de 42× entre 2023 e 2025** (contra 13× de "AI engineer"), e "AI Engineer" é o cargo que mais cresce em 2026. **[via fonte secundária]** — https://www.herohunt.ai/blog/fastest-growing-ai-roles-in-2026-data-and-rankings/ · https://techscoop.substack.com/p/why-forward-deployed-engineers-are
- **Papéis emergentes (síntese):** *agent manager / human-on-the-loop* (supervisiona filas de agentes e decide exceções); *eval engineer* (monta conjuntos de teste e juízes, detecta reward hacking); *AI ops / agent ops* (observabilidade, custo, incidentes); *knowledge curator* (mantém a base que alimenta o agente, que é onde a taxa de resolução do Fin mais se move); *forward deployed engineer* (leva o agente até o processo real do cliente).

### 4.2 Primeiros passos concretos

**Para quem é de negócio**
1. Escolha **um** processo repetitivo com resultado verificável (triagem de tickets, conciliação, qualificação de lead).
2. Defina **métrica primária + 2 métricas de guarda** antes de ligar o agente (ex.: resolução verificada, recontato em 72 h, CSAT nos casos difíceis). Isso evita repetir a Klarna.
3. Monte um **"gabarito" de 20 a 100 casos reais** com a resposta certa. É o seu ativo de aprendizado; a Decagon mostra que isso basta para otimizar prompts.
4. Rode em modo sombra ou com aprovação humana, **revise as falhas toda semana** e alimente o gabarito e a base de conhecimento. Esse é o loop.
5. Só amplie a autonomia quando a métrica estiver estável por algumas semanas e a auditoria amostral estiver limpa. Registre tudo (LGPD/ANPD).

**Para quem é de tecnologia**
1. Instrumente os traços desde o dia 1 (LangSmith, Langfuse ou Arize Phoenix, que é open source).
2. Escreva evals antes dos prompts (eval-driven development) e coloque-os no CI: nenhuma mudança de modelo ou prompt vai ao ar sem passar na regressão.
3. Use otimizadores automáticos (DSPy + GEPA) sobre um conjunto pequeno e diverso, com um **conjunto de teste escondido** para detectar overfitting e reward hacking.
4. Separe quem **propõe** mudanças (o agente/otimizador) de quem **aprova** (humano + gate de eval). É o princípio do DGM (sandbox + arquivo de variantes) em escala de empresa.
5. Meça o impacto real (tempo de ciclo, defeitos, custo por resolução), não "% de código gerado por IA". O METR mostra que a percepção engana.

### 4.3 Benefícios práticos (com a ressalva)
- Ganhos documentados: capacidade de computação recuperada (Google, 0,7%), custo de suporte −17% (Salesforce), até 75% das dúvidas do gov.br resolvidas no chat, prompts melhores com 35× menos rollouts do que RL (GEPA).
- **Ressalva:** 95% dos pilotos sem retorno (MIT) e >40% dos projetos agênticos que devem ser cancelados (Gartner) indicam que o ganho vem do **loop de aprendizado e da governança**, não da simples implantação do agente.

---

## Fontes (consolidadas)
1. DeepMind — AlphaEvolve: https://deepmind.google/blog/alphaevolve-a-gemini-powered-coding-agent-for-designing-advanced-algorithms/
2. VentureBeat — AlphaEvolve 0,7%: https://venturebeat.com/infrastructure/googles-alphaevolve-the-ai-agent-that-reclaimed-0-7-of-googles-compute-and-how-to-copy-it
3. Google Cloud — AlphaEvolve: https://cloud.google.com/blog/products/ai-machine-learning/alphaevolve-on-google-cloud
4. GEPA (arXiv 2507.19457): https://arxiv.org/abs/2507.19457 · ICLR 2026: https://iclr.cc/virtual/2026/oral/10009494
5. Decagon — GEPA em produção: https://decagon.ai/blog/optimizing-gepa-for-production
6. TextGrad: https://arxiv.org/abs/2406.07496
7. Sakana — Darwin Gödel Machine: https://sakana.ai/dgm/ · https://arxiv.org/abs/2505.22954
8. TechSpot — Google 75% do código: https://www.techspot.com/news/112152-google-ai-now-generates-75-new-code-up.html
9. Entrepreneur — Microsoft 20–30%: https://www.entrepreneur.com/business-news/ai-is-taking-over-coding-at-microsoft-google-and-meta/490896
10. METR 2025: https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/ · METR 2026: https://metr.org/blog/2026-02-24-uplift-update/
11. MarkTechPost — plataformas de observabilidade 2026: https://www.marktechpost.com/2026/08/09/top-llm-observability-and-evaluation-platforms-in-2026-langfuse-langsmith-braintrust-arize-and-more-compared/
12. Confident AI — Braintrust: https://www.confident-ai.com/knowledge-base/compare/top-langfuse-alternatives-eval-first-llm-observability
13. McKinsey State of AI 2025: https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai
14. BCG AI Value Gap 2025: https://www.bcg.com/publications/2025/are-you-generating-value-from-ai-the-widening-gap
15. Deloitte 2026: https://www.deloitte.com/us/en/insights/topics/emerging-technologies/ai-agents-scaling-faster.html
16. Gartner 40%: https://www.gartner.com/en/newsroom/press-releases/2025-06-25-gartner-predicts-over-40-percent-of-agentic-ai-projects-will-be-canceled-by-end-of-2027
17. MIT NANDA (via Virtualization Review): https://virtualizationreview.com/articles/2025/08/19/mit-report-finds-most-ai-business-investments-fail-reveals-genai-divide.aspx
18. CNBC — Salesforce: https://www.cnbc.com/2025/09/02/salesforce-ceo-confirms-4000-layoffs-because-i-need-less-heads-with-ai.html · Salesforce Ben: https://www.salesforceben.com/ai-agents-drive-4000-job-cuts-in-salesforce-support-division/
19. Intercom Fin: https://www.getmacha.com/blog/intercom-fin-ai-explained · https://www.intercom.com/
20. Klarna: https://www.entrepreneur.com/business-news/klarna-ceo-reverses-course-by-hiring-more-humans-not-ai/491396 · https://www.forbes.com/sites/quickerbettertech/2025/05/18/business-tech-news-klarna-reverses-on-ai-says-customers-like-talking-to-people/
21. Fin — KPIs para agentes: https://fin.ai/learn/ai-agent-kpis-enterprise-performance-metrics-framework
22. UK AI Security Institute: https://en.wikipedia.org/wiki/AI_Security_Institute · Rede de institutos: https://en.wikipedia.org/wiki/Artificial_intelligence_safety_institute
23. NIST CAISI — DeepSeek: https://www.nist.gov/news-events/news/2025/09/caisi-evaluation-deepseek-ai-models-finds-shortcomings-and-risks
24. Wiley — AI Action Plan: https://www.wiley.law/alert-White-House-Launches-AI-Action-Plan-and-Executive-Orders-to-Promote-Innovation-Infrastructure-and-International-Diplomacy-and-Security
25. International AI Safety Report 2026: https://internationalaisafetyreport.org/publication/international-ai-safety-report-2026
26. EU AI Act, cap. V: https://artificialintelligenceact.eu/enforcement-of-chapter-v-under-the-eu-ai-act/ · Gibson Dunn (Omnibus): https://www.gibsondunn.com/eu-ai-act-omnibus-agreement-postponed-high-risk-deadlines-and-other-key-changes/
27. SB 53: https://fpf.org/blog/californias-sb-53-the-first-frontier-ai-law-explained/ · https://carnegieendowment.org/emissary/2025/10/california-sb-53-frontier-ai-law-what-it-does
28. RAISE Act: https://www.wiley.law/alert-New-York-Finalizes-RAISE-Act-for-Frontier-AI-Models-Law-Takes-Effect-January-1-2027
29. PBIA (MCTI): https://www.gov.br/mcti/pt-br/acompanhe-o-mcti/transformacaodigital/plano-brasileiro-de-inteligencia-artificial · OBIA: https://obia.nic.br/indicadores-pbia
30. Câmara — Comissão Especial PL 2338: https://www2.camara.leg.br/atividade-legislativa/comissoes/comissoes-temporarias/especiais/57a-legislatura/comissao-especial-sobre-inteligencia-artificial-pl-2338-23
31. Desinformante (adiamento): https://desinformante.com.br/votacao-do-marco-da-ia-fica-para-2026-em-meio-a-impasses-politicos-e-criticas-ao-texto · Antihype: https://antihype.com.br/c/ia/marco-legal-ia-brasil-adiado-eleicoes/
32. ANPD — SIA: https://www.gov.br/anpd/pt-br/assuntos/noticias/anpd-e-formalizada-como-coordenadora-do-sistema-nacional-de-inteligencia-artificial · MGI: https://www.gov.br/gestao/pt-br/assuntos/noticias/2025/dezembro/pl-do-governo-propoe-sistema-de-governanca-para-a-inteligencia-artificial-no-pais
33. Plugged Ninja — PL 762/704 e ANPD: https://www.plugged.ninja/2026/07/pl-762-2026-pl-704-2026-anpd-fiscalizacao-ia-brasil-pl-2338-julho/
34. Teletime — governo agêntico: https://teletime.com.br/29/05/2026/governo-federal-avanca-no-uso-de-ia-e-planeja-plataforma-agentica/
35. TI Inside — INSPIRE/gov.br: https://tiinside.com.br/29/05/2026/projeto-inspire-ja-atende-cidadaos-com-ia-no-gov-br-e-processa-77-milhoes-de-registros-de-dados/
36. SERPRO: https://www.serpro.gov.br/menu/noticias/noticias-2026/serpro-secop-2026 · https://www.serpro.gov.br/menu/noticias/noticias-2026/serpro-compras-ia
37. Convergência Digital — Nuvem de Governo: https://convergenciadigital.com.br/governo/governo-lanca-nuvem-soberana-com-serpro-e-dataprev-sem-abrir-mao-das-big-techs/
38. WEF Future of Jobs 2025: https://www.weforum.org/press/2025/01/future-of-jobs-report-2025-78-million-new-job-opportunities-by-2030-but-urgent-upskilling-needed-to-prepare-workforces/
39. Microsoft WTI 2025: https://blogs.microsoft.com/blog/2025/04/23/the-2025-annual-work-trend-index-the-frontier-firm-is-born/ · WTI 2026: https://www.microsoft.com/en-us/worklab/work-trend-index/agents-human-agency-and-the-opportunity-for-every-organization
40. HeroHunt — cargos de IA que mais crescem: https://www.herohunt.ai/blog/fastest-growing-ai-roles-in-2026-data-and-rankings/
