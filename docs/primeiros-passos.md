# Primeiros passos — sua primeira jornada no Kubernetes Visual Guide

[⌂ Início](../README.md) · [Preparar o ambiente](../install-kind.md) · [Camada 01](../trilhas/01-fundamentos/README.md)

> **Para quem é este roteiro:** quem está começando em Kubernetes e quer aprender fazendo. Você não precisa conhecer Pods, controllers ou GitOps antes de iniciar. Reserve tempo para observar o cluster e escrever seu próprio manifesto.

## Antes de estudar

1. Tenha Git, Docker, Kind, kubectl, Make, Bash, Python 3 e PyYAML instalados conforme o [guia de ambiente](../install-kind.md).
2. Na raiz do repositório, execute `make doctor`, depois `make up` e `make nodes`.
3. Confirme que `kubectl config current-context` retorna `kind-dev` e que os três nós estão `Ready`. Se não estiverem, não prossiga aplicando YAML.
4. Para validar exemplos sem alterar o cluster, execute `make validate`.

## Percurso recomendado — leia, pratique, compare

**Primeira aula: estado declarativo.** Leia [Fundamentos 01.01](../trilhas/01-fundamentos/fundamentos/README.md) e observe o infográfico. Responda: quem recebe o YAML? O que é estado desejado? Em seguida, abra o [Lab 01.01](../trilhas/01-fundamentos/labs/fundamentos-cluster/README.md). Complete um ConfigMap, faça um patch manual, observe por que ele **não** reverte sozinho e reaplique o manifesto. Registre o valor antes e depois.

**Segunda aula: um Pod isolado.** Leia [Pods 01.02](../trilhas/01-fundamentos/pods/README.md) e faça o [Lab 01.02](../trilhas/01-fundamentos/labs/pods-basico/README.md). Inspecione eventos, logs e nó escolhido. Exclua o Pod e comprove que ele não reaparece sem um controlador. Reaplique seu YAML e limpe.

**Terceira aula: reconciliação com Deployment.** Leia [Deployments 01.03](../trilhas/01-fundamentos/deployments/README.md) e realize o [Lab 01.03](../trilhas/01-fundamentos/labs/deployments-basico/README.md). Observe a relação Deployment → ReplicaSet → Pod. Exclua um Pod gerenciado e compare com o laboratório anterior.

Esse contraste é o primeiro marco do curso: **um arquivo YAML não é um reconciliador; os controllers mantêm recursos sob seu gerenciamento, e o GitOps será estudado depois.**

## Comandos do estudante

Todos os comandos abaixo são executados **na raiz do repositório** e usam somente um cluster Kind local:

```bash
make doctor
make up
kubectl config current-context  # Deve retornar kind-dev
make nodes

# Primeiro exercício: preencha o arquivo antes de aplicar
make lab-prepare LAB=01.01
# Edite .labs/01.01/configmap.yaml conforme o desafio
kubectl apply -f .labs/01.01/
make lab-test LAB=01.01
make lab-observe LAB=01.01
# Execute o experimento descrito no README do Lab 01.01
make lab-down LAB=01.01
```

Para os labs seguintes, substitua o identificador `LAB=01.01` pelo ID correspondente. Leia sempre a aula e o lab antes de executar comandos.

**Atenção:** `make lab-up` aplica a **solução pronta** e é reservado à CI. `make down` remove o **cluster Kind inteiro**; não use esse comando para encerrar um exercício. `make lab-down LAB=XX.XX` preserva o namespace. O comando `kubectl apply` aplica a sua versão do YAML; confira o contexto antes de usá-lo.

## Como saber se aprendeu?

Ao terminar cada laboratório, sem consultar `solution/`, você deve conseguir explicar o objetivo, mostrar o YAML criado, apresentar ao menos uma observação real (estado, evento, log ou outro sinal), justificar o comportamento encontrado e limpar os recursos. `make lab-test` ajuda, mas **um teste verde sozinho não prova aprendizado**.

Se surgir um erro, confira o contexto, a instalação, o YAML, o namespace e as mensagens de `kubectl describe` e `kubectl get events`. Compare os resultados com a seção **Resultado esperado e evidências** do próprio lab; use a solução somente para revisão após a tentativa.

## Depois do primeiro percurso

Continue pela [Aula 01.04 — Controladores](../trilhas/01-fundamentos/controladores/README.md) e pelo [catálogo da Camada 01](../trilhas/01-fundamentos/README.md). Ingress e HPA exigem addons: siga seus guias de preparação; não confunda objeto aceito com experimento funcional homologado.

[↑ Voltar ao início](../README.md) · [Guia de todos os laboratórios](labs.md)
