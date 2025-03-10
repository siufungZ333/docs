---
title: Secretos cifrados
intro: 'Los secretos cifrados te permiten almacenar información sensible en tu organización{% ifversion fpt or ghes > 3.0 or ghec %}, repositorio o ambientes de repositorio{% else %} o repositorio{% endif %}.'
redirect_from:
  - /github/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets
  - /actions/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets
  - /actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets
  - /actions/configuring-and-managing-workflows/using-variables-and-secrets-in-a-workflow
  - /actions/reference/encrypted-secrets
versions:
  fpt: '*'
  ghes: '*'
  ghae: '*'
  ghec: '*'
---

{% data reusables.actions.enterprise-beta %}
{% data reusables.actions.enterprise-github-hosted-runners %}

## Acerca de los secretos cifrados

Los secretos son variables de ambiente cifradas que creas en una organización{% ifversion fpt or ghes > 3.0 or ghae or ghec %}, repositorio, o ambiente de repositorio{% else %} o repositorio{% endif %}. Los secretos que creas están disponibles para utilizarse en los flujos de trabajo de {% data variables.product.prodname_actions %}. {% data variables.product.prodname_dotcom %} utiliza una [caja sellada de libsodium](https://libsodium.gitbook.io/doc/public-key_cryptography/sealed_boxes) para ayudarte a garantizar que los secretos se cifran antes de llegar a {% data variables.product.prodname_dotcom %} y que permanezcan cifrados hasta que los utilices en un flujo de trabajo.

{% data reusables.github-actions.secrets-org-level-overview %}

{% ifversion fpt or ghes > 3.0 or ghae or ghec %}
Para que los secretos se almacenen a nivel de ambiente, puedes habilitar los revisores requeridos para controlar el acceso a los secretos. Un job de flujo de trabajo no puede acceder a los secretos del ambiente hasta que los revisores requeridos le otorguen aprobación.
{% endif %}

{% ifversion fpt or ghec or ghae-issue-4856 %}

{% note %}

**Nota**: {% data reusables.actions.about-oidc-short-overview %}

{% endnote %}

{% endif %}

### Nombrar tus secretos

{% data reusables.codespaces.secrets-naming %}

  Por ejemplo, {% ifversion fpt or ghes > 3.0 or ghae or ghec %}un secreto que se creó a nivel de ambiente debe tener un nombre único en éste, {% endif %}un secreto que se creó a nivel de repositorio debe tener un nombre único en éste, y un secreto que se creó a nivel de organización debe tener un nombre único en este nivel.

  {% data reusables.codespaces.secret-precedence %}{% ifversion fpt or ghes > 3.0 or ghae or ghec %} De forma similar, si una organización, repositorio y ambiente tienen el mismo secreto con el mismo nombre, el secreto a nivel de ambiente tomará precedencia.{% endif %}

Para ayudarte a garantizar que {% data variables.product.prodname_dotcom %} redacta tus secretos en bitácoras, evita utilizar datos estructurados como los valores de los secretos. Por ejemplo, evita crear secretos que contengan JSON o blobs de Git codificados.

### Acceder a tus secretos

Para hacer que un secreto esté disponible para una acción, debes configurar el secreto como una variable de entrada o de entorno en tu archivo de flujo de trabajo. Revisa el archivo README de la acción para saber qué variables de entrada y de entorno espera la acción. Para obtener más información, consulta "[Sintaxis del flujo de trabajo para{% data variables.product.prodname_actions %}](/articles/workflow-syntax-for-github-actions/#jobsjob_idstepsenv)".

Puedes usar y leer secretos cifrados en un archivo de flujo de trabajo si tienes acceso para editar el archivo. Para obtener más información, consulta "[Permisos de acceso en {% data variables.product.prodname_dotcom %}](/github/getting-started-with-github/access-permissions-on-github)."

{% data reusables.github-actions.secrets-redaction-warning %}

{% ifversion fpt or ghes > 3.0 or ghae or ghec %}
Los secretos de organización y de repositorio se leen cuando una ejecución de flujo de trabajo está en cola y los secretos de ambiente se leen cuando un job que referencia el ambiente comienza.
{% endif %}

También puedes administrar secretos utilizando la API de REST. Para obtener más información, consulta la sección "[Secretos](/rest/reference/actions#secrets)".

### Limitar permisos de credenciales

Cuando generes credenciales, te recomendamos que otorgues los mínimos permisos posibles. Por ejemplo, en lugar de usar credenciales personales, usa [implementar claves](/developers/overview/managing-deploy-keys#deploy-keys) o una cuenta de servicio. Considera otorgar permisos de solo lectura si eso es todo lo que se necesita, y limita el acceso lo máximo posible. Cuando generes un token de acceso personal (PAT), selecciona el menor número de ámbitos necesarios.

{% note %}

**Nota:** Puedes utilizar la API de REST para administrar los secretos. Para obtener más información, consulta la sección "[ API de secretos de {% data variables.product.prodname_actions %}](/rest/reference/actions#secrets)".

{% endnote %}

## Crear secretos cifrados para un repositorio

{% data reusables.github-actions.permissions-statement-secrets-repository %}

{% webui %}

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.github-actions.sidebar-secret %}
1. Da clic en **Secreto de repositorio nuevo**.
1. Teclea un nombre para tu secreto en el cuadro de entrada **Name**.
1. Ingresa el valor de tu secreto.
1. Haz clic en **Agregar secreto** (Agregar secreto).

Si tu repositorio {% ifversion fpt or ghes > 3.0 or ghae or ghec %}tiene secretos de ambiente o {% endif %}puede acceder a los secretos de la organización padre, entonces estos secretos también se listan en esta página.

{% endwebui %}

{% cli %}

{% data reusables.cli.cli-learn-more %}

Para agregar un secreto de repositorio, utiliza el subcomando `gh secret set`. Reemplaza a `secret-name` con el nombre de tu secreto.

```shell
gh secret set <em>secret-name</em>
```

El CLI te pedirá ingresar un valor secreto. Como alternativa, puedes leer el valor del secreto desde un archivo.

```shell
gh secret set <em>secret-name</em> < secret.txt
```

Para listar todos los secretos del repositorio, utiliza el subcomando `gh secret list`.

{% endcli %}

{% ifversion fpt or ghes > 3.0 or ghae or ghec %}

## Crear secretos cifrados para un ambiente

{% data reusables.github-actions.permissions-statement-secrets-environment %}

{% webui %}

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.github-actions.sidebar-environment %}
1. Da clic en el ambiente al cual quieras agregar un secreto.
2. Debajo de **Secretos de ambiente**, da clic en **Agregar secreto**.
3. Teclea un nombre para tu secreto en el cuadro de entrada **Name**.
4. Ingresa el valor de tu secreto.
5. Haz clic en **Agregar secreto** (Agregar secreto).

