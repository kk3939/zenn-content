---
title: "【react native】EAS update APIを使用してアプリが起動するたびにアップデートを自動で取得するようにする"
emoji: "🚘"
type: "tech"
topics: ["reactnative", "expo"]
published: false
---

react nativeのEAS updateではJS部分をストアを介さずに更新できて、スピード感を持った開発を実現しています。
しかしながら、再起動を2,3回しないと更新が適用されないのがちょこっとしんどい部分だと思います。（[参考](https://docs.expo.dev/eas-update/getting-started/#publish-an-update)）

EAS update APIをうまく使うことで、カスタム戦略を簡単に取れたのでメモします。

## app.jsonの設定

アカウント情報を記載しておきます。

```json:app.json
{
  "expo": {
    "plugins": [
      [
        "expo-updates",
        {
          "username": "account-username"
        }
      ]
    ]
  }
}
```

## EAS update APIを使う

EAS update APIを使用することで、起動時にアップデートを確認・ダウンロードすることができます。

```tsx:App.tsx
useEffect(() => {
    const onFetchUpdateAsync = async () => {
      try {
        if (process.env.NODE_ENV === "development") {
          return;
        }
        const update = await Updates.checkForUpdateAsync();
        if (update.isAvailable) {
          await Updates.fetchUpdateAsync();
          await Updates.reloadAsync();
        }
      } catch (error) {
        alert(`Error fetching latest update: ${error}`);
      }
    };

    onFetchUpdateAsync();
  }, []);
```

注意点としては、`fetchUpdateAsync`は開発モードではpromiseがrejectされる点です。
https://docs.expo.dev/versions/latest/sdk/updates/#updatesfetchupdateasync

また通信を行うので、機内モードにしているとエラーが発生します。

## 参考
https://docs.expo.dev/eas-update/getting-started/
