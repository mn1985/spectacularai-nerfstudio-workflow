# oak-d × spectacularAI × nerfstudio ワークフロー

このリポジトリは、oak-dで撮影したデータ（spectacularAI形式）をGoogle Colabにアップロードし、nerfstudioで学習・可視化するまでの一連の手順をまとめたものです。

## フロー概要
1. oak-dでデータ撮影（spectacularAI形式）
2. データをGoogle Colabにアップロード
3. Colab上でnerfstudioをセットアップ
4. spectacularAIデータをnerfstudio用に変換（必要に応じて）
5. nerfstudioで学習
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
- Colabのファイルアップローダを使い、`data/`フォルダごとアップロードします。

## 5. Colab上でnerfstudioをセットアップ
- Colabノートブック（`colab_nerfstudio.ipynb`）を参照してください。


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
