# Preparar o ambiente local — Kind (Windows, Linux ou macOS)

[← Início](README.md) · [Comandos dos labs](docs/labs.md)

Este guia prepara o laboratório local para executar os exercícios no Kind. O roteiro de Windows com WSL2 está detalhado abaixo; quem usa Linux ou macOS pode ir diretamente à instalação de Docker, Kind e kubectl para seu sistema. Confira sempre a versão compatível nas fontes oficiais.

**Antes de começar:** tenha um terminal Bash, Git, Make, Python 3, Docker com daemon acessível, Kind e kubectl. Faça o clone do repositório, entre na pasta que contém o `Makefile` e instale PyYAML. Os comandos `make` e caminhos `.labs/` são executados a partir dessa pasta.

```bash
git clone https://github.com/DavidFerreira21/kubernetes-visual-guide.git
cd kubernetes-visual-guide
python3 -m pip install 'PyYAML>=6,<7'
```

Se seu Python estiver em ambiente gerenciado e bloquear `pip install`, crie um ambiente virtual (`python3 -m venv .venv`, depois `source .venv/bin/activate`) e instale PyYAML nele. Execute os demais comandos com esse ambiente ativado.

**Linux/macOS:** instale e inicialize o Docker segundo a documentação do seu sistema, depois Kind e kubectl pelas instruções oficiais da seção 3. Confirme `docker info` e `make doctor`; não execute os comandos PowerShell/WSL abaixo. Em Linux, seu usuário precisa conseguir acessar o daemon Docker. Em macOS, use o terminal com Bash e Docker Desktop ativo.

**Windows:** siga as seções 1 e 2 antes da instalação de Kind e kubectl.

## 1. Windows: preparar WSL2

**Execute no PowerShell do Windows, como Administrador**:

```powershell
wsl --install -d Ubuntu
wsl --set-default-version 2
wsl --set-default Ubuntu
wsl --list --verbose
```

Reinicie o Windows se solicitado. Abra **Ubuntu (WSL)** para os próximos comandos Linux.

## 2. Windows: preparar Docker Desktop

Instale [Docker Desktop](https://docs.docker.com/desktop/setup/install/windows-install/) e habilite **Settings → Resources → WSL Integration** para Ubuntu.

No terminal do **Ubuntu (WSL)**, confirme:

```bash
docker version
docker ps
```

Os dois comandos precisam funcionar antes de criar o cluster.

## 3. Instalar Kind e kubectl (todos os sistemas)

Siga as instruções oficiais para seu sistema e arquitetura:

- [Kind — Quick Start](https://kind.sigs.k8s.io/docs/user/quick-start/)
- [kubectl — Instalação](https://kubernetes.io/docs/tasks/tools/)

Confirme os binários:

```bash
kind version
kubectl version --client
```

## 4. Criar e verificar o cluster

Execute na **raiz do repositório**:

```bash
make doctor
make help
make up
kubectl config current-context  # Esperado: kind-dev
make nodes
```

O Makefile cria um cluster Kind chamado `dev` (contexto esperado: `kind-dev`) usando [kind-config.yaml](kind-config.yaml). A configuração tem um nó Control Plane, dois Workers e mapeamento das portas 80/443 para experimentos futuros. Se essas portas já estiverem ocupadas na máquina, ajuste o mapeamento na configuração antes de criar o cluster.

Se quiser criar sem o Makefile:

```bash
kind create cluster --name dev --config kind-config.yaml
kubectl get nodes --context kind-dev
```

## 5. Executar o primeiro lab sem copiar a solução

Leia a [aula 01.01](trilhas/01-fundamentos/fundamentos/README.md) antes de iniciar o [laboratório 01.01](trilhas/01-fundamentos/labs/fundamentos-cluster/README.md). O aluno deve preencher o YAML incompleto, não aplicar a solução da CI.

```bash
make validate
make lab-prepare LAB=01.01
# Edite .labs/01.01/configmap.yaml seguindo o desafio do Lab 01.01.
kubectl apply -f .labs/01.01/
make lab-test LAB=01.01
make lab-observe LAB=01.01
# Execute o experimento do lab e registre o resultado.
make lab-down LAB=01.01
```

Caso apareça `Forbidden`, `connection refused` ou um namespace inesperado, confira `kubectl config current-context` e os eventos antes de repetir o comando. Para seguir da primeira aula até Deployments, use o [percurso inicial do aluno](docs/primeiros-passos.md).

> **Não use `make lab-up` como aluno:** esse comando aplica a solução de referência e é reservado à CI. `make lab-prepare` cria o namespace e o arquivo inicial sem aplicar a solução. O comando de limpeza é `make lab-down LAB=XX.XX`, e `make down` destrói o cluster inteiro.

## 6. Limpeza

- `make lab-down LAB=01.01` remove os recursos declarados do primeiro exercício e preserva o namespace `lab-fundamentos`.
- `make down` **apaga o cluster Kind `dev` inteiro**, incluindo seus recursos e dados. Execute apenas se não precisar mais dele.

## 7. Ingress

O cluster tem portas 80/443 mapeadas, mas **não instala automaticamente um Ingress Controller**. O [🧪 Lab 01.13](trilhas/01-fundamentos/labs/ingress-basico/README.md) usa Traefik como Ingress Controller opcional, instalado conforme o roteiro. Não dependa de um antigo alvo `make install-nginx`, pois ele não faz parte do projeto.

Referências: [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/) e [Gateway API](https://kubernetes.io/docs/concepts/services-networking/gateway/).
