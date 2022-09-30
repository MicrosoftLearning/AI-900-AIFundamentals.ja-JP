---
title: オンラインでホスティングされている手順
permalink: index.html
layout: home
ms.openlocfilehash: 86fa2da9bfa9e4d7edb0c853f77e95fe69e97411
ms.sourcegitcommit: 3fc8440b350b498c2f50ab4ce531a0f906792584
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/08/2022
ms.locfileid: "147866008"
---
# <a name="azure-ai-fundamentals-exercises"></a>Azure AI の基礎コースの演習

これらの実践的な演習は、[Microsoft Learn](https://docs.microsoft.com/training/) のトレーニング コンテンツをサポートするように設計されています。

演習を完了するには、Microsoft Azure のサブスクリプションが必要です。 [https://azure.microsoft.com](https://azure.microsoft.com) で無料試用版にサインアップできます。

{% assign labs = site.pages | where_exp:"page", "page.url contains '/instructions'" %}
| 演習 |
| ------- | 
{% for activity in labs %}| [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
