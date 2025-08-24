# oak-d × spectacularAI × nerfstudio ワークフロー

oak-dで撮影したデータ（spectacularAI形式）をGoogle Colabにアップロードし、nerfstudioで学習・可視化するまでの一連の手順をまとめたものです。

## フロー概要
1. **ローカル環境セットアップ** - spectacularAIのインストール
2. **データ撮影** - oak-dでspectacularAIをつかってデータ収集
3. **データ処理・変換** - NerfStudio形式に変換・プレビュー
4. **Google Colabでの学習** - クラウド環境での高速学習
5. **結果ダウンロード・確認** - 学習済みモデルの取得と可視化

---

## 1. ローカル環境でspectacularAIのインストール

```bash
# 仮想環境作成（推奨）
python -m venv .venv
.venv\Scripts\activate  # Windows
# source .venv/bin/activate  # macOS/Linux

# spectacularAIのインストール
pip install spectacularAI[full]
```

## 2. oak-dでデータ撮影

```bash
# データ撮影開始
sai-cli record oak

# 撮影のコツ：
# - 対象物の周りを円を描くように移動
# - ゆっくりと滑らかに移動
# - 十分な照明を確保
# - [q]キーで終了
```

## 3. データ処理・変換

### 3D プレビューで品質確認
```bash
# 3次元ビューワでプレビュー（品質確認）
sai-cli process --device_preset oak-d --preview3d .\data\2025-08-24_11-24-17 .\processed
```

### NerfStudio形式に変換
```bash
# NerfStudio形式に変換
sai-cli process --device_preset oak-d --format nerfstudio .\data\2025-08-24_11-24-17 .\processed
```

**生成されるファイル構造:**
```
processed/
├── transforms.json    # カメラパラメータ
├── images/           # フレーム画像
│   ├── frame_00001.jpg
│   ├── frame_00002.jpg
│   └── ...
├── sparse_pc.ply     # 点群データ（オプション）
└── colmap/          # COLMAP データ（オプション）
```

## 4. Google Colabでの学習

### データ準備
1. `processed/` フォルダを右クリック → 圧縮 → `processed.zip` を作成
2. Google Colab で `colab_nerfstudio.ipynb` を開く
3. セル2でzipファイルをアップロード

### Colabノートブック機能

**改善された機能:**
- **Windows互換性**: パス区切り文字の自動修正
- **GPU監視**: リアルタイムGPUメモリ使用量表示  
- **エラーハンドリング**: 各段階での詳細なエラー診断
- **ファイル管理**: サイズ確認付きの選択的ダウンロード
- **進捗表示**: 経過時間とステップ数の監視

**セル構成:**
1. **環境セットアップ** - GPU確認・PyTorch・nerfstudioインストール
2. **データアップロード** - zip展開・パス修正・構造確認
3. **nerfstudio動作確認** - インポートテスト・バージョン確認
4. **データ検証** - transforms.json・画像数・品質チェック
5. **学習実行** - GPU監視付き学習・リアルタイム進捗表示
6. **結果ダウンロード** - モデル・設定ファイルの取得

## 5. 結果ダウンロード・確認

### Colabからのダウンロード
- `.ckpt` ファイル（学習済みモデル）
- `config.yml` ファイル（設定情報）
- その他関連ファイル（JSON等）

### ローカルでの確認
```bash
# nerfstudio viewerで結果確認
ns-viewer --load-config path/to/config.yml

# または
ns-train nerfacto --data path/to/processed --load-dir path/to/outputs
```

---

## トラブルシューティング

### よくある問題と解決方法

**1. パス区切り文字エラー**
```
FileNotFoundError: 'processed/images\frame_00001.jpg'
```
→ セル2のパス修正機能で自動解決

**2. GPU利用不可**
```
CUDA available: False
```
→ Colab: ランタイム → ランタイムのタイプを変更 → GPU(T4)

**3. 画像数不足**
```
画像数が少ないため、学習結果の品質が低い可能性があります
```
→ より多くの角度からデータを撮影

**4. メモリ不足**
→ セル5のGPU監視で使用量を確認、バッチサイズ調整

---

## システム要件

### ローカル環境
- Python 3.8+ 
- oak-d カメラ
- 十分なストレージ容量（数GB）

### Google Colab
- GPU ランタイム（T4/A100推奨）
- 十分なメモリ（12GB以上推奨）

---

## 参考資料

- [nerfstudio公式ドキュメント](https://docs.nerf.studio/)
- [spectacularAI公式ドキュメント](https://docs.spectacularai.com/)
- [oak-d カメラ情報](https://docs.luxonis.com/projects/hardware/en/latest/)

---


