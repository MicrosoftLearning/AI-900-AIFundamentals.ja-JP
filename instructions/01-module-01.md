---
lab:
  title: Cognitive Services を確認する
ms.openlocfilehash: 53615bbe4bc6de0237022e6f759e9fb6ffc1be13
ms.sourcegitcommit: b3c7eccbe099234cfb6bde0a93b141d185da965b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/26/2022
ms.locfileid: "147694267"
---
## <a name="explore-cognitive-services"></a>Cognitive Services を確認する

> **注** このラボを完了するには、管理者アクセス権が与えられている [Azure サブスクリプション](https://azure.microsoft.com/free?azure-portal=true)が必要です。

Azure Cognitive Services には、ビジョン、音声、言語、意思決定サービスの 4 つの主要な柱に分類できる一般的な AI 機能がカプセル化されています。 この演習では、ソフトウェア アプリケーションでコグニティブ サービス リソースをプロビジョニングして使用する方法の一般的な感覚を得るために、意思決定サービスの 1 つを見ていきます。

この演習で探索する特定のコグニティブ サービスは *Anomaly Detector* です。 Anomaly Detector は、時間の経過に伴うデータ値を分析するためや、問題や課題を示す可能性がある異常な値を、さらなる調査用に検出するために使用されます。 たとえば、温度管理された保管施設のセンサーは、毎分温度を監視し、測定値をログに記録する場合があります。 Anomaly Detector サービスを使用して、ログに記録された温度の値を分析し、予想される温度の正常な範囲より著しく外れる値にフラグを設定できます。

異常検出サービスの機能をテストするために、Cloud Shell で実行される単純なコマンドライン アプリケーションを使用します。 Web サイトや電話アプリなど、実際のソリューションにも同じ原則と機能が適用されます。

> **注** この演習の目的は、コグニティブ サービスのプロビジョニングの使用方法について一般的に理解することです。 Anomaly Detector を例として使用しますが、この演習では異常検出に関する包括的な知識を得ることは想定していません。

## <a name="create-an-anomaly-detector-resource"></a>*Anomaly Detector* リソースを作成する

Azure サブスクリプション内に **Anomaly Detector** リソースを作成することから始めましょう。

1. 別のブラウザー タブで Azure portal ([https://portal.azure.com](https://portal.azure.com?azure-portal=true)) を開き、Microsoft アカウントでサインインします。

1. **[&#65291;リソースの作成]** ボタンをクリックして、*[Anomaly Detector]* を検索し、以下の設定を使用して **Anomaly Detector** リソースを作成します。
    - **[サブスクリプション]**: *お使いの Azure サブスクリプション*。
    - **[リソース グループ]**: 既存のリソース グループを選択するか、新しいリソース グループを作成します。
    - **リージョン**: 使用できるリージョンを選択します
    - **[名前]**: *一意の名前を入力します*。
    - **価格レベル**: Free F0

1. リソースを確認して作成します。 デプロイが完了するまで待ち、デプロイされたリソースに移動します。

1. その Anomaly Detector リソースの **[キーとエンドポイント]** ページを表示します。 クライアント アプリケーションから接続するには、エンドポイントとキーが必要です。

## <a name="run-cloud-shell"></a>Cloud Shell の実行

Anomaly Detector サービスの機能をテストするために、Azure の Cloud Shell で実行される単純なコマンドライン アプリケーションを使用します。

1. Azure portal で、ページの上部の検索ボックスの右側にある **[>_]** (*Cloud Shell*) ボタンを選択します。 これにより、ポータルの下部に Cloud Shell ペインが開きます。

    ![上部の検索ボックスの右側にあるアイコンをクリックして、Cloud Shell を起動します](media/anomaly-detector/powershell-portal-guide-1.png)

1. Cloud Shell を初めて開くと、使用するシェルの種類 (*Bash* または *PowerShell*) を選択するように求められる場合があります。 **[PowerShell]** を選択します。 このオプションが表示されない場合は、このステップをスキップします。  

1. Cloud Shell のストレージを作成するように求めるメッセージが表示された場合は、サブスクリプションが指定されていることを確認して、**[ストレージの作成]** を選択します。 その後、ストレージが作成されるのを 1 分程度待ちます。

    ![確認をクリックしてストレージを作成します。](media/anomaly-detector/powershell-portal-guide-2.png)

1. [Cloud Shell] ペインの左上に表示されるシェルの種類が *PowerShell* に切り替えられたことを確認します。 *Bash* の場合は、ドロップダウン メニューを使用して *PowerShell* に切り替えます。

    ![PowerShell に切り替えるための左側のドロップダウン メニューを見つける方法](media/anomaly-detector/powershell-portal-guide-3.png)

1. PowerShell が起動するまで待ちます。 Azure portal に次の画面が表示されます。  

    ![PowerShell が起動するまで待ちます。](media/anomaly-detector/powershell-prompt.png)

## <a name="configure-and-run-a-client-application"></a>クライアント アプリケーションを構成して実行する

Cloud Shell 環境を用意できたので、Anomaly Detector サービスを使用してデータを分析する単純なアプリケーションを実行できます。

1. コマンド シェルで、次のコマンドを入力してサンプル アプリケーションをダウンロードし、ai-900 というフォルダーに保存します。

    ```PowerShell
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    ```

    >**ヒント** 別のラボで既にこのコマンドを使用して *ai-900* リポジトリを複製した場合は、この手順をスキップできます。

1. ファイルが **ai-900** という名前のフォルダーにダウンロードされます。 次に、Cloud Shell ストレージ内のすべてのファイルを表示して、それらを使用します。 シェルに次のコマンドを入力します。

     ```PowerShell
    code .
    ```

    これにより、次の図のようなエディターが開きます。 

    ![コード エディター。](media/anomaly-detector/powershell-portal-guide-4.png)

1. 左側の **[ファイル]** ペインで、**[ai-900]** を展開し、**[detect-anomalies.ps1]** を選択します。 このファイルには、次に示すように、異常検出サービスを使用するコードがいくつか含まれています。

    ![異常を検出するコードを含んでいるエディター](media/anomaly-detector/detect-anomalies-code.png)

1. コードの詳細についてあまり心配しないでください。重要なのは、エンドポイント URL と、Anomaly Detector リソースのいずれかのキーが必要であることです。 お使いのリソース (まだブラウザーの先頭領域にあるはずです) の **[キーとエンドポイント]** ページからこれらをコピーし、コード エディターに貼り付けて **YOUR_KEY** と **YOUR_ENDPOINT** プレースホルダーの値をそれぞれ置き換えます。

    > **ヒント** **[キーとエンドポイント]** および **[エディター]** ペインを操作するときに、区分線を使用して画面領域を調整しなければならないことがあります。

    キーとエンドポイントの値を貼り付けると、コードの最初の 2 行は次のようになります。

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."    
    $endpoint="https..."
    ```

1. エディター ペインの右上の **[...]** ボタンを使用してメニューを開き、**[保存]** を選択して変更を保存します。 次に、メニューを再度開き、**[エディターを閉じる]** を選択します。

    異常検出は、系列内の値が、予期されるパラメーター内にあるかどうかを判断するために使用される人工知能の手法です。 サンプル クライアント アプリケーションでは、Anomaly Detector サービスを使用して、一連の日付/時刻と数値が含まれるファイルが分析されます。 このアプリケーションでは、数値が予期されたパラメーター内にあるかどうかを各時点で示す結果を返す必要があります。

1. PowerShell ウィンドウで、次のコマンドを入力してコードを実行します。

    ```PowerShell
    cd ai-900
    .\detect-anomalies.ps1
    ```

1. 結果をよく調べます。結果の最後の列が、それぞれの日付/時刻に記録された値が異常であると考えられるかどうかを示す **True** または **False** であることに注目します。 実際の状況でこの情報を使用する方法を検討してください。 値が冷蔵庫の温度または血圧の値で、異常が検出された場合、アプリケーションではどのようなアクションがトリガーされますか?  

## <a name="learn-more"></a>詳細情報

この簡単なアプリでは、Anomaly Detector サービスの一部の機能のみを示しています。 このサービスで実行できることの詳細を確認するには、[Anomaly Detector のページ](https://azure.microsoft.com/services/cognitive-services/anomaly-detector/)を参照してください。
