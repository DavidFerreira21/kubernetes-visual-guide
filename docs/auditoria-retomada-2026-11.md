# Retomada e homologação pedagógica — novembro de 2026

[⌂ Início](../README.md) · [Camada 01](../trilhas/01-fundamentos/README.md) · [Matriz dos labs](matriz-homologacao.md)

## Objetivo

Homologar a trilha sob a perspectiva de um estudante iniciante: preparar o ambiente, entender o objetivo, escrever YAML sem copiar a solução, aplicar, observar, experimentar, diagnosticar e limpar. **Não considerar sucesso da CI como homologação funcional de todos os labs.**

## Diagnóstico do estado atual

- A estrutura vigente na `main` tem **49 aulas, 49 labs e 49 infográficos**, com quatro ferramentas na Camada 04.
- A matriz registra **39 cenários cobertos pela CI Kind básica** e **10 com cobertura funcional separada ou pendente**.
- A PR [#39](https://github.com/DavidFerreira21/kubernetes-visual-guide/pull/39) foi criada antes da remoção de Crossplane, ESO e Karpenter; ainda usa contagens antigas e **não deve ser mesclada sem adaptação**.
- A PR #39 contém melhorias reutilizáveis: teste abrangente de links, execução de todos os testes Python na CI, guias de Traefik e Metrics Server, guardas de segurança para recursos globais, fluxo `lab-apply` e três cenários Kind adicionais. Esta retomada incorpora **os testes amplos, os dois guias e uma correção pedagógica do Lab 01.01**. As mudanças de segurança e smoke test ainda precisam de adaptação e validação próprias.
- No Lab 01.01, perguntas genéricas sobre `spec/status` eram inadequadas para um ConfigMap. Foram substituídas por perguntas relacionadas ao comportamento real do experimento.

## Critério de aceite do estudante — Camada 01

Para cada um dos **14 labs**:

1. O aluno encontra aula, infográfico e desafio sem depender da solução.
2. `make doctor` identifica os pré-requisitos; `make up` prepara o Kind e `make lab-prepare LAB=XX.XX` prepara o exercício.
3. O aluno completa o starter e aplica seu próprio manifesto, com contexto e namespace conferidos.
4. `make lab-test` identifica resultados esperados, e os comandos de observação demonstram o mecanismo da aula.
5. Existe uma hipótese explícita e um experimento controlado, com evidência de resultado, incluindo eventuais falhas.
6. `make lab-down` limpa somente os objetos pertinentes, sem destruir o cluster.
7. Links, documentos e infográficos correspondentes funcionam; uma revisão humana confirma clareza e correção técnica.

**Aviso:** estes critérios são o protocolo de homologação. Não representam afirmação de que os 14 labs já foram executados manualmente.

## Roteiro de execução

Começar pelos Labs **01.01, 01.02 e 01.03**, que estabelecem o fluxo introdutório: objetos declarativos, Pod independente e controlador Deployment. Depois executar **01.04 a 01.12** no Kind e registrar a evidência de cada experimento. Por fim, validar **01.13 com Traefik** e **01.14 com Metrics Server**, usando os guias de ambiente; se o addon não estiver pronto, classificar como pendente, não como aprovado.

Para cada execução registrar data, versão do Kubernetes, commit, contexto, manifesto escrito pelo estudante, comandos, saída esperada/observada, hipóteses, correções e cleanup, sem expor segredos.

## Critérios de conclusão da rodada

- Todos os 14 labs avaliados quanto à legibilidade e progressão didática.
- Evidências funcionais reproduzíveis, separando os labs que necessitam de addons.
- CI estática e smoke Kind aprovados na revisão proposta.
- PR #39 resolvida por adaptação seletiva ou encerramento documentado; **não fazer merge com a matriz curricular antiga**.
- Revisão visual dos 14 infográficos feita por humano, com eventual correção registrada por aula.

## Próximas melhorias, não incluídas nesta rodada

- Avaliar o mecanismo `lab-apply` e isolamento de recursos globais, sem permitir exclusão de objetos de outros usos.
- Ampliar cobertura de Kind dos Labs 02.09, 02.12 e 02.14, sem alterar o escopo de 49 labs.
- Executar homologação funcional documentada dos addons e revisar os infográficos.
