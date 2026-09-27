# Pipelines

Fluxo automatico: push na `master` -> CI (restore, build e testes) -> CD (imagem e deploy).
A CD so e disparada automaticamente quando a CI termina com sucesso na `master`.
As duas pipelines devem usar o mesmo repositorio para que a CD execute o mesmo commit validado pela CI.

## Configuracao no Azure DevOps

1. Cadastre a CI com o nome `UsersAPI-CI`, usando `Pipelines/CI.yml`.
2. Em `Pipelines/CD.yml`, o campo `resources.pipelines.source` deve corresponder exatamente ao nome da CI cadastrada (incluindo a pasta, se houver).
3. Cadastre ou atualize a CD usando `Pipelines/CD.yml`, no mesmo projeto e repositorio da CI.
4. Defina a branch padrao da CD (Default branch for manual and scheduled builds) como `refs/heads/master` e publique estes YAMLs nessa branch.
5. Autorize o recurso da CI para a CD se o Azure DevOps solicitar. Remova eventuais gatilhos de push configurados pela interface que sobrescrevam o YAML.

O gatilho de conclusao usa o mesmo commit automaticamente. Execucoes manuais da CD continuam disponiveis e nao passam por esse gatilho.

A solucao ainda nao possui projetos de teste; a etapa `dotnet test` executara os testes quando forem adicionados a solucao.

Referencia: [Gatilhos entre pipelines](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/pipeline-triggers?view=azure-devops).
