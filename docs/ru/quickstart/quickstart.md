# 1. Создайте новый проект

Цель этого краткого руководства по началу работы - предоставить вам полную настройку разработки и пошаговые инструкции по созданию вашего первого блокчейн проекта SubQuery. Он ориентирован на опытных разработчиков и тех, кто только начинает свой путь в блокчейне.

Это краткое руководство по началу работы должно занять около 10-15 минут.

После завершения этого краткого руководства у вас будет рабочий проект SubQuery, который будет выполняться на ноде SubQuery. Вы сможете адаптировать стандартный стартовый проект и индексировать переводы из вашей любимой блокчейн сети, такой, как Polkadot, Avalanche, Cosmos и т. д.

Давайте начнем процесс создания вашего первого блокчейн проекта SubQuery.

## Предварительные условия

Прежде чем вы начнете создавать свой первый блокчейн проект с помощью SubQuery, убедитесь, что у вас установлены необходимые вспомогательные программные приложения. Это:

- [Node](https://nodejs.org/en/): современная (например, LTS-версия) установка Node.
- [Docker](https://docker.com/): В этом учебнике будет использоваться необходимый Docker.

Теперь вы готовы начать с первого шага, который заключается в установке SubQuery CLI.

## 1. Установите SubQuery CLI

Установите SubQuery CLI глобально на свой терминал с помощью NPM:

```shell
# NPM
npm install -g @subql/cli
```

::: опасность Мы ** НЕ** рекомендуем использовать `yarn global` для установки `@subql/cli` из-за плохого управления зависимостями. так как это может привести к многочисленным ошибкам. :::

Взгляните на все доступные команды и их использование. Выполните приведенную ниже команду в командной строке:

```shell
subql help
```

## 2. Инициализируем Начальный Проект SubQuery

Выполните следующую команду внутри каталога, в котором вы хотите создать проект SubQuery:

```shell
subql init
```

::: предупреждение Важно

**Для пользователей Cosmos**

Cosmos пока не поддерживается в SubQuery's CLI (`subql`). Следовательно, если вы используете Cosmos, вы должны начать с клона Juno или сделать форк [стартовый проект](https://github.com/subquery/cosmos-subql-starter).

Чтобы инициализировать ваш проект с помощью Cosmos, выполните 4 шага, показанных в этой [ссылке](https://github.com/subquery/juno-subql-starter#readme). Как только вы выполните эти 4 шага, **перейдите** в раздел [Внести изменения в свой проект](../quickstart/quickstart.md#_3-make-changes-to-your-project). :::

По мере продвижения вперед вам будут задаваться определенные вопросы:

- **Имя проекта**: Имя проекта для вашего проекта SubQuery.
- **Network family**: Layer-1 блокчейн, которую этот проект SubQuery будет индексировать. Используйте клавиши со стрелками для выбора из доступных вариантов. Например, Polkadot, Avalanche, Cosmos или другая поддерживаемая сеть.
- **Network**: Конкретная сеть, которую будет индексировать этот проект SubQuery. Используйте клавиши со стрелками для выбора из доступных вариантов. Например, Polkadot, Avalanche или другая поддерживаемая сеть.
- **Template project**: Выберите шаблон проекта SubQuery, который станет отправной точкой в разработке. Мы предлагаем выбрать проект _"subql-starter"_ project.
- **RPC endpoint**: Укажите URL-адрес HTTPS для эндпоинта RPC, который будет использоваться по умолчанию для этого проекта. Вы можете быстро получить доступ к общедоступным эндпоинтам для разных сетей, создать свою собственную частную выделенную ноду, используя [OnFinality](https://app.onfinality.io), или просто использовать эндпоинт по умолчанию. Этот узел RPC должен быть архивным узлом (иметь состояние полной цепочки). В этом руководстве мы будем использовать значение по умолчанию. В зависимости от выбранной вами сети значение по умолчанию может быть:
  - Для Polkadot - _ "https://polkadot.api.onfinality.io"_,
  - Для Avalanche - _"https://avalanche.api.onfinality.io"_,
  - Для Terra - _“https://terra-columbus-5.beta.api.onfinality.io”_ and likewise for other networks. <br/>
- **Git repository**: Укажите URL-адрес Git для репозитория, в котором будет размещен этот проект SubQuery (при размещении в SubQuery Explorer) или примите указанное значение по умолчанию.
- **Authors**: Введите здесь владельца этого проекта SubQuery (например, ваше имя!) или примите предоставленное значение по умолчанию.
- **Description**: Предоставьте краткое описание вашего проекта, в котором объясняется, какие данные он содержит и что пользователи могут с ним делать, или примите предоставленное значение по умолчанию.
- **Version**: введите номер пользовательской версии или используйте значение по умолчанию (`1.0.0`).
- **Лицензия**: Укажите лицензию на программное обеспечение для этого проекта или примите значение по умолчанию (`MIT`).

Давайте рассмотрим пример:

```shell
$ subql init
Project name [subql-starter]: HelloWorld
? Select a network family Substrate
? Select a network Polkadot
? Select a template project subql-starter     Starter project for subquery
RPC endpoint: [wss://polkadot.api.onfinality.io/public-ws]:
Git repository [https://github.com/subquery/subql-starter]:
Fetching network genesis hash... done
Author [Ian He & Jay Ji]: Sean
Description [This project can be used as a starting po...]:
Version [1.0.0]:
License [MIT]:
Preparing project... done
HelloWorld is ready
```

После завершения процесса инициализации вы увидите папку с именем вашего проекта, созданную внутри каталога. Пожалуйста, обратите внимание, что содержимое этого каталога должно быть идентичным тому, что указано в [Directory Structure](../build/introduction.md#directory-structure).

Наконец, выполните следующую команду, чтобы установить зависимости нового проекта из каталога нового проекта.

<CodeGroup> <CodeGroupItem title="YARN" active> ```shell cd PROJECT_NAME yarn install ``` </CodeGroupItem>
<CodeGroupItem title="NPM"> ```shell cd PROJECT_NAME npm install ``` </CodeGroupItem> </CodeGroup>

Теперь вы инициализировали свой первый проект SubQuery всего за несколько простых шагов. Давайте теперь настроим стандартный шаблон проекта для конкретного интересующего блокчейна.

Возможно, вы захотите обратиться к [command line arguments](../run_publish/references.md), используемым в SubQuery. Это поможет вам лучше понять команды.

## 3. Внесите изменения в Свой проект

Есть 3 важных файла, которые необходимо изменить. К ним относятся:

1. Схема GraphQL в `schema.graphql`.
2. Манифест проекта в `project.yaml`.
3. Функции отображения в каталоге `src/mappings/`.

SubQuery supports various blockchain networks and provides a dedicated guide for each of them. Select your preferred blockchain under 2. Specific Chains and continue the quick start guide.