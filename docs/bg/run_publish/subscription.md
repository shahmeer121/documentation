# Абонаменти

## Какво е абонамент за GraphQL

SubQuery вече поддържа и Graphql абонаменти. Подобно на заявките, абонаментите ви позволяват да извличате данни. За разлика от заявките, абонаментите са дълготрайни операции, които могат да променят резултата си с течение на времето.

Абонаментите са много полезни, когато искате вашето клиентско приложение да промени данни или да покаже някои нови данни веднага щом тази промяна настъпи или новите данни са налични. Subscriptions allow you to _subscribe_ to your SubQuery project for changes.

::: info Note Read more about [Subscriptions](https://www.apollographql.com/docs/react/data/subscriptions/). :::

## Как да се абонирам за обект

Основният пример за абонамент за GraphQL е да бъдете уведомени, когато се създават нови обекти. В следващия пример се абонираме за обекта `Transfer` и получаваме актуализация, когато има промени в тази таблица.

Можете да създадете абонамента, като направите запитване към GraphQL ендпойнта, както следва. След това вашата връзка ще се абонира за всички промени, направени в таблицата с обекти `Transfer`.

```graphql
subscription {
  transfer {
    id
    mutation_type
    _entity
  }
}
```

Тялото на обекта във вашата заявка показва какви данни искате да получавате чрез абонамента си, когато таблицата `Transfer` се актуализира:

- `id`: Връща идентификатора на обекта, който е променен.
- `mutation_type`: Действието, което е извършено към този обект. Типовете мутации могат да бъдат `INSERT`, `UPDATE` или `DELETE`.
- `_entity`: стойността на самия обект във формат JSON.

## Филтриране

Ние също така поддържаме филтър за абонаменти, което означава, че клиентът трябва да получава актуализирани данни за абонамента само ако тези данни или мутация отговарят на определени критерии.

Има два вида филтри, които поддържаме:

- `id` : Филтрирайте, за да върнете само промени, които засягат конкретен обект (означен от идентификатора).
- `mutation_type`: Само същият тип мутация, който е направен, ще върне актуализация.

Да приемем, че имаме обект `Balances` и той записва салдото на всяка сметка.

```graphql
type Balances {
  id: ID! # нечий акаунт, напр. 15rb4HVycC1KLHsdaSdV1x2TJAmUkD7PhubmhL3PnGv7RiGY
  amount: Int! # салдото по тази сметка
}
```

Ако искаме да се абонираме за актуализации на баланса, които засягат конкретна сметка, можем да посочим филтъра за абонамент, както следва:

```graphql
subscription {
  balances(
    id: "15rb4HVycC1KLHsdaSdV1x2TJAmUkD7PhubmhL3PnGv7RiGY"
    mutation: UPDATE
  ) {
    id
    mutation_type
    _entity
  }
}
```

Имайте предвид, че филтърът за `mutation` може да бъде един от `INSERT`, `UPDATE` или `DELETE`.

::: warning Important Please note that you must enable the `--subscription` flag on both the node and query service in order to use these functions. :::

::: warning Important
The subcription feature works on SubQuery's Managed Service when you directly call the listed GraphQL endpoint. Няма да работи в рамките на игралната площадка на GraphQL в браузъра.
:::
