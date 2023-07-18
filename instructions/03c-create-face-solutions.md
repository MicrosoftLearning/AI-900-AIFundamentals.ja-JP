---
lab:
  title: 顔認識について調べる
---

# 顔認識について調べる

> **注** このラボを完了するには、管理者アクセス権が与えられている [Azure サブスクリプション](https://azure.microsoft.com/free?azure-portal=true)が必要です。

多くの場合、コンピューター ビジョン ソリューションでは、人工知能 (AI) ソリューションは人間の顔を検出できる必要があります。 たとえば、Northwind Traders という小売会社が、顧客に最適な支援を提供するために顧客が店舗内で立っている位置を特定したいとします。 これを実現する 1 つの方法は、画像内に顔があるかどうかを判断し、ある場合は、顔の周囲の境界ボックス座標を識別することです。

Face サービスの機能をテストするために、Cloud Shell で実行する単純なコマンドライン アプリケーションを使用します。 Web サイトや電話アプリなど、実際のソリューションにも同じ原則と機能が適用されます。

## *Face API* リソースを作成する

**Face** リソースを作成すると、Face サービスを利用できます。 (Face API は、Cognitive Services では使用できなくなりました)

まだ作成していない場合は、Azure サブスクリプションで **Face API** リソースを作成します。

1. 別のブラウザー タブで Azure portal ([https://portal.azure.com](https://portal.azure.com?azure-portal=true)) を開き、Microsoft アカウントでサインインします。

1. **[&#65291;リソースの作成]** ボタンをクリックして、「*Face*」を検索し、次の設定で **Face** リソースを作成します。
    - **[サブスクリプション]**: *お使いの Azure サブスクリプション*。
    - **[リソース グループ]**: *一意の名前のリソース グループを選択するか、作成します*。
    - **リージョン**: 使用できるリージョンを選択します**
    - **[名前]**: *一意の名前を入力します*。
    - **価格レベル**: Free F0

1. リソースを確認して作成し、デプロイが完了するまで待ちます。 次に、デプロイされたリソースに移動します。

1. Face リソースの **[キーとエンドポイント]** ページを表示します。 クライアント アプリケーションから接続するには、エンドポイントとキーが必要です。

## Cloud Shell の実行

Face サービスの機能をテストするために、Azure の Cloud Shell で実行する単純なコマンドライン アプリケーションを使用します。 

1. Azure portal で、ページの上部の検索ボックスの右側にある **[>_]** (*Cloud Shell*) ボタンを選択します。 これにより、ポータルの下部に Cloud Shell ペインが開きます。 

    ![上部の検索ボックスの右側にあるアイコンをクリックして、Cloud Shell を起動します](media/create-face-solutions/powershell-portal-guide-1.png)

1. Cloud Shell を初めて開くと、使用するシェルの種類 (*Bash* または *PowerShell*) を選択するように求められる場合があります。 **[PowerShell]** を選択します。 このオプションが表示されない場合は、このステップをスキップします。  

1. Cloud Shell のストレージを作成するように求めるメッセージが表示された場合は、サブスクリプションが指定されていることを確認して、**[ストレージの作成]** を選択します。 その後、ストレージが作成されるのを 1 分程度待ちます。

    ![確認をクリックしてストレージを作成します。](media/create-face-solutions/powershell-portal-guide-2.png)       

1. [Cloud Shell] ペインの左上に表示されるシェルの種類が *PowerShell* に切り替えられたことを確認します。 *Bash* の場合は、ドロップダウン メニューを使用して *PowerShell* に切り替えます。

    ![PowerShell に切り替えるための左側のドロップダウン メニューを見つける方法](media/create-face-solutions/powershell-portal-guide-3.png) 

1. PowerShell が起動するまで待ちます。 Azure portal に次の画面が表示されます。  

    ![PowerShell が起動するまで待ちます。](media/create-face-solutions/powershell-prompt.png)

## クライアント アプリケーションを構成して実行する

カスタム モデルが作成されたので、Face サービスを使用する簡単なクライアント アプリケーションを実行できます。

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

    ![コード エディター。](media/create-face-solutions/powershell-portal-guide-4.png) 

1. 左側の **[ファイル]** ペインで、**[ai-900]** を展開し、**[find-faes.ps1]** を選択します。 このファイルには、次に示すように、Face サービスを使用して画像内の顔を検出して分析するコードが含まれています。

    ![画像内の顔を検出するコードを含むエディター](media/create-face-solutions/find-faces-code.png)

1. コードについては、あまり細かい部分まで心配しないでください。重要なのは、エンドポイント URL と、Face リソースのいずれかのキーが必要であるということです。 Azure portal のリソースの **[キーとエンドポイント]** ページからこれらをコピーして、コード エディターに貼り付け、**YOUR_KEY** と **YOUR_ENDPOINT** プレースホルダーの値をそれぞれ置き換えます。

    > **ヒント** **[キーとエンドポイント]** および **[エディター]** ペインを操作するときに、区分線を使用して画面領域を調整しなければならないことがあります。

    キーとエンドポイントの値を貼り付けると、コードの最初の 2 行は次のようになります。

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."    
    $endpoint="https..."
    ```

1. エディター ペインの右上の **[...]** ボタンを使用してメニューを開き、**[保存]** を選択して変更を保存します。 次に、メニューを再度開き、**[エディターを閉じる]** を選択します。

    サンプル クライアント アプリケーションは、Face サービスを使用して、Northwind Traders ストア内のカメラによって撮影された次の画像を分析します。

    ![携帯電話のカメラでストア内で子どもの写真を撮影している親の画像](media/create-face-solutions/store-camera-1.jpg)

1. PowerShell ウィンドウで、次のコマンドを入力してコードを実行します。

    ```PowerShell
    cd ai-900
    ./find-faces.ps1 store-camera-1.jpg
    ```

1. 画像内の顔の位置を含む、返された情報を検討します。 次に示すように、顔の位置は左上の座標と "境界ボックス" の幅と高さによって示されます。**

    ![顔の輪郭が示された人物の画像](media/create-face-solutions/store-camera-1-face.jpg)

    >**注** 個人を特定できる機能を返す Face サービス機能は制限されています。 詳細については、https://azure.microsoft.com/blog/responsible-ai-investments-and-safeguards-for-facial-recognition/ を参照してください。

1. それでは、別の画像を試してみましょう。

    ![買い物かごを持っている人物の画像](media/create-face-solutions/store-camera-2.jpg)

    2 番目の画像を分析するには、次のコマンドを入力します。

    ```PowerShell
    ./find-faces.ps1 store-camera-2.jpg
    ```

1. 2 番目の画像の顔分析の結果を確認します。

1. もう 1 つ試してみましょう。

    ![ショッピングカートを持っている人の画像](media/create-face-solutions/store-camera-3.jpg)

    3 番目の画像を分析するには、次のコマンドを入力します。

    ```PowerShell
    ./find-faces.ps1 store-camera-3.jpg
    ```

1. 3 番目の画像の顔分析の結果を確認します。

## 詳細情報

この簡単なアプリでは、Face サービスの一部の機能のみを示しています。 このサービスで実行できる操作の詳細については、[Face API のページ](https://azure.microsoft.com/en-us/products/cognitive-services/vision-services)をご覧ください。
