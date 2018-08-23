# Graph Plotter

csvファイルを読んでいい感じにグラフを作る

X軸とプロット対象はデータから自由に選択できる

## 実装状況

- [x] 折れ線グラフ
- [x] 散布図
- [ ] 棒グラフ
- [ ] レーダーチャート
- [ ] バブルチャート
- [ ] データに応じてグラフの種類選択を制限
- [ ] グラフ画像の保存
- [ ] ファイルの再読込
- [ ] グラフの並び替え
- [ ] グラフの描画範囲変更

## 使用ライブラリ

- Vue.js
- Vuetify.js
  - vue-cli-plugin-vuetify
- Electron
  - vue-cli-plugin-electron-builder
- Chart.js
  - vue-chartjs

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve:electron
```

### Compiles and minifies for production
```
npm run build:electron
```

### Lints and fixes files
```
npm run lint
```
