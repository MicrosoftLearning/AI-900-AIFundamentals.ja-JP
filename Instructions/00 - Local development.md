## ローカル開発 

ローカル コンピューターで作業している場合は、次の手順に従って、ラボで動作する環境を構成できます。  

### C++ 再頒布可能パッケージ 
1. [Visual C++ 再頒布可能パッケージ (x64)](https://aka.ms/vs/16/release/vc_redist.x64.exe) をダウンロードしてインストールします 

### Python と必要なパッケージ 
1. [Python 3.6.1](https://www.python.org/downloads/release/python-361/) をインストールします  
   - **重要**: Python を PATH 変数に追加し、デフォルトの Python 環境として登録するオプションを選択します。 
2. インストールしたら*コマンド プロンプト*を開き、次のコマンドを入力して必要なパッケージをインストールします。 

> pip install ipython jupyter matplotlib pillow requests azure-cognitiveservices-vision-computervision azure-cognitiveservices-vision-customvision azure-cognitiveservices-vision-face azure-cognitiveservices-language-textanalytics azure.cognitiveservices.speech azure_ai_formrecognizer 

### Visual Studio Code 
1. Visual Studio Code をインストールしていない場合は、[ここからダウンロード](https://code.visualstudio.com/Download)します。インストールしたら Visual Studio Code を起動し、「拡張機能」タブ (Ctrl+Shift+X) で **Python** 拡張機能を検索してインストールします。

2. Visual Studio Code で新しいターミナルを開き、**git clone https://github.com/MicrosoftLearning/AI-900JA-Microsoft-Azure-AI-Fundamentals** と入力して *Enter* キーを押します。 

