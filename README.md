# uniai

> 本SDK是一个精心设计的开发工具包，它不仅完美兼容 `OpenAI` 与 `xfyun`  的API标准，还能统一调用两者的服务，采用与 `OpenAI` 一致的内容格式进行数据输出，极大地简化了开发者在不同平台间切换的工作流程。

### 安装方式
```shell
go get -u github.com/jun3372/uniai
```

### 使用方式
> 更多渠道的示例请参考 [示例](https://github.com/jun3372/uniai/tree/main/example)

```golang
package main

import (
	"fmt"

	"github.com/bytedance/sonic"
	"github.com/jun3372/uniai"
	"github.com/jun3372/uniai/client"
)

func mian() {
	token := "授权用的 token"
	chat := uniai.New(
		client.WithType(client.OpenAI),
		client.WithHost("https://dashscope.aliyuncs.com/compatible-mode"),
		client.AddHeader("Authorization", token),
	)

	in := *request.NewRequest(
		request.WithModel("qwen2-72b-instruct"),
		request.WithStream(true),
		request.WithMessages([]request.Messages{
			request.NewMessage(request.MessageRoleSystem, "你是一位现代诗人，能够轻松的写出李白和杜甫的风格的诗词"),
			request.NewMessage(request.MessageRoleUser, "请帮我写一首关于春天的诗"),
		}),
	)

	resp, err := chat.Completions(context.Background(), in)
	if err != nil {
		panic(err)
	}

	for item := range resp {
		bs, _ := sonic.ConfigDefault.MarshalToString(item)
		fmt.Println("resp.Item=", bs)
	}
}
```