# Jornada do aluno — Camada 03: arquitetura Kubernetes

[⌂ Início](../../README.md) · [Camada 03](../../trilhas/03-arquitetura/README.md) · [Preparar Kind](../../install-kind.md)

**Pré-requisito:** saiba criar e observar Pod, Deployment e Service. Nesta camada, seu objetivo é identificar **qual componente faz cada mudança**. Nem todo comportamento pode ser observado integralmente em Kind sem addons.

## Três marcos de compreensão

1. **03.01 a 03.05 — da API à reconciliação.** Comece com [Arquitetura 03.01](../../trilhas/03-arquitetura/arquitetura-cluster/README.md) e o [Lab 03.01](../../trilhas/03-arquitetura/labs/arquitetura-cluster-basico/README.md). Depois faça o [Lab 03.02 de API Server](../../trilhas/03-arquitetura/labs/kube-apiserver-basico/README.md) e siga etcd, admission e controller-manager. Evidência: diferencie requisição aceita, objeto persistido, scheduling e convergência de controller.
2. **03.06 a 03.11 — APIs customizadas versus controllers.** Compare o [Lab 03.06 de CRD](../../trilhas/03-arquitetura/labs/crds-custom-resources-basico/README.md) com o [Lab 03.07 de controllers](../../trilhas/03-arquitetura/labs/custom-controllers-operators-basico/README.md). **Registrar uma CRD não cria automaticamente um reconciler.** O lab básico demonstra reconciliação nativa; o exemplo opcional de Operator exige preparação própria. Continue scheduler, CCM, kubelet e runtime observando os limites do ambiente local.
3. **03.12 a 03.17 — rede, storage e entrega.** Faça os labs sequencialmente pelo [índice](../../trilhas/03-arquitetura/README.md). Verifique Pod IP, Service, DNS e PVC antes de afirmar que CNI/CSI estão funcionando. Em Gateway API, a existência de um HTTPRoute sem controlador compatível não prova roteamento. Compare renderização Kustomize com instalação real de charts somente quando Helm estiver disponível.

## Fluxo seguro do estudante

```bash
kubectl config current-context # Esperado: kind-dev
make lab-prepare LAB=03.02
# Complete .labs/03.02/resources.yaml
kubectl apply -f .labs/03.02/
make lab-observe LAB=03.02
make lab-test LAB=03.02
# Faça o experimento da aula e documente eventos/status.
make lab-down LAB=03.02
```

Os labs com CRDs criam definições de **escopo do cluster**. Execute apenas em Kind descartável, inventarie instâncias antes de excluir definições e não use `make lab-up` como estudante.

## Critério de conclusão

Para cada recurso, explique sua **responsabilidade exata**, onde observar evidências e quais resultados ficam fora do escopo do Kind. Documente as falhas e a limpeza; se não houver addon necessário, sinalize *dependência ausente*, nunca *funcional aprovado*. Confira a [matriz de homologação](../matriz-homologacao.md).