{% endwebui %}

{% cli %}

Para agregar un secreto para un ambiente, utiliza el subcomando `gh secret set` con el marcador `--env` o `-e` seguido del nombre del ambiente.

```shell
gh secret set --env <em>environment-name</em> <em>secret-name</em>
```

Para listar todos los secretos de un ambiente, utiliza el subcomando `gh secret list` con el marcador `--env` o `-e` seguido del nombre de ambiente.

```shell
gh secret list --env <em>environment-name</em>
```

{% endcli %}

{% endif %}

## Crear secretos cifrados para una organización

Cuando creas un secreto en una organización, puedes utilizar una política para limitar el acceso de los repositorios a este. Por ejemplo, puedes otorgar acceso a todos los repositorios, o limitarlo a solo los repositorios privados o a una lista específica de estos.

{% data reusables.github-actions.permissions-statement-secrets-organization %}

{% webui %}

{% data reusables.organizations.navigate-to-org %}
{% data reusables.organizations.org_settings %}
{% data reusables.github-actions.sidebar-secret %}
1. Da clic en **Secreto de organización nuevo**.
1. Teclea un nombre para tu secreto en el cuadro de entrada **Name**.
1. Ingresa el **Valor** para tu secreto.
1. Desde la lista desplegable **Acceso de los repositorios**, elige una política de acceso.
1. Haz clic en **Agregar secreto** (Agregar secreto).

{% endwebui %}

{% cli %}

{% note %}

**Nota:** Predeterminadamente, el {% data variables.product.prodname_cli %} se autentica con los alcances `repo` y `read:org`. Para administrar los secretos de la organización, debes autorizar el alcance `admin:org` adicionalmente.

```
gh auth login --scopes "admin:org"
```

{% endnote %}

Para agregar un secreto para una organización, utiliza el subcomando `gh secret set` con el marcador `--org` o `-o` seguido del nombre de la organización.

```shell
gh secret set --org <em>organization-name</em> <em>secret-name</em>
```

Predeterminadamente, el secreto solo se encuentra disponible para los repositorios privados. Para especificar que el secreto debe estar disponible para todos los repositorios dentro de la organización, utiliza el marcador `--visibility` o `-v`.

```shell
gh secret set --org <em>organization-name</em> <em>secret-name</em> --visibility all
```

Para especificar que el secreto debe estar disponible para los repositorios seleccionados dentro de la organización, utiliza el marcador `--repos` o `-r`.

