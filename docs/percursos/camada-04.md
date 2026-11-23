# Jornada do aluno — Camada 04: ferramentas do ecossistema

[⌂ Início](../../README.md) · [Camada 04](../../trilhas/04-platform-engineering/README.md) · [Guias de preparação](../ambientes/README.md)

**Pré-requisito:** entenda CRDs, controllers, Services e o ciclo de reconciliação. Esta camada exige **addons instalados intencionalmente**. O Kind padrão não instala Argo CD, Kyverno, Cilium nem Prometheus Operator.

## Antes de cada lab: separar declaração de efeito

1. Leia a aula teórica e seu infográfico; identifique o controlador responsável.
2. Escolha um **Kind descartável** e siga o [guia do addon](../ambientes/README.md), conferindo versão, namespace, CRDs e saúde dos controladores. Para Cilium, use o Kind dedicado com CNI padrão desativado.
3. Só depois execute `make lab-prepare LAB=04.XX`, complete seu próprio YAML em `.labs/04.XX/` e faça `kubectl apply`.
4. Compare resultado declarativo (`kubectl get`), funcional (reconciliação, política, tráfego ou coleta) e diagnóstico. Registre diferença entre ausência de dependência e falha do manifesto.
5. Faça cleanup do lab sem excluir addons compartilhados. Desinstale addon somente quando ele pertencer integralmente ao ambiente descartável.

## Roteiro de evidências

- **04.01 — [Argo CD](../../trilhas/04-platform-engineering/labs/argo-cd-basico/README.md):** prove que Application está Synced e Healthy **em instalação que observa o namespace do lab**; objeto aceito por CRD não demonstra sync.
- **04.02 — [Kyverno](../../trilhas/04-platform-engineering/labs/kyverno-basico/README.md):** evidencie uma admissão permitida e outra negada sob a política ativa; um YAML válido não comprova enforcement.
- **04.03 — [Cilium](../../trilhas/04-platform-engineering/labs/cilium-basico/README.md):** mostre comunicação permitida e bloqueada no Kind Cilium dedicado, não somente a presença de CiliumNetworkPolicy.
- **04.04 — [Prometheus e Grafana](../../trilhas/04-platform-engineering/labs/prometheus-grafana-basico/README.md):** confirme ServiceMonitor selecionado, target UP e consulta de métrica. Sem operator e scrape, o recurso é apenas declarativo.

## Fluxo do estudante

```bash
kubectl config current-context  # Verifique o Kind correto
make lab-prepare LAB=04.01
# Complete .labs/04.01/resources.yaml após instalar o addon.
kubectl apply -f .labs/04.01/
make lab-observe LAB=04.01
make lab-test LAB=04.01
# Reproduza o experimento e capture status/resultado.
make lab-down LAB=04.01
```

`make lab-up` aplica a solução de referência da CI, **não instala nem homologa addons**. A classificação funcional só deve ser atribuída com evidência. Consulte a [matriz](../matriz-homologacao.md).
