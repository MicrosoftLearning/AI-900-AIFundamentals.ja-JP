---
title: Online Hosted Instructions
permalink: index.html
layout: home
---

# Azure AI の基礎コースの演習

このリポジトリには、Microsoft のコース [AI-900 *Microsoft Azure Artificial Intelligence (Microsoft Azure の人工知能)*](https://docs.microsoft.com/ja-jp/learn/certifications/courses/ai-900t00) のハンズオン ラボの演習と、自分のペースで進められる Microsoft Learn の次のモジュールが含まれています。[Azure で人工知能を使ってみる](https://docs.microsoft.com/learn/paths/get-started-with-artificial-intelligence-on-azure/)、[Azure Machine Learning を使用したコードなしの予測モデルの作成](https://docs.microsoft.com/ja-jp/learn/paths/create-no-code-predictive-models-azure-machine-learning/)、[Microsoft Azure でのコンピューター ビジョンを調べる](https://docs.microsoft.com/learn/paths/explore-computer-vision-microsoft-azure/)、[自然言語処理について調べる](https://docs.microsoft.com/learn/paths/explore-natural-language-processing/)、[Conversational AI を調べる](https://docs.microsoft.com/learn/paths/explore-conversational-ai/)。演習は学習教材に沿って設計されており、教材で説明されているテクノロジを使って学習できます。 

演習を完了するには、Microsoft Azure のサブスクリプションが必要です。講師からサブスクリプションが提供されていない場合は、[https://azure.microsoft.com](https://azure.microsoft.com) で無料の試用版にサインアップできます。

{% assign labs = site.pages | where_exp:"page", "page.url contains '/instructions'" %}
| 演習 |
| ------- | 
{% for activity in labs  %}| [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