```shell
gh secret set --org <em>organization-name</em> <em>secret-name</em> --repos <em>repo-name-1</em>,<em>repo-name-2</em>"
```

Para listar todos los secretos para una organización, utiliza el subcomando `gh secret list` con el marcador `--org` o `-o` seguido del nombre de la organización.

```shell
gh secret list --org <em>organization-name</em>
```

{% endcli %}

## Revisar el acceso a los secretos de nivel organizacional

Puedes revisar qué políticas de acceso se están aplicando a un secreto en tu organización.

{% data reusables.organizations.navigate-to-org %}
{% data reusables.organizations.org_settings %}
{% data reusables.github-actions.sidebar-secret %}
1. La lista de secretos incluye cualquier política y permiso configurado. Por ejemplo: ![Lista de secretos](/assets/images/help/settings/actions-org-secrets-list.png)
1. Para encontrar más detalles sobre los permisos configurados para cada secreto, da clic en **Actualizar**.

## Usar secretos cifrados en un flujo de trabajo

{% note %}

**Nota:**{% data reusables.actions.forked-secrets %}

{% endnote %}

Para proporcionar una acción con un secreto como una variable de entrada o de entorno, puedes usar el contexto de `secretos` para acceder a los secretos que has creado en tu repositorio. Para obtener más información, consulta las secciones de "[Contextos](/actions/learn-github-actions/contexts)" y "[Sintaxis de flujo de trabajo para {% data variables.product.prodname_actions %}](/github/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions)".

{% raw %}
```yaml
steps:
  - name: Hello world action
    with: # Set the secret as an input
      super_secret: ${{ secrets.SuperSecret }}
    env: # Or as an environment variable
      super_secret: ${{ secrets.SuperSecret }}
```
{% endraw %}

Evita pasar secretos entre procesos desde la línea de comando, siempre que sea posible. Los procesos de la línea de comando pueden ser visibles para otros usuarios (utilizando el comando `ps`) o ser capturados por [eventos de auditoría de seguridad](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/command-line-process-auditing). Para ayudar a proteger los secretos, considera usar variables de entorno, `STDIN` u otros mecanismos admitidos por el proceso de destino.

Si debes pasar secretos dentro de una línea de comando, enciérralos usando las normas de uso de comillas adecuadas. Los secretos suelen contener caracteres especiales que pueden afectar involuntariamente a tu shell. Para evitar estos caracteres especiales, usa comillas en tus variables de entorno. Por ejemplo:

### Ejemplo usando Bash

{% raw %}
```yaml
steps:
  - shell: bash
    env:
      SUPER_SECRET: ${{ secrets.SuperSecret }}
    run: |
      example-command "$SUPER_SECRET"
```
{% endraw %}

### Ejemplo usando PowerShell

{% raw %}
```yaml
steps:
  - shell: pwsh
    env:
      SUPER_SECRET: ${{ secrets.SuperSecret }}
    run: |
      example-command "$env:SUPER_SECRET"
```
{% endraw %}

### Ejemplo usando Cmd.exe

{% raw %}
```yaml
steps:
  - shell: cmd
    env:
      SUPER_SECRET: ${{ secrets.SuperSecret }}
    run: |
      example-command "%SUPER_SECRET%"
```
{% endraw %}

## Límites para los secretos

Puedes almacenar hasta 1000 secretos de organización{% ifversion fpt or ghes > 3.0 or ghae or ghec %}, 100 secretos de repositoirio y 100 secretos de ambiente{% else %} y 100 secretos de repositorio{% endif %}.

Un flujo de trabajo que se haya creado en un repositorio puede acceder a la siguiente cantidad de secretos:

* Todos los 100 secretos de repositorio.
* Si se asigna acceso a más de 100 secretos de la organización para este repositorio, el flujo de trabajo solo puede utilizar los primeros 100 secretos de organización (que se almacenan por orden alfabético por nombre de secreto).
{% ifversion fpt or ghes > 3.0 or ghae or ghec %}* Todos los 100 secretos de ambiente.{% endif %}

Los secretos tienen un tamaño máximo de 64 KB. Para usar secretos de un tamaño mayor a 64 KB, puedes almacenar los secretos cifrados en tu repositorio y guardar la contraseña de descifrado como un secreto en {% data variables.product.prodname_dotcom %}. Por ejemplo, puedes usar `gpg` para cifrar tus credenciales de manera local antes de verificar el archivo en tu repositorio en {% data variables.product.prodname_dotcom %}. Para obtener más información, consulta la página del manual "[gpg](https://www.gnupg.org/gph/de/manual/r1023.html)".

