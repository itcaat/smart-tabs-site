---
title: Smart Tabs
meta_description: Расширение для браузера — порядок во вкладках, быстрый поиск и сохранённые сессии без лишнего шума.

hero:
  eyebrow: Для Chrome, Edge, Brave и других Chromium-браузеров
  title: Smart Tabs
  subtitle: >
    Находите нужную вкладку за секунды, группируйте задачи и возвращайтесь к сохранённым сессиям —
    без перегруженного интерфейса и лишних кликов.
  preview:
    image: /images/placeholder-hero.svg
    alt: Интерфейс Smart Tabs — обзор панели и списка вкладок
    caption: Обзор интерфейса
  primary_label: Установить
  primary_anchor: "#install"
  secondary_label: Смотреть возможности
  secondary_anchor: "#features"

highlights:
  - Поиск по заголовку и адресу среди открытых вкладок
  - Группы под проекты и контексты работы
  - Сессии и аккуратный UI без отвлечений

features:
  lead: Всё необходимое для спокойной работы с десятками и сотнями вкладок — в одной панели расширения.
  items:
    - title: Умная группировка
      description: Собирайте вкладки по смыслу — проект, исследование, обучение. Меньше хаоса в строке вкладок.
    - title: Мгновенный поиск
      description: Введите часть заголовка или URL — список сузится до релевантного. Без перебора вручную.
    - title: Сессии и снимки
      description: Сохраняйте наборы вкладок и открывайте их снова, когда задача возвращается.
    - title: Минимум шума
      description: Чистый интерфейс и предсказуемое поведение — фокус на задаче, а не на расширении.

widgets:
  title: Виджеты
  lead: Примеры источников данных для виджетов — PromQL, публичные API и геокодинг + погода.
  items:
    - title: Метрика через Prometheus
      description: Запрос к Prometheus (PromQL) и разбор ответа для числового виджета.
      name: Метрика (напр. Req rate)
      link: ""
      code: |
        const query = 'sum(increase(ota_apps_search_request_api_total[1m]))';
        const res = await fetchData(
          'http://prometheus?query=' + encodeURIComponent(query)
        );
        const data = JSON.parse(res);
        const val = data.data.result[0]?.value[1];
        const rounded = Math.round(Number(val));
        return { value: rounded, label: 'Reqs/5m', color: '#10b981' };
    - title: Курс BTC (Coinbase)
      description: Спотовая цена Bitcoin и форматирование с цветом по порогу.
      name: Курс BTC
      link: https://api.coinbase.com/v2/prices/BTC-USD/spot
      code: |
        const res = await fetchData('https://api.coinbase.com/v2/prices/BTC-USD/spot');
        const data = JSON.parse(res);
        const price = Math.round(Number(data.data.amount));
        const formatted = price >= 1000 ? Math.round(price / 1000) + 'k' : price;
        return { value: '$' + formatted, label: 'BTC', color: price >= 80000 ? '#22c55e' : '#ef4444' };
    - title: Погода по городу
      description: Геокодинг Open-Meteo и прогноз current — температура и «ощущается как».
      name: Погода в городе
      link: ""
      code: |
        const city = 'St Petersburg';
        const geoRes = await fetchData(
          'https://geocoding-api.open-meteo.com/v1/search?name=' + encodeURIComponent(city) + '&count=1'
        );
        const geo = JSON.parse(geoRes);
        if (!geo.results?.length) {
          return { value: '—', label: 'City not found', color: '#ef4444' };
        }
        const { latitude, longitude, name, country_code } = geo.results[0];
        const res = await fetchData(
          'https://api.open-meteo.com/v1/forecast?latitude=' +
            latitude +
            '&longitude=' +
            longitude +
            '&current=temperature_2m,apparent_temperature&timezone=auto'
        );
        const data = JSON.parse(res);
        const t = Math.round(data.current.temperature_2m);
        const feels = Math.round(data.current.apparent_temperature);
        return {
          value: t + '°C',
          label: name + ', ' + country_code + ' · feels ' + feels + '°',
          color: t > 15 ? '#f59e0b' : t > 0 ? '#22c55e' : '#3b82f6',
        };

trust:
  title: Надёжность и прозрачность
  lead: Расширение работает в вашем браузере; уточните детали политики при публикации.
  items:
    - icon: lock
      title: Данные под вашим контролем
      description: Контент открытых страниц не покидает браузер в обход ваших ожиданий — опишите модель в политике конфиденциальности.
    - icon: browsers
      title: Chromium-семейство
      description: Рассчитано на Chrome и другие браузеры на базе Chromium. Про Safari/Firefox — по мере поддержки.
    - icon: shield
      title: Открытость
      description: Опубликуйте политику и контакты — пользователям проще доверять расширению из магазина.

install:
  title: Готовы попробовать Smart Tabs?
  subtitle: Установка в один клик из официального магазина расширений.
  button_label: Установить расширение
  note: После публикации подставьте ссылку на страницу в Chrome Web Store в настройках сайта (параметр store_url).

footer:
  product: Smart Tabs
  tagline: Умная работа с вкладками в браузере.
  copyright: Smart Tabs
---
