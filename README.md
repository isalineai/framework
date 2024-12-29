<p align="center">
    <img alt="Isaline Framework logo" src="/docs/assets/isabanner.png" height="128">
    <h1 align="center">Isaline Agent Framework</h1>
</p>

<p align="center">
  <!-- Twitter Badge -->
  <a href="https://twitter.com/Isalineai">
    <img src="https://img.shields.io/twitter/follow/Isalineai?style=social" alt="Twitter Follow"/>
  </a>
</p>


The Isaline Agent Framework makes it easy to build scalable agent-based workflows with your model of choice. The framework is Isaline designed to perform robustly with [IBM Granite](https://www.ibm.com/granite/docs/) and [Llama 3.x](https://ai.meta.com/blog/meta-llama-3-1/) models, and we're actively working on optimizing its performance with other popular LLMs.<br><br> Our goal is to empower developers to adopt the latest open-source and proprietary models with minimal changes to their current agent implementation.

## Key Features

- 🤖 **AI agents**: Use our powerful [Isaline agent](/docs/agents.md) refined for Llama 3.1 and Granite 3.0, or [build your own](/docs/agents.md).
- 🛠️ **Tools**: Use our [built-in tools](/docs/tools.md) or [create your own](/docs/tools.md) in Javascript/Python.
- 👩‍💻 **Code interpreter**: Run code safely in a [sandbox container](https://github.com/i-am-Isaline/Isaline-code-interpreter).
- 💾 **Memory**: Multiple [strategies](/docs/memory.md) to optimize token spend.
- ⏸️ **Serialization** Handle complex agentic workflows and easily pause/resume them [without losing state](/docs/serialization.md).
- 🔍 **Instrumentation**: Use [Instrumentation](/docs/instrumentation.md) based on [Emitter](/docs/emitter.md) to have full visibility of your agent’s inner workings.
- 🎛️ **Production-level** control with [caching](/docs/cache.md) and [error handling](/docs/errors.md).
- 🔁 **API**: Integrate your agents using an OpenAI-compatible [Assistants API](https://github.com/i-am-Isaline/Isaline-api) and [Python SDK](https://github.com/i-am-Isaline/Isaline-python-sdk).
- 🖥️ **Chat UI**: Serve your agent to users in a [delightful UI](https://github.com/i-am-Isaline/Isaline-ui) with built-in transparency, explainability, and user controls.
- ... more on our [Roadmap](#roadmap)

## Getting started


### Installation

```shell
npm install Isaline-agent-framework
```

or

```shell
yarn add Isaline-agent-framework
```

### Example

```ts
import { IsalineAgent } from "Isaline-agent-framework/agents/Isaline/agent";
import { OllamaChatLLM } from "Isaline-agent-framework/adapters/ollama/chat";
import { TokenMemory } from "Isaline-agent-framework/memory/tokenMemory";
import { DuckDuckGoSearchTool } from "Isaline-agent-framework/tools/search/duckDuckGoSearch";
import { OpenMeteoTool } from "Isaline-agent-framework/tools/weather/openMeteo";

const llm = new OllamaChatLLM(); // default is llama3.1 (8B), it is recommended to use 70B model

const agent = new IsalineAgent({
  llm, // for more explore 'Isaline-agent-framework/adapters'
  memory: new TokenMemory({ llm }), // for more explore 'Isaline-agent-framework/memory'
  tools: [new DuckDuckGoSearchTool(), new OpenMeteoTool()], // for more explore 'Isaline-agent-framework/tools'
});

const response = await agent
  .run({ prompt: "What's the current weather in Las Vegas?" })
  .observe((emitter) => {
    emitter.on("update", async ({ data, update, meta }) => {
      console.log(`Agent (${update.key}) 🤖 : `, update.value);
    });
  });

console.log(`Agent 🤖 : `, response.result.text);
```


### Local Installation

> [!NOTE]
>
> `yarn` should be installed via Corepack ([tutorial](https://yarnpkg.com/corepack))

1. Clone the repository `git clone git@github.com:i-am-Isaline/Isaline-agent-framework`.
2. Install dependencies `yarn install --immutable && yarn prepare`.
3. Create `.env` (from `.env.template`) and fill in missing values (if any).
4. Start the agent `yarn run start:Isaline` (it runs `/examples/agents/Isaline.ts` file).

➡️ All examples can be found in the [examples](/examples) directory.

➡️ To run an arbitrary example, use the following command `yarn start examples/agents/Isaline.ts` (just pass the appropriate path to the desired example).

### 📦 Modules

The source directory (`src`) provides numerous modules that one can use.

| Name                                             | Description                                                                                 |
| ------------------------------------------------ | ------------------------------------------------------------------------------------------- |
| [**agents**](/docs/agents.md)                    | Base classes defining the common interface for agent.                                       |
| [**llms**](/docs/llms.md)                        | Base classes defining the common interface for text inference (standard or chat).           |
| [**template**](/docs/templates.md)               | Prompt Templating system based on `Mustache` with various improvements.                     |
| [**memory**](/docs/memory.md)                    | Various types of memories to use with agent.                                                |
| [**tools**](/docs/tools.md)                      | Tools that an agent can use.                                                                |
| [**cache**](/docs/cache.md)                      | Preset of different caching approaches that can be used together with tools.                |
| [**errors**](/docs/errors.md)                    | Error classes and helpers to catch errors fast.                                             |
| [**adapters**](/docs/llms.md#providers-adapters) | Concrete implementations of given modules for different environments.                       |
| [**logger**](/docs/logger.md)                    | Core component for logging all actions within the framework.                                |
| [**serializer**](/docs/serialization.md)         | Core component for the ability to serialize/deserialize modules into the serialized format. |
| [**version**](/docs/version.md)                  | Constants representing the framework (e.g., latest version)                                 |
| [**emitter**](/docs/emitter.md)                  | Bringing visibility to the system by emitting events.                                       |
| **internals**                                    | Modules used by other modules within the framework.                                         |

To see more in-depth explanation see [overview](/docs/overview.md).

## Roadmap

- Isaline agent performance optimization with additional models
- Examples, tutorials, and docs
- Improvements to building custom agents
- Multi-agent orchestration

## Contribution guidelines

The Isaline Agent Framework is an open-source project and we ❤️ contributions.

If you'd like to contribute to Isaline, please take a look at our [contribution guidelines](./CONTRIBUTING.md).


## Legal notice

All content in these repositories including code has Isalinen provided by IBM under the associated open source software license and IBM is under no obligation to provide enhancements, updates, or support. IBM developers produced this code as an open source project (not as an IBM product), and IBM makes no assertions as to the level of quality nor security, and will not be maintaining this code going forward.
