В Strapi есть маркетплейс, на котором можно найти полезные плагины. Например, добавить в админку выбор цвета, выбор страны, поиск и выбор или иконок MUI и т.д.

По умолчанию Strapi не отдаёт информацию о таких данных, как Media, Relation, Component, Dynamic Zone (это всё - Relational Fields). Для того, чтобы их все было видно в API-ответе, в запрос нужно добавить "?populate=\*"

Для того, чтобы получить те поля, которые нам нужны, в т.ч. с глубокой вложенностью, нужно использовать [Interactive Query Builder](https://docs.strapi.io/cms/api/rest/interactive-query-builder). Там нужно указать endpoint и прописать то, что нам нужно. Например:

```
{
  populate: {
    blocks: {
      on: {
        "blocks.hero-section": {
          populate: {
            image: {
              fields: ["url", "alternativeText"],
            },
            logo: {
              populate: {
                image: {
                  fields: ["url", "alternativeText"],
                },
              },
            },
            cta: true,
          }
        }
      }
    }
  }
}
```

А для того, чтобы не использовать сайт, лучше установить пакет qs (и @types/qs), которые может обрабатывать такие объекты и делать из них ссылки:

```
const homePageQuery = qs.stringify({
  populate: {
    blocks: {
      ...
    },
  },
});

const result = await fetch(`http://localhost:1337/api/home-page/?${homePageQuery}`)
```

При создании новых страниц нужно:

- создать необходимую структуру данных в Content-Type Builder
- создать страницу в Content Manager
- в Settings/Roles разрешить доступ к данным этой страницы
