---
title: About permissions for GitHub Packages
intro: Learn about how to manage permissions for your packages.
product: '{% data reusables.gated-features.packages %}'
versions:
  free-pro-team: '*'
  enterprise-server: '>=2.22'
  github-ae: '*'
---

{% if currentVersion == "free-pro-team@latest" %}
The permissions for packages are either repository-scoped or user/organization-scoped.
{% endif %}

### Permissions for repository-scoped packages

A repository-scoped package inherits the permissions and visibility of the repository that owns the package. You can find a package scoped to a repository by going to the main page of the repository and clicking the **Packages** link to the right of the page.

The {% data variables.product.prodname_registry %} registries below use repository-scoped permissions:

  - Docker registry (`docker.pkg.github.com`)
  - npm registry
  - RubyGems registry
  - Apache Maven registry
  - NuGet registry

{% if currentVersion == "free-pro-team@latest" %}
### Granular permissions for user/organization-scoped packages

Pacotes com permissões granulares são escopos para uma conta de usuário pessoal ou de organização. You can change the access control and visibility of the package separately from a repository that is connected (or linked) to a package.

Atualmente, apenas o {% data variables.product.prodname_container_registry %} oferece permissões granulares para os seus pacotes de imagem de contêiner.

### Visibilidade e permissões de acesso para imagens de contêiner

{% data reusables.package_registry.visibility-and-access-permissions %}

Para obter mais informações, consulte "[Configurar o controle de acesso e visibilidade de um pacote](/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility)".

{% endif %}

### Sobre escopos e permissões para registros de pacotes

To use or manage a package hosted by a package registry, you must use a token with the appropriate scope, and your user account must have appropriate permissions.

Por exemplo:
-  To download and install packages from a repository, your token must have the `read:packages` scope, and your user account must have read permission.
- {% if currentVersion == "free-pro-team@latest" or if currentVersion ver_gt "enterprise-server@3.0" %}Para excluir um pacote em {% data variables.product.product_name %}, o seu token deve ter pelo menos o escopo `delete:packages` e `read:packages`. O escopo de `repo` também é necessário para pacotes com escopo de repositórios.{% elsif currentVersion ver_lt "enterprise-server@3.1" %}Para excluir uma versão especificada de um pacote privado em {% data variables.product.product_name %}, o seu token deve ter o escopo `delete:packages` e `repo`. Os pacotes públicos não podem ser excluídos.{% elsif currentVersion == "github-ae@latest" %}Para excluir uma versão específica de um pacote em {% data variables.product.product_name %}, seu token deve ter o escopo `delete:packages` e `repo`.{% endif %} Para obter mais informações, consulte "{% if currentVersion == "free-pro-team@latest" or currentVersion ver_gt "enterprise-server@3.0" %}[Excluindo e restaurando um pacote](/packages/learn-github-packages/deleting-and-restoring-a-package){% elsif currentVersion ver_lt "enterprise-server@3.1" or currentVersion == "github-ae@latest" %}[Excluindo um pacote](/packages/learn-github-packages/deleting-a-package){% endif %}.

| Escopo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | Descrição                                                                             | Permissão necessária |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- | -------------------- |
| `read:packages`                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Faça o download e instale pacotes do {% data variables.product.prodname_registry %}   | leitura              |
| `write:packages`                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Faça o upload e publique os pacotes em {% data variables.product.prodname_registry %} | gravação             |
| `delete:packages`                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |                                                                                       |                      |
| {% if currentVersion == "free-pro-team@latest" or currentVersion ver_gt "enterprise-server@3.0" %} Excluir pacotes de {% data variables.product.prodname_registry %} {% elsif currentVersion ver_lt "enterprise-server@3.1" %} Excluir versões especificadas de pacotes privados de {% data variables.product.prodname_registry %}{% elsif currentVersion == "github-ae@latest" %} Excluir versões especificadas de pacotes de {% data variables.product.prodname_registry %} {% endif %} |                                                                                       |                      |
| administrador                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |                                                                                       |                      |
| `repo`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | Faça o upload e exclua os pacotes (junto com `write:packages` ou `delete:packages`)   | write or admin       |

Ao criar um fluxo de trabalho de {% data variables.product.prodname_actions %}, você pode usar o `GITHUB_TOKEN` para publicar e instalar pacotes no {% data variables.product.prodname_registry %} sem precisar armazenar e gerenciar um token de acesso pessoal.

For more information, see:{% if currentVersion == "free-pro-team@latest" %}
- "[Configuring a package’s access control and visibility](/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility)"{% endif %}
- "[Publishing and installing a package with {% data variables.product.prodname_actions %}](/packages/managing-github-packages-using-github-actions-workflows/publishing-and-installing-a-package-with-github-actions)"
- "[Criar um token de acesso pessoal](/github/authenticating-to-github/creating-a-personal-access-token/)"
- "[Escopos disponíveis](/apps/building-oauth-apps/understanding-scopes-for-oauth-apps/#available-scopes)"

### Maintaining access to packages in {% data variables.product.prodname_actions %} workflows

To ensure your workflows will maintain access to your packages, ensure that you're using the right access token in your workflow and that you've enabled {% data variables.product.prodname_actions %} access to your package.

For more conceptual background on {% data variables.product.prodname_actions %} or examples of using packages in workflows, see "[Managing GitHub Packages using GitHub Actions workflows](/packages/managing-github-packages-using-github-actions-workflows)."

#### Access tokens

- To publish packages associated with the workflow repository, use `GITHUB_TOKEN`.
- To install packages associated with other private repositories that `GITHUB_TOKEN` can't access, use a personal access token

For more information about `GITHUB_TOKEN` used in {% data variables.product.prodname_actions %} workflows, see "[Authentication in a workflow](/actions/reference/authentication-in-a-workflow#using-the-github_token-in-a-workflow)."

{% if currentVersion == "free-pro-team@latest" %}
#### {% data variables.product.prodname_actions %} access for container images

To ensure your workflows have access to your container image, you must enable {% data variables.product.prodname_actions %} access to the repositories where your workflow is run. You can find this setting on your package's settings page. For more information, see "[Ensuring workflow access to your package](/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility#ensuring-workflow-access-to-your-package)."

{% endif %}
