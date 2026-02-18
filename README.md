# sing-box-releases-with-traffic-stats

这是 [SagerNet/sing-box](https://github.com/SagerNet/sing-box) 的一个编译产物

使用 `go build -v -tags "with_quic with_dhcp with_wireguard with_utls with_acme with_v2ray_api with_gvisor with_tailscale" ./cmd/sing-box` 命令编译，与官方编译tag相比，移除了 `with_clash_api` tag, 添加了 `with_v2ray_api` tag，可以进行流量统计。

在 [v2fly/v2ray-core/releases](https://github.com/v2fly/v2ray-core/releases/latest) 页面可以下载 `v2ray-linux-64.zip`。

也可以使用以下 bash 脚本下载

```bash
curl -L -o /tmp/v2ray-linux-64.zip $( curl -s https://api.github.com/repos/v2fly/v2ray-core/releases/latest | jq -r '.assets[] | select(.name == "v2ray-linux-64.zip") | .browser_download_url' )
unzip /tmp/v2ray-linux-64.zip v2ray
```

运行 `./sing-box -c ./config.json run` 命令时，注意修改 `config.json` 文件，添加以下内容

```jsonc
{
  "experimental": {
    "v2ray_api": {
      "listen": "0.0.0.0:9090",
      "stats": {
        "enabled": true,
        "inbounds": [
          // inbounds-tags
        ],
        "outbounds": [
          // outbounds-tags
        ],
        "users": [
          // usernames
        ]
      }
    }
  }
}
```

然后使用以下命令，读取流量统计

```
./v2ray api stats --server 127.0.0.1:9090 --json
```

# Note

不删除 `v2ray` 其中的不需要的功能，因为 golang 编译产物大小主要是 golang 本体，删除不需要的功能几乎不减少体积。
