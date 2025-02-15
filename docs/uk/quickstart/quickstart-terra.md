# Швидкий старт Terra

У цьому короткому посібнику ми почнемо з простого стартового проекту Terra, а потім закінчимо індексацією деяких фактичних реальних даних. Це чудова основа для початку розробки власного проекту SubQuery.

**Якщо ви шукаєте посібники для Substrate/Polkadot, ви можете прочитати [Посібник із короткого запуску спеціального Substrate/Polkadot](./quickstart-polkadot).**

В кінці цього посібника у вас буде робочий проєкт SubQuery, який працює на вузлі SubQuery з кінцевою точкою GraphQL, з якої можна запитувати дані.

Якщо ви ще цього не зробили, ми пропонуємо вам ознайомитись із [ terminology ](../#terminology), що використовується в SubQuery.

**Метою цього короткого посібника є адаптація стандартного стартового проекту, щоб почати індексацію всіх переказів з Terra, це займе всього 10-15 хвилин**

## Підготовка

### Місцеве середовище розвитку

- [Node](https://nodejs.org/en/): сучасна (наприклад, версія LTS) інсталяція Node.
- [Docker](https://docker.com/): у цьому посібнику буде використовуватися необхідний Docker

### Встановити SubQuery CLI

Встановіть SubQuery CLI глобально на свій термінал за допомогою NPM:

```shell
# NPM
npm install -g @subql/cli
```

Зауважте, що ми **НЕ** заохочуємо використання `yarn global` для встановлення `@subql/cli` через погане керування залежностями, що може призвести до помилки в подальшому.

Потім ви можете запустити довідку, щоб побачити доступні команди та використання, надані CLI

```shell
subql help
```

## Ініціалізуйте проект SubQuery Starter

У каталозі, в якому ви хочете створити проект SubQuery, просто виконайте таку команду, щоб почати.

```shell
subql init
```

Під час ініціалізації проекту SubQuery вам зададуть певні запитання:

- Project Name: A name for your SubQuery project
- Сімейство мереж. Сімейство мереж блокчейн рівня 1, для індексації якого буде розроблено цей проект SubQuery, використовуйте клавіші зі стрілками на клавіатурі, щоб вибрати один із варіантів, для цього посібника ми будемо використовувати *"Terra"*
- Мережа: конкретна мережа, для індексації якої буде розроблено цей проект SubQuery. Використовуйте клавіші зі стрілками на клавіатурі, щоб вибрати один із параметрів, для цього посібника ми будемо використовувати *"Terra"*
- Шаблон: виберіть шаблон проекту SubQuery, який буде відправною точкою для початку розробки, ми пропонуємо вибрати *"Початковий проект"*
- Репозиторій Git (необов’язково): надайте URL-адресу Git до репозиторію, в якому буде розміщено цей проект SubQuery (якщо він розміщено в SubQuery Explorer)
- Кінцева точка RPC (обов’язково): надайте URL-адресу HTTPS для запущеної кінцевої точки RPC, яка буде використовуватися за замовчуванням для цього проекту. Цей вузол RPC повинен бути вузлом архіву (мати стан повного ланцюга). Для цього посібника ми будемо використовувати значення за замовчуванням *"https://terra-columbus-5.beta.api.onfinality.io"*
- Автори (обов’язково): Введіть тут власника цього проекту SubQuery (наприклад, ваше ім’я!)
- Опис (необов’язково): ви можете надати короткий абзац про ваш проект, який описує, які дані він містить і що користувачі можуть з ними робити
- Версія (обов’язково): введіть користувацький номер версії або використовуйте стандартний (`1.0.0`)
- Ліцензія (обов’язково): надайте ліцензію на програмне забезпечення для цього проекту або прийміть стандартну (`Apache-2.0`)

Після завершення процесу ініціалізації ви побачите, що в каталозі створена папка з назвою вашого проекту. Вміст цього каталогу має бути ідентичним тому, що вказано в [Структурі каталогу](../create/introduction.md#directory-structure).

Нарешті, у каталозі проекту виконайте наступну команду, щоб встановити залежності нового проекту.

<CodeGroup> <CodeGroupItem title="YARN" active> ``` оболонка компакт-диск PROJECT_NAME установка yarn ``` </CodeGroupItem>
<CodeGroupItem title="NPM"> ``` оболонка компакт-диск PROJECT_NAME npm встановити ``` </CodeGroupItem> </CodeGroup>

## Внесення змін до проекту

У стартовому пакеті, який ви щойно ініціалізували, ми надали стандартну конфігурацію для вашого нового проекту. Ви в основному працюватимете з такими файлами:

1. Схема GraphQL в `schema.graphql`
2. Маніфест проекту в `project.yaml`
3. Картографування функціонує в каталозі ` src / mappings / `

Метою цього короткого посібника є адаптація стандартного стартового проекту, щоб почати індексацію всіх переказів із смарт-контракту bLuna.

### Оновлення файлу схеми GraphQL

Файл `schema.graphql` визначає різні схеми GraphQL. Завдяки тому, як працює мова запитів GraphQL, файл схеми по суті визначає форму ваших даних із SubQuery. Це чудове місце для початку, тому що воно дає змогу заздалегідь визначити кінцеву мету.

Ми збираємося оновити файл `schema.graphql`, щоб він виглядав так

```graphql
type Transfer @entity {
  id: ID! Поле # id завжди є обов’язковим і має виглядати так
  txHash: рядок!
  blockHeight: BigInt # Висота блоку передачі
  відправник: рядок! # Рахунок, з якого здійснюються перекази
  одержувач: Нитка! # Рахунок, на який здійснюються перекази
  кількість: рядок! # Сума, яка передається
}
```

**Важливо: коли ви вносите будь-які зміни до файлу схеми, переконайтеся, що ви повторно створили каталог типів. Зробіть це зараз.**

<CodeGroup> <CodeGroupItem title="YARN" active> ``` оболонка кодоген yarn ``` </CodeGroupItem>
<CodeGroupItem title="NPM"> ``` оболонка npm run-script codegen ``` </CodeGroupItem> </CodeGroup>

Згенеровані моделі можна знайти в каталозі `/src/types/models`. Щоб отримати додаткові відомості про файл `schema.graphql`, перегляньте нашу документацію в розділі [Build/GraphQL Schema](../build/graphql.md)

### Оновлення файлу маніфесту проекту

Файл маніфесту проекту (`project.yaml`) можна розглядати як точку входу до вашого проекту, і він визначає більшість деталей про те, як SubQuery буде індексувати та перетворювати дані ланцюга.

Ми не будемо робити багато змін у файлі маніфесту, оскільки він уже налаштований правильно, але нам потрібно змінити наші обробники. Пам’ятайте, що ми плануємо індексувати всі події передачі Terra, тому нам потрібно оновити розділ ` datasources `, щоб прочитати наступне.

```yaml
відображення:
      файл: ./dist/index.js
      обробники:
        - обробник: handleEvent
          вид: terra/EventHandler
          # це спрацює для всіх подій, які відповідають наступній умові фільтра смарт-контракту
          фільтр:
            тип: передача
            messageFilter:
              тип: /terra.wasm.v1beta1.MsgExecuteContract
              значення:
                # Ми підписуємось на смарт-контракт bLuna (наприклад, передаємо лише події з цього контракту)
                договір: terra1j66jatn3k50hjtg2xemnjm8s7y8dws9xqa5y8w
```

Це означає, що ми запускатимемо функцію відображення `handleEvent` кожного разу, коли буде подія `transfer` зі смарт-контракту bLuna.

Щоб отримати додаткові відомості про файл маніфесту проекту (`project.yaml`), перегляньте нашу документацію в розділі [Файл збірки/маніфесту](../build/manifest/polkadot.md)

### Додайте функцію відображення

Функції відображення визначають, як дані ланцюга перетворюються в оптимізовані сутності GraphQL, які ми раніше визначили у файлі `schema.graphql`.

Перейдіть до функції відображення за замовчуванням у каталозі `src/mappings`. Ви побачите три експортовані функції: `handleBlock`, `handleEvent` і `handleCall`. Ви можете видалити як функції `handleBlock`, так і `handleCall`, ми маємо справу лише з функцією `handleEvent`.

Функція `handleEvent` отримує дані про події щоразу, коли подія відповідає фільтрам, які ми вказали раніше в нашому `project.yaml`. Ми збираємося оновити його, щоб обробити всі події `transfer` та зберегти їх у сутності GraphQL, які ми створили раніше.

Ви можете оновити функцію `handleEvent` до наступного (зверніть увагу на додатковий імпорт):

```ts
import { TerraEvent } з "@subql/types-terra";
import { Transfer } з "../types";
import { MsgExecuteContract } з (приховано)
export асинхронну функцію handleEvent(
  event: TerraEvent<MsgExecuteContract>
): Promise <void> {
    // Друк налагоджувальних даних із події
    // logger.info(JSON.stringify(event));

    // Створити нову сутність передачі з унікальним ідентифікатором
    const transfer = new Transfer(
      `${event.tx.tx.txhash}-${event.msg.idx}-${event.idx}`
    );
    transfer.blockHeight = BigInt(event.block.block.block.header.height);
    transfer.txHash = event.tx.tx.txhash;
    for (const attr of event.event.attributes) {
      switch (attr.key) {
        case "sender":
          transfer.sender = attr.value;
          break;
        case "recipient":
          transfer.recipient = attr.value;
          break;
        case "amount":
          transfer.amount = attr.value;
          break;
        default:
      }
    }
    очікування transfer.save();
}
```

Це отримує SubstrateEvent, який включає дані передачі корисного навантаження. Ми витягуємо ці дані, а потім створюємо новий об’єкт `Transfer`, який ми визначили раніше у файлі `schema.graphql`. Ми додаємо додаткову інформацію, а потім використовуємо функцію `.save()` для збереження нової сутності (SubQuery автоматично збереже це в базі даних).

Щоб отримати додаткові відомості про функції відображення, перегляньте нашу документацію в розділі [Build/Mappings](../build/mapping/polkadot.md)

### Створіть проект

Щоб запустити ваш новий проект SubQuery, нам спочатку потрібно створити нашу роботу. Запустіть команду збірки з кореневого каталогу проекту.

<CodeGroup> <CodeGroupItem title="YARN" active> ``` оболонка побудова yarn ``` </CodeGroupItem> <CodeGroupItem title="NPM"> ``` оболонка npm run-script build ``` </CodeGroupItem> </CodeGroup>

**Важливо: щоразу, коли ви вносите зміни у свої функції відображення, вам потрібно буде перебудувати свій проект**

## Запуск і запити вашого проекту

### Запустіть свій проект за допомогою Docker

Кожного разу, коли ви створюєте новий проект SubQuery, ви завжди повинні запускати його локально на своєму комп’ютері, щоб спочатку перевірити його. Найпростіший спосіб зробити це за допомогою Docker.

Уся конфігурація, яка керує запуском вузла SubQuery, визначена в цьому файлі `docker-compose.yml`. Для нового проекту, який щойно ініційовано, вам не потрібно нічого змінювати тут, але ви можете прочитати більше про файл і налаштування в нашому розділі [Запуск проекту](../run_publish/run.md)

У каталозі проекту виконайте таку команду:

<CodeGroup> <CodeGroupItem title="YARN" active> ``` оболонка початок yarn ``` </CodeGroupItem> <CodeGroupItem title="NPM"> ``` оболонка npm run-script start:docker ``` </CodeGroupItem> </CodeGroup>

Завантаження необхідних пакетів може зайняти деякий час ([`@subql/node`](https://www.npmjs.com/package/@subql/node),

`@subql/query`</7 > і Postgres) вперше, але незабаром ви побачите запущений вузол SubQuery. Будьте терплячі тут.</p> 



### Запитуйте свій проект

Відкрийте свій браузер і перейдіть до [http://localhost:3000](http://localhost:3000).

Ви повинні побачити, що ігровий майданчик GraphQL відображається в провіднику та схеми, які готові до запиту. У верхньому правому куті ігрового майданчика ви знайдете кнопку _ Docs _, яка відкриє розіграш документації. Ця документація генерується автоматично і допомагає вам знайти, які сутності та методи ви можете запитувати.

Для нового початкового проекту SubQuery ви можете спробувати такий запит, щоб зрозуміти, як він працює, або [дізнатися більше про мову Query GraphQL](../run_publish/graphql.md).



```graphql
{
  запит {
    перекази (
      перший: 10,
      замовлення за: ID_DESC
    ) {
      вузли {
        id
        txHash
        сума
        blockHeight
        відправник
        одержувач
      }
    }
  }
}
```




### Опублікуйте проект SubQuery

SubQuery надає безкоштовну керовану службу, коли ви можете розгорнути свій новий проект. Ви можете розгорнути його в [SubQuery Projects](https://project.subquery.network) і зробити запит за допомогою нашого [ Explorer](https://explorer.subquery.network).

[Прочитайте посібник, щоб опублікувати свій новий проект у SubQuery Projects](../run_publish/publish.md)



## Настуні кроки

Вітаємо, тепер у вас є локально запущений проект SubQuery, який приймає запити GraphQL API для передачі даних з bLuna.

Тепер, коли ви зрозуміли, як створити базовий проект SubQuery, виникає питання, куди звідси? Якщо ви почуваєтеся впевнено, ви можете перейти до вивчення трьох ключових файлів. Файл маніфесту, схема GraphQL і файл зіставлення в розділі [Створення цих документів](../build/introduction.md).

В іншому випадку перейдіть до нашого [розділу Академія](../academy/academy.md), де є докладніші семінари, навчальні посібники та приклади проектів. Там ми розглянемо більш просунуті модифікації та глибше зануримось у запуск проектів SubQuery, запустивши легкодоступні проекти з відкритим кодом.

Нарешті, якщо ви шукаєте більше способів запустити та опублікувати свій проект, наш [розділ «Виконати та опублікувати»](../run_publish/run.md) надає детальну інформацію про всі способи запуску проекту SubQuery та інші розширені функції агрегації та підписки GraphQL.