{% warning %}

**Advertencia**: Evita que tus secretos se impriman cuando se ejecute tu acción. Cuando usas esta solución, {% data variables.product.prodname_dotcom %} no redacta los secretos que están impresos en los registros.

{% endwarning %}

1. Ejecuta el siguiente comando en tu terminal para cifrar el archivo `my_secret.json` usando `gpg` y el algoritmo de cifras AES256.

 ``` shell
 $ gpg --symmetric --cipher-algo AES256 my_secret.json
 ```

1. Se te pedirá que ingreses una contraseña. Recuerda la contraseña, porque deberás crear un nuevo secreto en {% data variables.product.prodname_dotcom %} que use esa contraseña como valor.

1. Crear un nuevo secreto que contiene la frase de acceso. Por ejemplo, crea un nuevo secreto con el nombre `LARGE_SECRET_PASSPHRASE` y establece el valor del secreto para la contraseña que seleccionaste en el paso anterior.

1. Copia tu archivo cifrado en tu repositorio y confírmalo. En este ejemplo, el archivo cifrado es `my_secret.json.gpg`.

1. Crea un script shell para descifrar la contraseña. Guarda este archivo como `decrypt_secret.sh`.

  ``` shell
  #!/bin/sh

  # Decrypt the file
  mkdir $HOME/secrets
  # --batch to prevent interactive command
  # --yes to assume "yes" for questions
  gpg --quiet --batch --yes --decrypt --passphrase="$LARGE_SECRET_PASSPHRASE" \
  --output $HOME/secrets/my_secret.json my_secret.json.gpg
  ```

1. Asegúrate de que tu shell script sea ejecutable antes de verificarlo en tu repositorio.

  ``` shell
  $ chmod +x decrypt_secret.sh
  $ git add decrypt_secret.sh
  $ git commit -m "Add new decryption script"
  $ git push
  ```

1. En tu flujo de trabajo, usa un `step` para llamar al shell script y descifrar el secreto. Para tener una copia de tu repositorio en el entorno en el que se ejecuta tu flujo de trabajo, deberás usar la acción [`code>actions/checkout`](https://github.com/actions/checkout). Haz referencia a tu shell script usando el comando `run` relacionado con la raíz de tu repositorio.

{% raw %}
  ```yaml
  name: Workflows with large secrets

  on: push

  jobs:
    my-job:
      name: My Job
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v2
        - name: Decrypt large secret
          run: ./.github/scripts/decrypt_secret.sh
          env:
            LARGE_SECRET_PASSPHRASE: ${{ secrets.LARGE_SECRET_PASSPHRASE }}
        # Este comando es solo un ejemplo para mostrar cómo se imprime tu secreto.
        # Asegúrate de eliminar las declaraciones impresas de tus secretos. GitHub 
        # no oculta los secretos que usan esta solución.
        - name: Test printing your secret (Elimina este paso en la producción)
          run: cat $HOME/secrets/my_secret.json
  ```
{% endraw %}


## Almacenar blobs binarios en Base64 como secretos

Puedes utilizar el cifrado en Base64 para almacenar blobs binarios pequeños como secretos. Puedes referenciar el secreto en tu flujo de trabajo y decodificarlo para utilizarlo en el ejecutor. Para los límites de tamaño, consulta la sección de [Límites para los secretos"](/actions/security-guides/encrypted-secrets#limits-for-secrets).

{% note %}

**Nota**: Toma en cuenta que Base64 solo convierte los binarios a texto y no es un sustituto de un cifrado real.

{% endnote %}

1. Utiliza `base64` para cifrar tu archivo en una secuencia Base64. Por ejemplo:

   ```
   $ base64 -i cert.der -o cert.base64
   ```

1. Crea un secreto que contenga la secuencia de Base64. Por ejemplo:

   ```
   $ gh secret set CERTIFICATE_BASE64 < cert.base64
   ✓ Set secret CERTIFICATE_BASE64 for octocat/octorepo
   ```

1. Para acceder a la secuencia de Base64 desde tu ejecutor, lleva el secreto a `base64 --decode`.  Por ejemplo:

   ```yaml
   name: Retrieve Base64 secret
   on:
     push:
       branches: [ octo-branch ]
   jobs:
     decode-secret:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v2
         - name: Retrieve the secret and decode it to a file
           env:
             {% raw %}CERTIFICATE_BASE64: ${{ secrets.CERTIFICATE_BASE64 }}{% endraw %}
           run: |
             echo $CERTIFICATE_BASE64 | base64 --decode > cert.der
         - name: Show certificate information
           run: |
             openssl x509 -in cert.der -inform DER -text -noout
   ```

