---
title: オンラインでホスティングされている手順
permalink: index.html
layout: home
---

# <a name="azure-ai-fundamentals-exercises"></a>Azure AI の基礎コースの演習

これらの実践的な演習は、[Microsoft Learn](https://docs.microsoft.com/training/) のトレーニング コンテンツをサポートするように設計されています。

演習を完了するには、Microsoft Azure のサブスクリプションが必要です。 [https://azure.microsoft.com](https://azure.microsoft.com) で無料試用版にサインアップできます。

## <a name="labs"></a>ラボ

{% assign labs = site.pages | where_exp:"page", "page.url contains '/instructions'" %}
| モジュール | ラボ |
| --- | --- | 
{% for activity in labs %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}

