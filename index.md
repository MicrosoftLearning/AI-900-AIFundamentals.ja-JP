---
title: オンラインでホスティングされている手順
permalink: index.html
layout: home
---

# <a name="azure-ai-fundamentals-exercises"></a>Azure AI の基礎コースの演習

これらの実践的な演習は、[Microsoft Learn](https://docs.microsoft.com/training/) のトレーニング コンテンツをサポートするように設計されています。

To complete these exercises, you'll need a Microsoft Azure subscription. You can sign up for a free trial at <bpt id="p1">[</bpt><ph id="ph1">https://azure.microsoft.com</ph><ept id="p1">](https://azure.microsoft.com)</ept>.

{% assign labs = site.pages | where_exp:"page", "page.url contains '/instructions'" %}
| 演習 |
| ------- | 
{% for activity in labs %}| [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
