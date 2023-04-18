# damage-calc
[![Damage Calculation Test](https://github.com/saitoyoshi/damage-calc-4005-gh-actions/actions/workflows/main.yml/badge.svg)](https://github.com/saitoyoshi/damage-calc-4005-gh-actions/actions/workflows/main.yml)

このモジュールでは、ダメージ計算を行うことができます。

計算モジュールに

- 攻撃力
- 防御力
- 防御力貫通

の 3 つの値を入力することで、その時の**ダメージ**を計算して出力します。

計算方法は以下の通りです。

- 3 つの入力値の範囲は、下限が 0 で、上限が 2000 です。
  そのため、入力値が負の値だった場合 0 、2000 以上だった場合は 2000 として扱います。

- 実効防御力は、防御力 - 防御力貫通 を計算します。
  つまり、防御力貫通を指定した場合、その分だけ防御力が減少することになります。
  この実効防御力の下限は 0 です。負の値になった場合、0 として扱います。

- ダメージ減少率は、実効防御力 / (100 + 実効防御力) で計算します。
  結果、ダメージ減少率は 0 ～ 1 未満の範囲をとり、実行防御力が高いほど 1 に近い値になります。

- ダメージは、攻撃力 * (1 - ダメージ減少率) で計算します。
  計算後、小数点以下については四捨五入し、ダメージには整数値が返されます。

## 使い方

```js
var dc = require('.');
console.log(dc.effectiveDamage(100, 50, 30));
```

以上を実行すると、

```
83
```

と表示されます。

上の例では、攻撃力が 100、防御力が 50、防御力貫通が 30 で指定されています。
実効防御力は、 50 - 30 で 20 となります。
ダメージ減少率は、 20 / (100 + 20) を計算し、20 / 120 を約分すると 1 / 6 となります。
ダメージは、 100 * (1 - (1 / 6)) = 100 * 5 / 6 を計算した 83.33333... となり、四捨五入して 83 となります。
