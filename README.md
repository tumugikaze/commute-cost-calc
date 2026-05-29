# commute-cost-calc

通勤交通費の月次計算ツール片道運賃と利用日をもとに、その月の交通費合計と利用日一覧を出力する

## Features

- 片道運賃と利用日数から往復交通費を自動計算
- カレンダーUIで複数日を直感的に選択
- 利用日を `M/D, M/D` 形式で一覧表示
- ダークモード対応

## Stack

| カテゴリ | 技術 |
|---|---|
| Framework | Vue 3 |
| Build Tool | Vite |
| UI Library | Vuetify |
| CSS | Tailwind CSS |
| Language | TypeScript |
| Linter | Biome |

## Getting Started

```bash
npm install
npm run dev
```

## Usage

1. **片道運賃** を入力する
2. **カレンダー** から利用した日付を複数選択する
3. **合計金額** と **利用日一覧** が自動的に表示される

## Output Format

| 項目 | 形式 | 例 |
|---|---|---|
| 合計金額 | 円表示 | `¥4,840` |
| 利用日一覧 | `M/D` をカンマ+スペース区切り | `1/6, 1/7, 1/8` |

## Calculation

```
合計金額 = 片道運賃 × 2 × 利用日数
```

## Project Structure

```
src/
├── App.vue
├── main.ts
└── components/
    └── CommuteCostCalc.vue
```