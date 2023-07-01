---
lab:
  title: テキスト分析について調べる
---

# テキスト分析について調べる

> **注** このラボを完了するには、管理者アクセス権が与えられている [Azure サブスクリプション](https://azure.microsoft.com/free?azure-portal=true)が必要です。

自然言語処理 (NLP) は、記述言語と音声言語を扱う人工知能 (AI) のブランチです。 NLP を使用して、テキストや音声からセマンティックな意味を抽出するソリューションや、自然言語で意味のある応答を作成するソリューションを構築できます。

Microsoft Azure *Cognitive Services* には "言語" サービス内にテキスト分析機能が含まれており、テキスト内のキー フレーズの識別や、センチメントに基づくテキストの分類など、すぐに使用できるいくつかの NLP 機能が用意されています。**

たとえば、架空の *Margie's Travel* という組織が顧客にホテル滞在のレビューを提出するように促していると想定してみましょう。 言語サービスを使用して、キー フレーズを抽出してレビューを要約したり、レビューのうち肯定的なものと否定的なものを特定したり、場所や人などの既知のエンティティのメンションについてレビュー テキストを分析したりすることができます。

言語サービスの機能をテストするために、Cloud Shell で実行する単純なコマンドライン アプリケーションを使用します。 Web サイトや電話アプリなど、実際のソリューションにも同じ原則と機能が適用されます。

## *Cognitive Services* リソースを作成する

言語サービスを使用するには、**言語**リソースまたは **Cognitive Services** リソースを作成します。

まだ作成していない場合は、Azure サブスクリプションに **Cognitive Services** リソースを作成します。

1. 別のブラウザー タブで Azure portal ([https://portal.azure.com](https://portal.azure.com?azure-portal=true)) を開き、Microsoft アカウントでサインインします。

1. **[&#65291;リソースの作成]** ボタンをクリックして、「*Cognitive Services*」を検索し、次の設定を使用して **Cognitive Services** リソースを作成します。
    - **[サブスクリプション]**: *お使いの Azure サブスクリプション*。
    - **[リソース グループ]**: *一意の名前のリソース グループを選択するか、作成します*。
    - **リージョン**: 使用できるリージョンを選択します**
    - **[名前]**: *一意の名前を入力します*。
    - **価格レベル**: Standard S0
    - **このボックスをオンにすることで、私は以下のすべての契約条件を読んで理解したことを認めます**: 選択されています。

1. リソースを確認して作成します。

### Cognitive Services のリソースのキーとエンドポイントを取得する

1. デプロイが完了するまで待ちます。 次に、Cognitive Services リソースに移動し、**[概要]** ページで、サービスのキーを管理するためのリンクを選択します。 クライアント アプリケーションから Cognitive Services リソースに接続するには、エンドポイントとキーが必要です。

1. リソースの **[キーとエンドポイント]** ページを表示します。 クライアント アプリケーションから接続するには、**キー**と**エンドポイント**が必要です。

## Cloud Shell の実行

言語サービスのテキスト分析機能をテストするために、Azure の Cloud Shell で実行される単純なコマンドライン アプリケーションを使用します。

1. Azure portal で、ページの上部の検索ボックスの右側にある **[>_]** (*Cloud Shell*) ボタンを選択します。 これにより、ポータルの下部に Cloud Shell ペインが開きます。

    ![上部の検索ボックスの右側にあるアイコンをクリックして、Cloud Shell を起動します](media/analyze-text-language-service/powershell-portal-guide-1.png)

1. Cloud Shell を初めて開くと、使用するシェルの種類 (*Bash* または *PowerShell*) を選択するように求められる場合があります。 **[PowerShell]** を選択します。 このオプションが表示されない場合は、このステップをスキップします。  

1. Cloud Shell のストレージを作成するように求めるメッセージが表示された場合は、サブスクリプションが指定されていることを確認して、**[ストレージの作成]** を選択します。 その後、ストレージが作成されるのを 1 分程度待ちます。

    ![確認をクリックしてストレージを作成します。](media/analyze-text-language-service/powershell-portal-guide-2.png)

1. [Cloud Shell] ペインの左上に表示されるシェルの種類が *PowerShell* に切り替えられたことを確認します。 *Bash* の場合は、ドロップダウン メニューを使用して *PowerShell* に切り替えます。

    ![PowerShell に切り替えるための左側のドロップダウン メニューを見つける方法](media/analyze-text-language-service/powershell-portal-guide-3.png)

1. PowerShell が起動するまで待ちます。 Azure portal に次の画面が表示されます。  

    ![PowerShell が起動するまで待ちます。](media/analyze-text-language-service/powershell-prompt.png)

## クライアント アプリケーションを構成して実行する

カスタム モデルが作成されたので、言語サービスを使用するシンプルなクライアント アプリケーションを実行できます。

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

    ![コード エディター。](media/analyze-text-language-service/powershell-portal-guide-4.png)

1. 左側の **[ファイル]** ペインで、**[ai-900]** を展開して、**[analyze-text.ps1]** を選択します。 このファイルには、言語サービスを使用するコードが含まれています。

    ![言語サービスを使用するコードが表示されているエディター](media/analyze-text-language-service/analyze-text-code.png)

1. コードの詳細についてはあまり気にしないでください。 Azure portal で、Cognitive Services リソースに移動します。 次に、左側のウィンドウの **[キーとエンドポイント]** ページを選択します。 キーとエンドポイントをページからコピーして、コード エディターに貼り付け、**YOUR_KEY** と **YOUR_ENDPOINT** プレースホルダーの値をそれぞれ置き換えます。

    > **ヒント** **[キーとエンドポイント]** および **[エディター]** ペインを操作するときに、区分線を使用して画面領域を調整しなければならないことがあります。

    ![Cognitive Services リソースの左側のペインで、キーとエンドポイントのタブを見つけます。](media/analyze-text-language-service/key-endpoint-support.png)

    キーとエンドポイントの値を置き換えると、コードの最初の行は次のようになります。

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."
    $endpoint="https..."
    ```

1. エディター ペインの右上で、**[...]** ボタンを使用してメニューを開き、**[保存]** を選択して変更を保存します。 次に、メニューを再度開き、**[エディターを閉じる]** を選択します。

    サンプル クライアント アプリケーションでは、Cognitive Services の言語サービスを使用して、言語の検出、キー フレーズの抽出、センチメントの決定、レビュー用の既知のエンティティの抽出を行います。

1. Cloud Shell で、次のコマンドを入力してコードを実行します。

    ```PowerShell
    cd ai-900
    ./analyze-text.ps1 review1.txt
    ```

    次のテキストを確認します。

    >Good Hotel and staff The Royal Hotel, London, UK 3/2/2018 Clean rooms, good service, great location near Buckingham Palace and Westminster Abbey, and so on. (良いホテルで、スタッフも良い。英国、ロンドン、The Royal Hotel、2018 年 3 月 2 日、清潔は客室、優れたサービス、バッキンガム宮殿やウエストミンスター寺院などに近い便利な場所。) We thoroughly enjoyed our stay. (ホテル滞在中、十分に楽しめました。) The courtyard is very peaceful and we went to a restaurant which is part of the same group and is Indian ( West coast so plenty of fish) with a Michelin Star. (中庭はとても静かで、同じグループに属するインド料理レストランに行きました。そこはミシュラン一つ星のレストランで、西海岸なので魚が豊富でした。) We had the taster menu which was fabulous. (テイスター メニューがあり、素晴らしかったです。) The rooms were very well appointed with a kitchen, lounge, bedroom and enormous bathroom. (客室には、素敵なキッチン、ラウンジ、ベッドルーム、そして広々としたバスルームが備わっていました。) Thoroughly recommended. (とてもお勧めします。)

1. 出力結果を確認します。

1. PowerShell ウィンドウで、次のコマンドを入力してコードを実行します。

    ```PowerShell
    ./analyze-text.ps1 review2.txt
    ```

    次のテキストを確認します。

    >Tired hotel with poor service The Royal Hotel, London, United Kingdom 5/6/2018 This is a old hotel (has been around since 1950's) and the room furnishings are average - becoming a bit old now and require changing. (サービスが悪い、古臭いホテル。英国、ロンドン、The Royal Hotel、2018 年 5 月 6 日。ここは古いホテルで (1950 年代ぐらいに設立)、客室の調度品は平均的ですが、少し古くなっており、交換する必要があると思います。) The internet didn't work and had to come to one of their office rooms to check in for my flight home. (インターネットは機能していなかったため、帰りの便をチェックインするために、オフィスに行く必要がありました。) The website says it's close to the British Museum, but it's too far to walk. (Web サイトには大英博物館に近いと記載されていましたが、徒歩で行くには遠すぎます。)

1. 出力結果を確認します。

1. PowerShell ウィンドウで、次のコマンドを入力してコードを実行します。

    ```PowerShell
    ./analyze-text.ps1 review3.txt
    ```

    次のテキストを確認します。

    >Good location and helpful staff, but on a busy road. (便利な場所にあり、スタッフは親切ですが、交通量の多い道路に面しています。)
    The Lombard Hotel, San Francisco, USA 8/16/2018 We stayed here in August after reading reviews. (米国、サンフランシスコ、The Lombard Hotel、2018 年 8 月 16 日。レビューを確認した後、8 月にこのホテルに滞在しました。) We were very pleased with location, just behind Chestnut Street, a cosmopolitan and trendy area with plenty of restaurants to choose from. (場所は非常に便利なところにあり、Chestnut Street のすぐ裏です。国際的で流行に敏感な地域であり、たくさんのレストランがあります。) The Marina district was lovely to wander through, very interesting houses. (Marina 地区を歩き回るのは楽しく、とても興味深い家屋もありました。) Make sure to walk to the San Francisco Museum of Fine Arts and the Marina to get a good view of Golden Gate bridge and the city. (ぜひ、サンフランシスコ美術館や Marina まで歩いて行って、ゴールデンゲートブリッジや街の美しい景色を楽しんでください。) On a bus route and easy to get into centre. (バスを利用すれば、簡単に中心部に行けます。) Rooms were clean with plenty of room and staff were friendly and helpful. (客室は十分な広さがあり、清潔で、スタッフはフレンドリーで親切でした。) The only down side was the noise from Lombard Street so ask to have a room furthest away from traffic noise. (唯一の欠点は、ロンバート街の騒音です。車の騒音から十分に離れた部屋がないか聞いてみてください。)

1. 出力結果を確認します。

1. PowerShell ウィンドウで、次のコマンドを入力してコードを実行します。

    ```PowerShell
    ./analyze-text.ps1 review4.txt
    ```

    次のテキストを確認します。

    >Very noisy and rooms are tiny The Lombard Hotel, San Francisco, USA 9/5/2018 Hotel is located on Lombard street which is a very busy SIX lane street directly off the Golden Gate Bridge. (非常に騒音が多く、部屋は狭い。米国、サンフランシスコ、The Lombard Hotel、2018 年 9 月 5 日。ホテルはロンバート街にあり、ゴールデンゲートブリッジと直接つながっている 6 車線の非常に交通量の多い地区です。) Traffic from early morning until late at night especially on weekends. (早朝から深夜まで、特に週末はずっと車が通ります。) Noise would not be so bad if rooms were better insulated but they are not. (客室が適切に防音されていれば騒音もそれほど気にならなかったかもしれませんが、防音されていませんでした。) Had to put cotton balls in my ears to be able to sleep--was too tired to enjoy the city the next day. (耳の中に綿ボールを入れなければ眠ることができず、疲れが取れなかったので、次の日に街を楽しむことができませんでした。) Rooms are TINY. (部屋は非常に狭いです。) I picked the room because it had two queen size beds--but the room barely had space to fit them. (2 台のクイーン サイズのベッドがあるため、その部屋を選んだのですが、部屋のサイズはその 2 台のベッドがかろうじて収まる程度のものでした。) With family of four in the room it was tight. (家族 4 人で泊まるには、狭すぎました。) With all that said, rooms are clean and they've made an effort to update them. (そうは言っても、客室は清潔で、きれいに保とうと努力していました。) The hotel is in Marina district with lots of good places to eat, within walking distance to Presidio. (ホテルは Marina 地区にあり、Presidio までの徒歩圏内にたくさんの良いレストランがあります。) May be good hotel for young stay-up-late adults on a budget. (おそらく、夜更かしをする若い成人層には予算的に良いホテルと言えるでしょう。)

1. 出力結果を確認します。

## 詳細情報

このシンプルなアプリでは、言語サービスの一部の機能のみを示しています。 このサービスで実行できる操作の詳細については、[言語サービスのページ](https://azure.microsoft.com/services/cognitive-services/language-service/)を参照してください。
