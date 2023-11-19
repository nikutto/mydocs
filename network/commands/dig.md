# DNS resolution command

## 前置き

DNS resolutionするコマンドは以下がある。
- `dig`
- `host`
- `nslookup`

やっていることはどれも同じなので、`dig`だけを扱う。

## コマンド

```
dig google.com
dig -x 142.251.42.142
```

## 注意点

通常、`/etc/hosts`を参照しない。
ただし、loopback addressの場合は参照したりしていそう。
