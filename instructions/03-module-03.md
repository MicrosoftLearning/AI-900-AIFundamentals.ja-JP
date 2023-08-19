---
lab:
  title: Computer Vision の詳細を確認する
---

# Computer Vision の詳細を確認する

> **注** このラボを完了するには、管理者アクセス権が与えられている [Azure サブスクリプション](https://azure.microsoft.com/free?azure-portal=true)が必要です。

この *Computer Vision* コグニティブ サービスでは、事前トレーニング済みの機械学習モデルを使用して画像を分析し、その画像に関する情報を抽出します。

たとえば、架空の小売業者 *Northwind Traders* が、"スマート ストア" の実装を決定したとします。AI サービスによって店舗を監視し、支援を必要とする顧客を特定し、従業員に支援を指示します。 Computer Vision サービスを使用すると、店舗全体のカメラによって撮影された画像を分析して、その画像の意味のある説明を提供できます。

このラボでは、単純なコマンド ライン アプリケーションを使用して、Computer Vision サービスが動作しているのを確認します。 Web サイトや電話アプリなど、実際のソリューションにも同じ原則と機能が適用されます。

## *Cognitive Services* リソースを作成する

Computer Vision サービスを使用するには、**Computer Vision** リソースまたは **Cognitive Services**リソースを作成します。

まだ作成していない場合は、Azure サブスクリプションに **Cognitive Services** リソースを作成します。

1. 別のブラウザー タブで Azure portal ([https://portal.azure.com](https://portal.azure.com?azure-portal=true)) を開き、Microsoft アカウントでサインインします。

1. **[&#65291;リソースの作成]** ボタンをクリックして、「*Cognitive Services*」を検索し、次の設定を使用して **Cognitive Services** リソースを作成します。
    - **[サブスクリプション]**: *お使いの Azure サブスクリプション*。
    - **[リソース グループ]**: *一意の名前のリソース グループを選択するか、作成します*。
    - **リージョン**: 使用できるリージョンを選択します**
    - **[名前]**: *一意の名前を入力します*。
    - **価格レベル**: Standard S0
    - **このボックスをオンにすることで、私は以下のすべての契約条件を読んで理解したことを認めます**: 選択されています。

1. リソースを確認して作成し、デプロイが完了するまで待ちます。 次に、デプロイされたリソースに移動します。

1. Cognitive Services リソースの **[キーとエンドポイント]** ページを表示します。 クライアント アプリケーションから接続するには、エンドポイントとキーが必要です。

## Cloud Shell の実行

Computer Vision サービスの機能をテストするために、Azure の Cloud Shell で実行される単純なコマンドライン アプリケーションを使用します。

1. Azure portal で、ページの上部の検索ボックスの右側にある **[>_]** (*Cloud Shell*) ボタンを選択します。 これにより、ポータルの下部に Cloud Shell ペインが開きます。

    ![上部の検索ボックスの右側にあるアイコンをクリックして、Cloud Shell を起動します](media/analyze-images-computer-vision-service/powershell-portal-guide-1.png)

1. Cloud Shell を初めて開くと、使用するシェルの種類 (*Bash* または *PowerShell*) を選択するように求められる場合があります。 **[PowerShell]** を選択します。 このオプションが表示されない場合は、このステップをスキップします。  

1. Cloud Shell のストレージを作成するように求めるメッセージが表示された場合は、サブスクリプションが指定されていることを確認して、**[ストレージの作成]** を選択します。 その後、ストレージが作成されるのを 1 分程度待ちます。

    ![確認をクリックしてストレージを作成します。](media/analyze-images-computer-vision-service/powershell-portal-guide-2.png)

1. Cloud Shell ペインの左上に表示されるシェルの種類が *PowerShell* に切り替えられたことを確認します。 *Bash* の場合は、ドロップダウン メニューを使用して *PowerShell* に切り替えます。

    ![PowerShell に切り替えるための左側のドロップダウン メニューを見つける方法](media/analyze-images-computer-vision-service/powershell-portal-guide-3.png)

1. PowerShell が起動するまで待ちます。 Azure portal に次の画面が表示されます。  

    ![PowerShell が起動するまで待ちます。](media/analyze-images-computer-vision-service/powershell-prompt.png)

## クライアント アプリケーションを構成して実行する

Cloud Shell 環境を用意できたので、Computer Vision サービスを使用して画像を分析する単純なアプリケーションを実行できます。

1. コマンド シェルで、次のコマンドを入力してサンプル アプリケーションをダウンロードし、ai-900 というフォルダーに保存します。

    ```PowerShell
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    ```

    > **ヒント** 別のラボで既にこのコマンドを使用して *ai-900* リポジトリを複製した場合は、この手順をスキップできます。

1. ファイルが **ai-900** という名前のフォルダーにダウンロードされます。 次に、Cloud Shell ストレージ内のすべてのファイルを表示して、それらを使用します。 シェルに次のコマンドを入力します。

    ```PowerShell
    code .
    ```

    これにより、次の図のようなエディターが開きます。

    ![コード エディター。](media/analyze-images-computer-vision-service/powershell-portal-guide-4.png)

1. 左側の **[ファイル]** ペインで、**[ai-900]** を展開して、**[analyze-image.ps1]** を選択します。 このファイルには、次に示すように、Computer Vision サービスを使用して画像を分析するコードが含まれています。

    ![画像を分析するコードを含むエディター](media/analyze-images-computer-vision-service/analyze-image-code.png)

1. コードについてあまり心配しないでください。重要なのは、エンドポイント URL と Cognitive Services リソースのいずれかのキーが必要であることです。 Azure portal のリソースの **[キーとエンドポイント]** ページからこれらをコピーして、コード エディターに貼り付け、**YOUR_KEY** と **YOUR_ENDPOINT** プレースホルダーの値をそれぞれ置き換えます。

    > **ヒント** **[キーとエンドポイント]** および **[エディター]** ペインを操作するときに、区分線を使用して画面領域を調整しなければならないことがあります。

    キーとエンドポイントの値を貼り付けると、コードの最初の 2 行は次のようになります。

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."    
    $endpoint="https..."
    ```

1. エディター ペインの右上の **[...]** ボタンを使用してメニューを開き、**[保存]** を選択して変更を保存します。

    サンプル クライアント アプリケーションでは、Computer Vision サービスを使用して、Northwind Traders ストア内のカメラによって撮影された次の画像を分析します。

    ![Cellphone カメラを使用してストア内の子どもの画像を撮影している親の画像](media/analyze-images-computer-vision-service/store-camera-1.jpg)

1. PowerShell ウィンドウで、次のコマンドを入力してコードを実行します。

    ```PowerShell
    cd ai-900
    ./analyze-image.ps1 store-camera-1.jpg
    ```

1. 以下のような画像分析の結果を確認します。
    - 画像の説明として、示唆されるキャプション。
    - 画像内で識別されたオブジェクトの一覧。
    - 画像に関連する "タグ" のリスト。

1. それでは、別の画像を試してみましょう。

    ![スーパーマーケットで買い物かごを持っている人の画像](media/analyze-images-computer-vision-service/store-camera-2.jpg)

    2 番目の画像を分析するには、次のコマンドを入力します。

    ```PowerShell
    ./analyze-image.ps1 store-camera-2.jpg
    ```

1. 2 番目の画像の分析結果を確認します。

1. もう 1 つ試してみましょう。

    ![ショッピングカートを持っている人の画像](media/analyze-images-computer-vision-service/store-camera-3.jpg)

    3 番目の画像を分析するには、次のコマンドを入力します。

    ```PowerShell
    ./analyze-image.ps1 store-camera-3.jpg
    ```

1. 3 番目の画像の分析結果を確認します。

## 詳細情報

このシンプルなアプリでは、Computer Vision サービスの一部の機能しか示されていません。 このサービスで実行できる操作の詳細については、[Computer Vision のページ](https://azure.microsoft.com/products/ai-services?activetab=pivot:visiontab)を参照してください。
