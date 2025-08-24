# oak-d × spectacularAI × nerfstudio ワークフロー## 5. Colab上でnerfstudioをセットアップ・学習
- Colabノートブック（`colab_nerfstudio.ipynb`）を参照してください。
- GPU対応PyTorchとnerfstudioが自動インストールされます
- データ確認後、学習が自動実行されます

## 6. 結果をダウンロード
- Colabのファイルダウンロード機能で、学習済みモデル（.ckpt）や設定ファイルをローカルに保存します。
- ブラウザのダウンロードフォルダに保存されます

## 7. ローカルでビューワで確認、oak-dで撮影したデータ（spectacularAI形式）をGoogle Colabにアップロードし、nerfstudioで学習・可視化するまでの一連の手順をまとめたものです。

## フロー概要
1. ローカル環境でspectacularAIのインストール
2. oak-dでデータ撮影（spectacularAI形式）
3. データ処理・NerfStudio形式に変換
4. データをGoogle Colabにアップロード
5. Colab上でnerfstudioをセットアップ・学習
6. 結果をダウンロード
7. ローカルでビューワで確認

---


## 1. ローカル環境でspectacularAIのインストール
```bash
pip install spectacularAI[full]
```

## 2. oak-dでデータ撮影
```bash
sai-cli record oak
# [q]キーで終了
```

## 3. データ処理・変換
- 3次元ビューワでプレビュー
```bash
sai-cli process --device_preset oak-d --preview3d .\data\2025-08-24_10-37-14 .\processed
```
- NerfStudio形式に変換
```bash
sai-cli process --device_preset oak-d --format nerfstudio .\data\2025-08-24_10-37-14 .\processed
```

---

## 4. データをGoogle Colabにアップロード
- `processed/` フォルダを右クリック → 圧縮 → `processed.zip` を作成
- Colabのファイルアップローダを使い、`processed.zip` をアップロード
- ノートブックが自動で展開します

## 5. Colab上でnerfstudioをセットアップ
- Colabノートブック（`colab_nerfstudio.ipynb`）を参照してく ださい。


## 6. nerfstudioで学習
- Colabノートブックの指示に従い、コマンドを実行します。

## 7. 結果をダウンロード
- Colabのファイルダウンロード機能で、学習済みモデルや画像をローカルに保存します。

## 8. ローカルでビューワで確認
- nerfstudio viewer等で結果を確認します。

---

## 参考
- [nerfstudio公式](https://docs.nerf.studio/)
- [spectacularAI公式](https://docs.spectacularai.com/)

---

## Colabノートブック
- `colab_nerfstudio.ipynb` を参照してください。
