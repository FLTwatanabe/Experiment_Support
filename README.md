# README

## Python
anacondaより通常のpythonをインストールした方が使いやすいです。  
以下のページからインストールしてください。  
https://www.python.jp/install/windows/install_py3.html  
注）anacondaがすでにインストールされている場合は不具合が起こるかも。

## プロジェクトを１から自分で作る場合
仮想環境を作ります。  
次の記事を参考に。  
https://qiita.com/yniji/items/b1b20211846a5a9f189b

```
# プロジェクト用ディレクトリを作る。
mkdir <project-name>

# プロジェクト用ディレクトリに移動
cd <path-to-directory>

# 仮想環境作成コマンド
py -m venv .
> ディレクトリの直下に色々生成されるはず。

# 仮想環境に入る
.\Scripts\activate

# 仮想環境から出る
deactivate
```

プロジェクトの作業はこの仮想環境内で行います。  
pipコマンドでインストールされる依存ライブラリなどは、この環境内のみで使用できます。  

## Githubからクローンしたものを使う場合
世界の研究者やエンジニアが作成したプロジェクトをgithubに公開してくれています。  
そのプロジェクトをクローンして、環境を整えればすぐに使い始めることができます。  

```
# デスクトップなどに移動。そのディレクトリの直下にプロジェクトフォルダがクローンされる。
git clone https://github.com/eriklindernoren/PyTorch-YOLOv3.git

# そのディレクトリに移動
cd PyTorch_YOLOv3

# 仮想環境
py -m venv .
.\Scripts\activate

# 依存ライブラリのインストール（requirements.txtがある場合）
pip install -r requirements.txt

# ない場合には手動でインストールする必要があります。
pip install pytorch torchvision opencv-python

# もしエラーが出た場合は、エラーメッセージに従って必要なものをインストールしてください。
```

### 学習データを作る
* LabelImgを使ってアノテーションデータを作る。  
注） アノテーション形式をYOLOにすることを忘れずに！！  
* configファイルを作成する。--data_nameオプションに、対象となるデータが格納されたディレクトリの名称（例: <your-data-name>）を指定してください。  
注）データのディレクトリには、'images': jpg画像が格納、'labels': アノテーションデータが格納、'config': 設定ファイルが格納される  
注）LabelImgでアノテーションデータを作成すると、'labels'ディレクトリに'classes.txt'が生成されますが、そのファイルはconfigに移動しておいてください。  
```
python create_cfg.py --mode cfg --dataset_type yolo --data_name <your-data> --class_num 1 --ratio 0.6
```
最終的に  
data/your-data-name/images/*.jpg  
data/your-data-name/labels/*.txt  
data/your-data-name/config/*  
というディレクトリ構造になります。  
configディレクトリに、yolov3.cfgとclasses.txtをおいてください。

### 学習の実行
以下のリンクから、'darknet53.conv.74'というweightsファイルをダウンロードしてください。  
https://pjreddie.com/media/files/darknet53.conv.74

```
# weightファイルは以下に格納
/<your-project>/weights/darknet53.conv.74

# 多分バグるので "train.py" 150~175行目をコメントアウト

# 実行
python train.py --data_config data/<your-data-name>/config/<your-data-name>.data  --pretrained_weights weights/darknet53.conv.74 --model_def data/<your-data-name>/config/yolov3.cfg
```

## バグが出たら

### 懸念事項
* 筆者の実行環境がLinuxなので、windowsだと不具合ある可能性大
* パスの書き方がlinuxと違うので、そのあたりが怖いかも。

### GPU周り
* メモリ不足になったら、バッチサイズを小さくする。train.pyのオプションで指定可能。