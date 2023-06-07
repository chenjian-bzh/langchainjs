# 🦜️🔗 @cloudfun/langchain

@cloudfun/langchain 内部引用 cloudfun-openai , 改写了 OpenAI 相关的 API 和鉴权传值，目前仅支持 completion、embedding、chat 相关接口

## Install

```javascript
yarn add @cloudfun/langchain
```

## Usage

将内部申请的 jwt token 作为 openai 的 accesskey 传入， 同时需要指定 basePath

### completion

```javascript
import { ChatOpenAI } from "@cloudfun/langchain/chat_models/openai";
import { LLMChain } from "@cloudfun/langchain/chains";
import {
  ChatPromptTemplate,
  SystemMessagePromptTemplate,
  HumanMessagePromptTemplate,
} from "@cloudfun/langchain/prompts";

const llm = new ChatOpenAI(
  {
    temperature: 0,
    openAIApiKey: "xx",
  },
  {
    basePath: "http://bychat-boe.byted.org/api/gpt/openapi/online/v2/crawl",
  }
);

const chatPrompt = ChatPromptTemplate.fromPromptMessages([
  SystemMessagePromptTemplate.fromTemplate(
    "You are a helpful assistant that translates {input_language} to {output_language}."
  ),
  HumanMessagePromptTemplate.fromTemplate("{text}"),
]);
const chain = new LLMChain({ llm: llm, prompt: chatPrompt });

const res = await chain.call({
  input_language: "English",
  output_language: "French",
  text: "I love programming.",
});
```

### embedding

如果要使用 openai embedding 的接口, 在声明 OpenAIEmbeddings 的时候传入自己的 token 和 basePath

```javascript
new OpenAIEmbeddings(
  { openAIApiKey: JWT_TOKEN, timeout: 10000, verbose: true, maxRetries: 1 },
  { basePath: OEPNAI_BASEPATH }
);
```
