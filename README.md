# hipDDH — 股関節DDRのYOLO Pose解析（PA / ML・LCEA）

健常股関節の動的デジタルX線撮影（DDR）から骨性ランドマークを検出する研究、および両側DDH例を含むLCEA自動計測の探索的研究用Notebookです。

> **研究用途**。モデルの推論値を臨床診断・治療判断に直接使用しないでください。研究用X線画像、COCO annotation、患者情報、学習重み、結果動画は**このリポジトリに保存しません**。

## Google Colabで開く

### 1. 最新：健常PA/MLの患者単位CV・精度再検証・#005 Matsuda figure / LCEA動画

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/atstuyuki/HipDDH/blob/main/notebooks/DDR_Healthy_PA_ML_GroupCV_Matsuda005_LCEA_20261023.ipynb)

**[ノートブックをColabで開く](https://colab.research.google.com/github/atstuyuki/HipDDH/blob/main/notebooks/DDR_Healthy_PA_ML_GroupCV_Matsuda005_LCEA_20261023.ipynb)**

- 入力：`AI-Hip Gate Analysis.zip`（GitHubへアップロードしない）
- PA：健常 **9患者・174フレーム**、10 keypoints（現時点のZIPにある症例を自動集計）
- ML：健常 **10患者・192フレーム**、9 keypoints
- 検証：患者単位5-fold CV（`CV_MODE='group5'`）、任意でLOVO（`CV_MODE='lovo'`）
- 学会用出力：Pose precision / recall / mAP50 / mAP50-95、keypoint位置誤差と基準線角度誤差のMAE / RMSE、患者別解析
- #005 Matsuda：正面・側面の手動点／AI点比較PNG、および正面のAI-LCEA値入り動画・CSV
- #005は**学習に使わず**、未知患者として予測します。

**注意**：#005の動画はCOCO画像をフレーム順に並べた**提示用の再構成動画**です。`DISPLAY_FPS`は表示速度であって撮影機器のDDR実fpsではありません。別途元動画を指定する場合はフレーム対応の確認が必要です。

### 2. 健常・両側DDH統合のLCEA探索的解析（参考）

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/atstuyuki/HipDDH/blob/main/notebooks/DDR_Hip_DDH_LCEA_FEAR_Revised_FromScratch_Colab.ipynb)

**[統合NotebookをColabで開く](https://colab.research.google.com/github/atstuyuki/HipDDH/blob/main/notebooks/DDR_Hip_DDH_LCEA_FEAR_Revised_FromScratch_Colab.ipynb)**

- 入力：`AI-Hip Gate Analysis.zip` と DDHアノテーションZIP `gigafile-1022-d973a9d3dde722592c63b5704b11c478.zip`
- 新規に全骨盤Pose＋左右局所Poseを学習し、LCEAを計測
- FEAR-like角度は**未完成の探索的指標**です。発表用の確定指標にはしないでください。
- 患者単位hold-out：例示設定のままでは2患者のみです。汎化性能の確証は得られません。

## 実行手順

1. 上の **Open in Colab** を押します。
2. Colabの **ランタイム → ランタイムのタイプを変更 → GPU（T4など）** を選択します。
3. Notebookのセルを上から実行します。指定のZIPがない場合は、入力セルにアップロード画面が表示されます。
4. Colab環境にZIPが置かれていれば、`SOURCE_ZIP` / `HEALTHY_ZIP` / `DDH_ZIP` をその絶対パスに合わせます。
5. GPU学習が完了すると、評価CSV、Figure、動画、学習重みを含む結果ZIPが作成・ダウンロードされます。

**長時間の学習時**はGoogle DriveをColabにマウントし、作業先の `ROOT` をDrive配下に変更すると、ランタイム切断による出力消失を避けられます。転送する研究データの権限管理にもご注意ください。

リポジトリが **Private** の場合、Colab側でGitHubアカウントへのアクセス許可が必要になることがあります。上のリンクで開けない場合は、Colabの「ファイル → ノートブックを開く → GitHub」から対象リポジトリを指定してください。

## 評価・報告に関する重要事項

- **患者単位分割**を維持し、連続フレームを独立症例として数えない。
- PAとMLではランドマーク定義が異なるため、誤差を無条件に合算しない。
- keypoint MAE（画素単位の位置距離の平均）と角度MAE（度）は別の指標である。
- YOLOのPose mAPと、LCEAの臨床角度計測誤差は別々に記載する。
- 検出に失敗したフレームを無視して高い精度だけ報告せず、有効率・欠測率を示す。
- 旧5例LOVOの結果と追加症例の5-fold CVは**同じ評価実験ではない**。旧結果と新結果を単純に平均・優劣比較しない。
- DDHのLCEA手動参照がある健常例は全10例ではありません。症例数はNotebook出力で確認してください。

## リポジトリ構造

```text
HipDDH/
├── README.md
├── .gitignore
├── requirements.txt
├── docs/
│   └── DATA_POLICY.md
└── notebooks/
    ├── DDR_Healthy_PA_ML_GroupCV_Matsuda005_LCEA_20261023.ipynb
    └── DDR_Hip_DDH_LCEA_FEAR_Revised_FromScratch_Colab.ipynb
```

### 実行環境

- Google Colab（Python + GPU）
- `ultralytics` (YOLO11 Pose)、`torch`、`opencv-python-headless`、`pandas`、`numpy`、`scikit-learn` など
- 依存ライブラリは各Notebook冒頭の `%pip install` セルでも導入されます。

研究データや未検証結果を含めないように、**Notebookの出力セルは消去した状態で管理**します。

## 前田先生 — 18クラスSegmentation（患者独立3-fold）

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/atstuyuki/HipDDH/blob/main/notebooks/Maeda_HipDDR_18class_Group3Fold_Segmentation_Colab.ipynb)

健常10例＋DDH3例のCOCO segmentationを患者単位で分割する探索的解析です。
左右（R/L）×歩行相（SW/ST/DS）×3領域（SL/SM/M）の18クラスについてMask mAPを評価し、歩行相を除いた左右6領域のDice/IoUも患者別に算出します。
**研究用のZIP・画像・患者情報・学習重みはGitHubにアップロードしないでください。**
GPUによる再学習は別途Colabで実行してください。
