# Smart Tabs — лендинг на Hugo

Одностраничный сайт для расширения **Smart Tabs**: герой, возможности, виджеты с примерами кода (подсветка через Chroma), блок доверия, CTA. Статическая сборка, готова к публикации на GitHub Pages.

## Требования

- [Hugo Extended](https://gohugo.io/installation/) (нужен для SCSS в `assets/scss/`). Проверка: `hugo version` — в выводе должно быть `extended`.

## Локальная разработка

```bash
git clone <url> && cd smart-tabs-site
hugo server -D
```

Сайт: [http://localhost:1313/](http://localhost:1313/) (с автопересборкой при изменениях).

## Сборка

Выполняется очень просто

```bash
hugo --gc --minify
```

Результат в каталоге **`public/`** — его можно отдавать любым статическим хостингом.

## Где что править

| Что | Где |
|-----|-----|
| Тексты, блоки героя, фичи, виджеты, доверие, футер | [`content/_index.md`](content/_index.md) (front matter в YAML) |
| `baseURL`, ссылки на Store и политику, описание сайта | [`hugo.toml`](hugo.toml) → `[params]` и корень файла |
| Вёрстка главной страницы | [`layouts/index.html`](layouts/index.html) |
| Карточка виджета (форма + код) | [`layouts/partials/widget-editor.html`](layouts/partials/widget-editor.html) |
| Глобальные стили | [`assets/scss/main.scss`](assets/scss/main.scss) |
| Подсветка кода (тема Chroma) | [`hugo.toml`](hugo.toml) → `[markup.highlight]` и файл [`assets/css/chroma-github-dark.css`](assets/css/chroma-github-dark.css) (можно пересобрать: `hugo gen chromastyles --style=github-dark > assets/css/chroma-github-dark.css`) |
| Картинки для героя | поле `hero.preview` в `_index.md` и файлы в **`static/images/`** |

Параметры вроде `store_url`, `privacy_url`, `contact_email` задаются в `hugo.toml` и подхватываются шаблоном.

## GitHub Pages

В репозитории включите **Settings → Pages → Source: GitHub Actions**. Workflow [`.github/workflows/hugo-pages.yml`](.github/workflows/hugo-pages.yml) собирает сайт на push в `main` или `master` и публикует артефакт. При сборке в CI передаётся корректный `--baseURL` для Pages.

## Структура (кратко)

```
content/_index.md    # контент главной
layouts/             # шаблоны
assets/scss/         # стили (обрабатывает Hugo)
assets/css/          # Chroma для подсветки кода
static/              # статика как есть (изображения и т.д.)
public/              # результат сборки (в .gitignore)
```
