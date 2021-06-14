---

layout: yandex2

style: |
    /* собственные стили можно писать здесь!! */

---

# ![](themes/yandex2/images/logo-{{ site.presentation.lang }}.svg){:.logo}

## {{ site.presentation.title }}
{:.title}

### ![](themes/yandex2/images/title-logo-{{ site.presentation.lang }}.svg){{ site.presentation.service }}

{% if site.presentation.nda %}
![](themes/yandex2/images/title-nda.svg)
{:.nda}
{% endif %}

<div class="authors">
{% if site.author %}
<p>{{ site.author.name }}{% if site.author.position %}, {{ site.author.position }}{% endif %}</p>
{% endif %}

{% if site.author2 %}
<p>{{ site.author2.name }}{% if site.author2.position %}, {{ site.author2.position }}{% endif %}</p>
{% endif %}

</div>

## Задание

- Асинхронное API с колбэками
- Нелинейная последовательность действий
- Представление событий в виде потоков

## Основные идеи

- {:.next}Если одно из составных действий — асинхронное, то вся операция тоже асинхронная
- {:.next}Потренироваться работать с API промисов
- {:.next}Потренироваться работать с вложенными промисами
- {:.next}Потренироваться использовать RxJS

## Промисификация

```js
function add2(a, b) {
    return new Promise((resolve) => {
        add(a, b, (result) => {
            resolve(result);</mark>
        });
    });
}
```

## Промисификация

```js
function add2(a, b) {
    return new Promise((resolve) => {
        add(a, b, resolve);
    })
}
```

## Промисификация

```js
function exec(fn, ...args) {
    return new Promise((resolve) => {
        fn(...args, resolve);
    })
}
```

## Промисификация

```js
function exec(fn, ...args) {
    return new Promise((resolve) => {
        fn(...args, resolve);
    })
}

// пример вызова
const result = await exec(add, 5, 3)
```

## поиск максимального элемента
{:.fullscreen}

```js
// поиск максимального элемента
async function max(a, cb) {
    const len = await exec(a.length);
    let max = await exec(a.get, 0);
    let index = 1;

    while (await exec(less, index, len)) {
        const num = await exec(a.get, index);

        if (await exec(less, max, num)) {
            max = num;
        }
        index = await exec(add, index, 1);
    }
    cb(max);
}
```

## finally
{:.fullscreen}

```js
// finally
Promise.prototype._finally = function (callback) {
    return this.then(
        value => Promise.resolve(callback()).then(() => value),
        reason => Promise.resolve(callback()).then(() => { throw reason })        
    )
};
```

## allSettled
{:.fullscreen}

```js
// allSettled
Promise.prototype._allSettled = function (list) {
    return Promise.all(
        list.map(item => 
            Promise.resolve(item)
            .then(value => ({ status: 'fulfilled', value: value }))
            .catch(reason => ({ status: 'rejected', reason: reason}))
        )
    );
};
```

## any
{:.fullscreen}

```js
// any
Promise.prototype._any = function (list) {
    return new Promise((resolve, reject) => {
        let errors = [], len = list.length;

        list.forEach((item, i) => {
            Promise.resolve(item).then(resolve, (reason) => {
                errors[i] = reason;
                len--;
                
                if (!len) { 
                    reject({ errors })
                }
            })
        });
    });
};
```

## Спасибо за внимание!
{:.section}

## Контакты 
{:.contacts}

{% if site.author %}

<figure markdown="1">

### {{ site.author.name }}

{% if site.author.position %}
{{ site.author.position }}
{% endif %}

</figure>

{% endif %}

{% if site.author2 %}

<figure markdown="1">

### {{ site.author2.name }}

{% if site.author2.position %}
{{ site.author2.position }}
{% endif %}

</figure>

{% endif %}

<!-- разделитель контактов -->
-------

<!-- left -->
<!-- - {:.skype}author -->
<!-- - {:.mail}author@yandex-team.ru -->
<!-- - {:.github}author -->

<!-- right -->
<!-- - {:.twitter}@author -->
<!-- - {:.facebook}author -->
- {:.telegram}@stromov

<!-- 

- {:.mail}author@yandex-team.ru
- {:.phone}+7-999-888-7766
- {:.github}author
- {:.bitbucket}author
- {:.twitter}@author
- {:.telegram}author
- {:.skype}author
- {:.instagram}author
- {:.facebook}author
- {:.vk}@author
- {:.ok}@author

-->
