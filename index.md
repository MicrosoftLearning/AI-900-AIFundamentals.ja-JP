---
title: オンラインでホスティングされている手順
permalink: index.html
layout: home
ms.openlocfilehash: b85af520a10e63a2f9a5696db03bfd946aff968f
ms.sourcegitcommit: 1ef64e3008a439d0d0bb3d93a27d3df68d3d64a9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/17/2022
ms.locfileid: "140688694"
---
# <a name="azure-ai-fundamentals-exercises"></a>Azure AI の基礎コースの演習

以下のリンクを使用して、Microsoft コース [AI-900 *Microsoft Azure AI の基礎*](https://docs.microsoft.com/learn/certifications/courses/ai-900t00) のハンズオン ラボ演習を完了してください。

演習を完了するには、Microsoft Azure のサブスクリプションが必要です。 講師からサブスクリプションが提供されていない場合は、[https://azure.microsoft.com](https://azure.microsoft.com) で無料の試用版にサインアップできます。

{% assign labs = site.pages | where_exp:"page", "page.url contains '/instructions'" %}
| 演習 |
| ------- | 
{% for activity in labs  %}| [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
