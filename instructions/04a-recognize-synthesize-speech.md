---
lab:
  title: スピーチについて調べる
---

# <a name="explore-speech"></a>スピーチについて調べる

> **注** このラボを完了するには、管理者アクセス権が与えられている [Azure サブスクリプション](https://azure.microsoft.com/free?azure-portal=true)が必要です。

可聴音声を解釈して適切に対応できるソフトウェアを構築するには、**音声** Cognitive Service を使用できます。これにより、音声をテキストに簡単に再生することができます。また、その逆も可能です。

たとえば、"何時ですか" など、口頭での質問に対して音声で他者に応答できるスマートデバイスを作成するとします。 応答は、現地時刻である必要があります。

音声サービスの機能をテストするために、Cloud Shell で実行する単純なコマンドライン アプリケーションを使用します。 Web サイトや電話アプリなど、実際のソリューションにも同じ原則と機能が適用されます。

## <a name="create-a-cognitive-services-resource"></a>*Cognitive Services* リソースを作成する

音声サービスを使用するには、**音声**リソースまたは **Cognitive Services** リソースを作成します。

まだ作成していない場合は、Azure サブスクリプションで **Cognitive Services** リソースを作成します。

1. 別のブラウザー タブで Azure portal ([https://portal.azure.com](https://portal.azure.com?azure-portal=true)) を開き、Microsoft アカウントでサインインします。

1. **[&#65291;リソースの作成]** ボタンをクリックして、「*Cognitive Services*」を検索し、次の設定を使用して **Cognitive Services** リソースを作成します。
    - **[サブスクリプション]**: *お使いの Azure サブスクリプション*。
    - **[リソース グループ]**: *一意の名前のリソース グループを選択するか、作成します*。
    - **リージョン**: 使用できるリージョンを選択します**
    - **[名前]**: *一意の名前を入力します*。
    - **価格レベル**: Standard S0
    - **このボックスをオンにすることで、私は以下のすべての契約条件を読んで理解したことを認めます**: 選択されています。

1. リソースを確認して作成します。

### <a name="get-the-key-and-location-for-your-cognitive-services-resource"></a>Cognitive Services のリソースのキーと場所を取得する

1. デプロイが完了するまで待ちます。 次に、Cognitive Services リソースに移動し、**[概要]** ページで、サービスのキーを管理するためのリンクをクリックします。 クライアント アプリケーションから Cognitive Services リソースに接続するには、エンドポイントとキーが必要です。

1. リソースの **[キーとエンドポイント]** ページを表示します。 クライアント アプリケーションから接続するには、**[場所/リージョン]** と **[キー]** が必要です。

## <a name="run-cloud-shell"></a>Cloud Shell の実行

音声サービスの機能をテストするために、Azure の Cloud Shell で実行する単純なコマンドライン アプリケーションを使用します。

1. Azure portal で、ページの上部の検索ボックスの右側にある **[>_]** (*Cloud Shell*) ボタンを選択します。 これにより、ポータルの下部に Cloud Shell ペインが開きます。

    ![上部の検索ボックスの右側にあるアイコンをクリックして、Cloud Shell を起動します](media/recognize-synthesize-speech/powershell-portal-guide-1.png)

1. Cloud Shell を初めて開くと、使用するシェルの種類 (*Bash* または *PowerShell*) を選択するように求められる場合があります。 **[PowerShell]** を選択します。 このオプションが表示されない場合は、このステップをスキップします。  

1. Cloud Shell のストレージを作成するように求めるメッセージが表示された場合は、サブスクリプションが指定されていることを確認して、**[ストレージの作成]** を選択します。 その後、ストレージが作成されるのを 1 分程度待ちます。

    ![確認をクリックしてストレージを作成します。](media/recognize-synthesize-speech/powershell-portal-guide-2.png)

1. Cloud Shell ペインの左上に表示されるシェルの種類が *PowerShell* に切り替えられたことを確認します。 *Bash* の場合は、ドロップダウン メニューを使用して *PowerShell* に切り替えます。

    ![PowerShell に切り替えるための左側のドロップダウン メニューを見つける方法](media/recognize-synthesize-speech/powershell-portal-guide-3.png)

1. PowerShell が起動するまで待ちます。 Azure portal に次の画面が表示されます。  

    ![PowerShell が起動するまで待ちます。](media/recognize-synthesize-speech/powershell-prompt.png)

## <a name="configure-and-run-a-client-application"></a>クライアント アプリケーションを構成して実行する

カスタム モデルが作成されたので、音声サービスを使用する簡単なクライアント アプリケーションを実行できます。

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

    ![コード エディター。](media/recognize-synthesize-speech/powershell-portal-guide-4.png)

1. 左側の **[ファイル]** ペインで、**[ai-900]** を展開し、**[speaking-clock.ps1]** を選択します。 このファイルには、音声サービスを使用して音声認識と合成を行うコードがいくつか含まれています。

    ![音声サービスを使用するコードが表示されているエディター](media/recognize-synthesize-speech/speaking-clock-code.png)

1. コードの詳細についてあまり心配しないでください。重要なのは、Cognitive Services リソースのリージョン/場所といずれかのキーが必要であることです。 Azure portal のリソースの **[キーとエンドポイント]** ページからこれらをコピーして、コード エディターに貼り付け、**YOUR_KEY** と **YOUR_LOCATION** プレースホルダーの値をそれぞれ置き換えます。

    > **ヒント** **[キーとエンドポイント]** および **[エディター]** ペインを操作するときに、区分線を使用して画面領域を調整しなければならないことがあります。

    キーと地域または場所の値を貼り付けると、コードの先頭の数行は次のようになります。

    ```PowerShell
    $key = "1a2b3c4d5e6f7g8h9i0j...."
    $region="somelocation"
    ```

1. エディター ペインの右上の **[...]** ボタンを使用してメニューを開き、**[保存]** を選択して変更を保存します。 次に、メニューを再度開き、**[エディターを閉じる]** を選択します。

    サンプル クライアント アプリケーションでは、音声サービスを使用して音声入力の文字起こしを行い、音声による適切な応答を合成します。 実際のアプリケーションでは、マイクから入力を受け取り、応答をスピーカーに送ることができますが、この簡単な例では、ファイルに事前に記録された入力を使用し、別のファイルとして応答を保存します。

    以下のビデオ プレーヤーを使用して、アプリケーションが処理する入力オーディオを聞いてください。

    <div class="embeddedvideo"><iframe src="https://www.microsoft.com/videoplayer/embed/RWMAvi" frameborder="0" allowfullscreen="true" data-linktype="external"></iframe></div>

1. PowerShell ウィンドウで、次のコマンドを入力してコードを実行します。

    ```PowerShell
    cd ai-900
    ./speaking-clock.ps1
    ```

1. 出力を確認します。 "何時ですか" というテキストが正常に認識されているはずです。 そして、*output.wav* という名前のファイルに適切な応答を保存します。

    次のビデオ プレーヤーを使用して、アプリケーションによって生成される音声出力を聞きます。

    <div class="embeddedvideo"><iframe src="https://www.microsoft.com/videoplayer/embed/RWMSIU" frameborder="0" allowfullscreen="true" data-linktype="external"></iframe></div>

## <a name="learn-more"></a>詳細情報

この簡単なアプリでは、音声サービスの一部の機能のみを示しています。 このサービスで実行できる操作の詳細については、[音声のページ](https://azure.microsoft.com/services/cognitive-services/speech-services/)を参照してください。