# efb-qq-plugin-oicqhttp

efb-qq-plugin-oicqhttp 是 efb-qq-slave 的插件，需要配合 efb-qq-slave 使用，使用前请先阅读 [efb-qq-slave](https://github.com/milkice233/efb-qq-slave) 的文档。

下面的教程展示了当 oicq 和 ehForwarderBot 在同一台机器上运行时如何设置两端。

（高级） 对于其他的情况，例如 oicq 和 ehForwarderBot 在不同的机器上运行时， `oicq port url` 和 `oicq api url` 必须修改为相应的值（前者是 efb-qq-slave 监听的地址/端口，后者是 oicq 监听的地址/端口），同时防火墙应允许双方的数据包通过，以便双方的请求不会被防火墙拦截。如果双方通信内容必须经过 Internet 传输，请确保已配置 ``Access Token`` 并启用 ``HTTPS`` 确保双方通信内容不会在公网被窃听/篡改。

有关 oicq 的详细信息，请访问 [oicq 文档](https://github.com/takayama-lily/oicq/tree/master/http-api)。

## 配置 oicq


1. 安装 oicq


```bash
npm i oicq@1 -g
oicq # 生成配置文件
```

2. 编辑 `config.js` 配置文件， 默认在 `~/.oicq` 下，

```js
module.exports = {
    general: {
        use_cqhttp_notice:  true,       //使用cqhttp标准的notice事件格式
        host:               "0.0.0.0",  //监听主机名
        port:               5700,       //端口
        use_http:           true,       //启用http
        access_token:       "",         //访问api的token
        secret:             "",         //上报数据的sha1签名密钥
        post_message_format:"array",    //"string"或"array"
        post_url: [                     //上报地址
	        "http://127.0.0.1:8000",
        ],
    },
};
```

3. 运行 oicq： `oicq <qq number>`

注意，关于滑动验证，请见 [滑动验证助手](https://github.com/mzdluo123/TxCaptchaHelper)。

## 配置 efb-qq-slave 端


1. 安装 efb-qq-plugin-oicqhttp `pip install git+https://github.com/youxam/efb-qq-plugin-oicqhttp`

2. 为 `milkice.qq` 从端创建 `config.yaml` 配置文件

   配置文件通常位于 `~/.ehforwarderbot/profiles/default/milkice.qq/config.yaml`

   样例配置文件如下:

   ```yaml

       Client: OICQHttp                      # 指定要使用的 QQ 客户端（此处为 OICQHttp
       OICQHttp:
           type: HTTP                        # 指定 efb-qq-plugin-oicqhttp 与 oicq 通信的方式 现阶段仅支持 HTTP
           access_token:
           api_root: http://127.0.0.1:5700/  # OICQHttp API接口地址/端口
           host: 127.0.0.1                   # efb-qq-slave 所监听的地址用于接收消息
           port: 8000                        # 同上
    ```

3. 启动 `ehforwarderbot`，大功告成！
