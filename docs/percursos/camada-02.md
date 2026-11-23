# Jornada do aluno — Camada 02: operação e segurança

[⌂ Início](../../README.md) · [Camada 02](../../trilhas/02-operacao-seguranca/README.md) · [Preparar Kind](../../install-kind.md)

**Pré-requisito:** conclua ao menos Pods, Deployments, Services e Namespaces da Camada 01. Trabalhe na raiz do repositório com `kubectl config current-context` retornando `kind-dev`. Os labs desta camada exigem observação de permissões, eventos e agendamento; **não devem ser realizados em cluster compartilhado**.

## Percurso essencial — descubra o efeito, não apenas o YAML

1. **02.01 → 02.02, identificação e autorização:** leia [Labels/Selectors](../../trilhas/02-operacao-seguranca/labels-selectors-annotations/README.md), pratique [Lab 02.01](../../trilhas/02-operacao-seguranca/labs/labels-selectors-basico/README.md); depois leia [RBAC](../../trilhas/02-operacao-seguranca/rbac-serviceaccount/README.md) e faça [Lab 02.02](../../trilhas/02-operacao-seguranca/labs/rbac-minimo/README.md). Evidência: comando `kubectl auth can-i` autorizado e negado para a identidade correta; observe namespace e escopo.
2. **02.03 → 02.04, admissão e limites:** leia [SecurityContext/PSA](../../trilhas/02-operacao-seguranca/securitycontext-pod-security/README.md), execute [Lab 02.03](../../trilhas/02-operacao-seguranca/labs/securitycontext-psa-basico/README.md) e avance ao [Lab 02.04 — Quotas](../../trilhas/02-operacao-seguranca/labs/quota-namespace/README.md). Evidência: diferencie aviso, rejeição pela API e recurso aceito. Use dry-run quando previsto e reverta labels temporárias.
3. **02.05 → 02.11, execução e disponibilidade:** siga a ordem no [catálogo da camada](../../trilhas/02-operacao-seguranca/README.md). Em Scheduling e Spread, compare Pods agendados e Pending. Em QoS e PDB, registre condições observadas; **não simule eviction ou drain destrutivo** fora do roteiro.
4. **02.12 → 02.14, manutenção, rede e troubleshooting:** [Lab 02.12](../../trilhas/02-operacao-seguranca/labs/node-maintenance-basico/README.md) altera agendamento em Kind isolado e exige uncordon. [Lab 02.13](../../trilhas/02-operacao-seguranca/labs/networkpolicy-basico/README.md) exige CNI com enforcement para provar bloqueio; YAML aceito é apenas evidência declarativa. [Lab 02.14](../../trilhas/02-operacao-seguranca/labs/troubleshooting-basico/README.md) deve registrar a falha de pull, hipótese e recuperação.

## Ritual por laboratório

```bash
kubectl config current-context
make lab-prepare LAB=02.02
# Leia o desafio e escreva .labs/02.02/resources.yaml
kubectl apply -f .labs/02.02/
make lab-observe LAB=02.02
make lab-test LAB=02.02
# Realize o experimento indicado no próprio laboratório.
make lab-down LAB=02.02
```

**Não use `make lab-up`**, que aplica a solução da CI; não use `make down`, que destrói o Kind. Para recursos de escopo global, confira ownership e objetos preexistentes antes da limpeza.

## Evidência e critérios

Para concluir, explique: **quem pode executar a operação, qual regra decide o resultado, qual evidência prova a decisão e como voltar ao estado inicial**. Se faltar CNI, permissão ou condição reproduzível, marque o cenário funcional como *pendente* e registre o motivo. Volte à [matriz de homologação](../matriz-homologacao.md).
