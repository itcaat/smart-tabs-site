---
title: Smart Tabs
meta_description: Replace your new tab with a Smart Tabs dashboard — tabs grouped by site, widgets, Quick Dial, search, and dark mode.

hero:
  eyebrow: For Chrome, Edge, Brave, and other Chromium-based browsers
  title: Smart Tabs
  subtitle: >
    Smart Tabs replaces your new tab page with a sleek, powerful dashboard that brings order to your browser.
    It automatically groups all your open tabs by website, making it effortless to find, switch, or close tabs with a single click.
    Pin your go-to sites, search across all tabs, and enjoy a beautifully organized browsing experience — complete with dark mode and intuitive controls.
  preview:
    image: /images/smart-tabs-hero.png
    width: 3434
    height: 1782
    alt: Smart Tabs panel — tabs grouped by site, search bar, and widgets
    caption: New tab dashboard — grouped tabs, widgets, Quick Dial, and search
  primary_label: Install
  secondary_label: Features
  secondary_anchor: "#features"
  widgets_label: Widgets
  widgets_anchor: "#widgets"
  donate_label: Donate

highlights:
  - Create your own widgets and use the Quick Dial panel
  - Tabs grouped by site — search, pin, switch, or close in one place
  - Light and dark themes, duplicate cleanup, and more

features:
  section_title: Key features
  items:
    - title: Create your own widgets
      description: Build and arrange widgets on your new tab dashboard.
    - title: Quick Dial panel
      description: Fast access to the sites you use most.
    - title: Grouped by domain
      description: Open tabs are intelligently grouped by domain, giving you a clean, structured view of your browsing activity.
    - title: Favicons and color coding
      description: Each tab group shows the site’s favicon and is color-coded for faster identification.
    - title: Switch tabs instantly
      description: Jump to any open tab from the dashboard with a single click.
    - title: Close tabs or whole groups
      description: Close individual tabs or entire groups in one click.
    - title: Pin favorites
      description: Pin favorite domains or groups for easy access.
    - title: Search and filter
      description: Quickly find tabs by title, URL, or domain using the built-in search and filter tools.
    - title: Light and dark themes
      description: Toggle between light and dark themes to match your environment or preference.
    - title: Remove duplicates
      description: Clean up duplicate tabs so your list stays tidy.
    - title: Wake the cat
      description: Wake up the cat to show old visited tabs.

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
