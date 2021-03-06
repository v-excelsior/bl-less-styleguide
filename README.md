# Less Style Guide

## Навигация

[Документация Less](https://lesscss.org/features)

1. [CSS](#css)
    - [Форматирование](#Форматирование)
    - [Комментарии](#Комментарии)
    - [Именование](#Именование)
    - [Селекторы id](#Селекторы-id)
    - [Свойство !important](#important)
2. [Less](#less)
    - [Синтаксис](#Синтаксис)
    - [Сортировка свойств](#Сортировка-свойств)
    - [Переменные](#Переменные)
    - [Миксины](#Миксины)
    - [Директива extend](#Директива-extend)
    - [Вложенные селекторы](#Вложенные-селекторы)

## CSS

### Форматирование

* Используйте табы (2 пробела) для отступов
* Kebab-case предпочтительнее camelCase в названии классов.
    - Snake_case и PascalCase можно использовать при работе с BEM (подробнее [OOCSS и BEM](#oocss-и-bem)).
* Не используйте селекторы #id
* При использовании нескольких селекторов для одного правила - каждое начинается с новой строки.
* Перед `{` ставим пробел в объявлении правила
* В свойства после `:` добавляем пробел.
* В конце объявления правила символ - `}`, должен переносится на новую строку.
* Между объявлениями правил добавляйте пустую строку.

**Плохо**

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

**Хорошо**

```css
.avatar {
  border-radius: 50%;
  border: 2px solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```

### Комментарии

* Предпочитайте однострочные комментарии (`// в Less`) многострочным.
* Комментарий должен находится на отдельной строке. Избегайте комментариев в конце строки.
* Пишите детальные комментарии для неочевидных вещей:
    - Для z-index
    - CSS-хаков для отдельных браузеров и совместимости

### Именование

Мы поощеряем некоторые комбинации OOCSS и BEM, вот почему:

* Это позволяет создавать чистую и строгую связь между CSS и HTML
* Это помогает нам создавать многоразовые, составные компоненты
* Меньше вложенностей, низкая специфичность правил.
* Это помогает создавать масштабируемые таблицы стилей

[OOCSS wiki](https://github.com/stubbornella/oocss/wiki) | [Original BEM Docs](https://ru.bem.info/methodology/css/)


Мы рекомендуем вариант БЭМ, в котором используются PascalCased "блоки". Подчеркивания и тире по-прежнему используются для модификаторов и элементов.

**Пример**

```css
.ListingCard { }
.ListingCard--featured { }
.ListingCard__title { }
.ListingCard__content { }
```

* `.ListingCard` — это "блок" и он представляет из себя компонент верхнего уровня.
* `.ListingCard__title` — это "элемент" и он является дочерним компонентом `.ListingCard`. Из "элементов" состоит "блок".
* `.ListingCard--featured` — это "модификатор" и он предствляет различные состояния "блока" `.ListingCard`.

### Селекторы id

Хоть в CSS и можно находить элемент по ID, это является плохой практикой. Селекторы ID вводят излишне высокий уровень [специфичности](https://developer.mozilla.org/ru/docs/Web/CSS/Specificity) для ваших правил, и лишает возможности их реиспользовать.

### important

Используйте `!important` только в исключительных случаях или для переопределения уже существующего `!important`. Частое использование этого  свойста услоняет поддержку написаных ранее вами стилей.

Если у вас возникают с этим сложности, ознакомьтесь детальнее со [специфичностью](https://developer.mozilla.org/ru/docs/Web/CSS/Specificity)

Так же можете использовать в работе [калькулятор](https://specificity.keegan.st/) специфичности

**[⬆ К навигации](#Навигация)**

## Less

### Синтаксис

* Упорядочивайте обычный CSS и `@include`-объявления логически.

### Сортировка свойств

1. Объявление свойств

   Перечисляйте максимальное количество стандартных свойств используя кастомный css а не встроенный редактор(Grapes), это нужно для переиспользования вашых стилей, и отстутсвия проблем спецификчности с inline стилями.

    ```less
    .btn-green {
      background: green;
      font-weight: bold;
      // ...
    }
    ```

2. Вложенные селекторы

   Вложенные селекторы, (_если они небходимы_), идут в самом конце, и ничего не должно идти после них. Добавляйте пустую строку между свойствами правила и вложенными селекторами, а также между смежными вложенными селекторами. Применяйте эти правила ко всем вложенным селекторам.

    ```less
    .btn {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);

      .icon {
        margin-right: 10px;
      }
    }
    ```

### Переменные

Мы советуем использовать переменные как можно чаще, это позволяет делать ваши стили более легкими в поддержке. При создании проекта можно описать в отдельном файле все ваши переменные в виде css-токенов, т.е. набора переменных которые связаны между собой по смыслу. Пример:

```less
@surface: ...
@background: ...

@primary: ...
@secondary: ...
@accent: ...

@color-on-primary: ...
@color-on-secondary: ...

@border-primary: ...
```

Любая строка это валидная переменная Less, это значит что вы так же можете в переменные записывать что-то более сложное чем свет: `@shadow: 10px 5px 5px red;`

Предпочитайте kebab-case именование переменных (напр. `@my-variable`) camelCase или snake_cased именованию. Допускается использовать знак нижнего подчеркивания для обозначения переменной используемой в пределах одного файла (напр. `@_my-variable`).

### Миксины

Миксины делают ваш код более чистым и понятным и позволяют поддерживать абстрактную сложность, так же как и хорошо названные функции.

[Mixins in Docs](https://lesscss.org/features/#mixins-feature)

### Директива extend

`:extend()` следует избегать, поскольку он имеет неинтуитивное и потенциально опасное поведение, особенно при использовании с вложенными селекторами. Даже наследование селекторов верхнего уровня может создать проблемы, если в будущем будет изменён порядок селекторов. Сжатие компенсирует экономию, которую вы получили бы с помощью наследования.

### Вложенные селекторы

**Вложенность селекторов не должна быть больше 3!**

Допустимо увелечение вложености до 4, для переопределения стилей из нашего UI SDK, методом упаковки всех стилей в один wrapper - `bl-page`

```less
.bl-page {
  .first {
    .second {
      .third {
        // Стоп!
      }
    }
  }
  
  .anotherFirst {
    .second {
      //
    }
  }
}
```

Когда вы пишите такие длинные селекторы, вы вероятно пишите CSS который:

* Слишком сильно привязан к HTML (хрупкий) *—или—*
* Чрезмерно специфичный *—или—*
* Не реиспользуемый


Еще раз: **никаких вложенных ID селекторов!**

Если все-таки пришлось использовать селекторы ID (а этого на самом деле лучше избегать), они никогда не должны быть вложенными. Если такое происходит, вам требуется пересмотреть свою разметку и выяснить, зачем нужна такая сильная специфика. Если у вас правильно структурирован HTML-код и CSS-стили, вам **никогда** не придется это делать.

**[⬆ К навигации](#Навигация)**
