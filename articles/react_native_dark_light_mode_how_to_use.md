---
title: "【react native】expo使ってOSのダークモードとライトモードを取得する"
emoji: "🚦"
type: "tech"
topics: ["reactnative", "expo"]
published: false
---

react nativeでOSのダークモードとライトモードを取得する方法がすごい簡単だったのでメモする。

## app.jsonの設定

`userInterfaceStyle`を使用して、アプリがlight mode, dark modeの両方をサポートしていることを明示的に記載する。automaticにすることで、システムの外観設定が変更された場合、通知される。

```json
{
  "expo": {
    "userInterfaceStyle": "automatic"
  }
}
```


## useColorSchemeを使用する

呼び出しもとで`useColorScheme`を使用することで、切り替えを実装できる。

```tsx
// 例
import { useColorScheme } from "react-native";

export default function App() {
  const colorMode = useColorScheme();

  return (
    <Component
      backgroundColor={colorMode === "light" ? White : Black}
    >
      <ChildComponent />
    </Component>
  );
}
```

## 参考

https://docs.expo.dev/develop/user-interface/color-themes/#configuration

https://docs.expo.dev/develop/user-interface/color-themes/
