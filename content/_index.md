---
title: Smart Tabs
meta_description: Browser extension — tidy tabs, fast search, and saved sessions without clutter.

hero:
  eyebrow: For Chrome, Edge, Brave, and other Chromium-based browsers
  title: Smart Tabs
  subtitle: >
    Find the right tab in seconds, group work by task, and return to saved sessions —
    without a noisy UI or extra clicks.
  preview:
    image: /images/smart-tabs-hero.png
    width: 3434
    height: 1782
    alt: Smart Tabs panel — tabs grouped by site, search bar, and widgets
    caption: Group by domain, search, quick links, and widgets
  primary_label: Install
  secondary_label: See features
  secondary_anchor: "#features"
  widgets_label: Widgets
  widgets_anchor: "#widgets"

highlights:
  - Search open tabs by title or URL
  - Groups for projects and contexts
  - Sessions and a calm, minimal UI

features:
  lead: Everything you need to stay calm with dozens or hundreds of tabs — in one extension panel.
  items:
    - title: Smart grouping
      description: Group tabs by meaning — project, research, learning. Less chaos in the tab strip.
    - title: Instant search
      description: Type part of a title or URL — the list narrows to what matters. No manual hunting.
    - title: Sessions and snapshots
      description: Save sets of tabs and reopen them when the task comes back.
    - title: Minimal noise
      description: A clean UI and predictable behaviour — focus on the task, not the extension.

widgets:
  title: Widgets
  lead: Example data sources for widgets — PromQL, public APIs, and geocoding + weather.
  items:
    - title: Prometheus metric
      description: Prometheus (PromQL) query and parsing the response for a numeric widget.
      name: Metric (e.g. req rate)
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
    - title: BTC price (Coinbase)
      description: Bitcoin spot price and formatting with a colour threshold.
      name: BTC price
      link: https://api.coinbase.com/v2/prices/BTC-USD/spot
      code: |
        const res = await fetchData('https://api.coinbase.com/v2/prices/BTC-USD/spot');
        const data = JSON.parse(res);
        const price = Math.round(Number(data.data.amount));
        const formatted = price >= 1000 ? Math.round(price / 1000) + 'k' : price;
        return { value: '$' + formatted, label: 'BTC', color: price >= 80000 ? '#22c55e' : '#ef4444' };
    - title: Weather by city
      description: Open-Meteo geocoding and current forecast — temperature and “feels like”.
      name: City weather
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
  title: Trust and transparency
  lead: The extension runs in your browser; publish policy details when you ship.
  items:
    - icon: lock
      title: Your data stays with you
      description: Describe how page content is handled — users expect a clear privacy policy.
    - icon: browsers
      title: Chromium family
      description: Built for Chrome and other Chromium browsers. Safari/Firefox when you add support.
    - icon: shield
      title: Openness
      description: Publish a policy and contact — it helps people trust an extension from the store.

install:
  title: Ready to try Smart Tabs?
  subtitle: One-click install from the official extension store.
  button_label: Install extension
---
