# TradingAgents Framework Architecture

## Complete Solution Architecture

```mermaid
graph TB
    %% Configuration & Initialization
    subgraph CONFIG["🔧 Configuration & LLM Setup"]
        MAIN[main.py]
        DEFAULT_CONFIG[DEFAULT_CONFIG]
        LLM_PROVIDERS[LLM Providers]
        MAIN --> DEFAULT_CONFIG
        MAIN --> LLM_PROVIDERS
        
        subgraph LLM_TYPES["LLM Provider Options"]
            OPENAI[OpenAI/GPT]
            GOOGLE[Google Gemini]
            DASHSCOPE[Alibaba DashScope]
            DEEPSEEK[DeepSeek]
            ANTHROPIC[Anthropic Claude]
        end
        LLM_PROVIDERS --> LLM_TYPES
    end

    %% Core Framework
    subgraph CORE["🏗️ Core Framework"]
        TRADING_GRAPH[TradingAgentsGraph]
        GRAPH_SETUP[GraphSetup]
        PROPAGATOR[Propagator]
        CONDITIONAL_LOGIC[ConditionalLogic]
        
        TRADING_GRAPH --> GRAPH_SETUP
        TRADING_GRAPH --> PROPAGATOR
        TRADING_GRAPH --> CONDITIONAL_LOGIC
    end

    %% Data Sources & Tools
    subgraph DATA["📊 Data Sources & Tools"]
        TOOLKIT[Toolkit]
        
        subgraph DATA_SOURCES["Data Sources"]
            MARKET_DATA[Market Data APIs]
            NEWS_API[News APIs]
            SOCIAL_API[Social Media APIs]
            FUNDAMENTAL_API[Fundamental Data APIs]
        end
        
        subgraph TOOL_NODES["Tool Nodes"]
            MARKET_TOOLS[Market Tools]
            NEWS_TOOLS[News Tools]
            SOCIAL_TOOLS[Social Tools]
            FUND_TOOLS[Fundamental Tools]
        end
        
        TOOLKIT --> DATA_SOURCES
        TOOLKIT --> TOOL_NODES
    end

    %% Analyst Layer
    subgraph ANALYSTS["🔍 Analysis Layer"]
        direction TB
        
        subgraph ANALYST_TEAM["👥 Analyst Team"]
            MARKET_ANALYST[Market Analyst]
            FUND_ANALYST[Fundamentals Analyst]
            NEWS_ANALYST[News Analyst]
            SOCIAL_ANALYST[Social Media Analyst]
        end
        
        MARKET_ANALYST --> MARKET_TOOLS
        FUND_ANALYST --> FUND_TOOLS
        NEWS_ANALYST --> NEWS_TOOLS
        SOCIAL_ANALYST --> SOCIAL_TOOLS
    end

    %% Research Layer
    subgraph RESEARCH["💭 Research & Debate Layer"]
        direction TB
        
        RESEARCH_MANAGER[Research Manager]
        
        subgraph DEBATE_TEAM["⚖️ Debate Team"]
            BULL_RESEARCHER[Bull Researcher]
            BEAR_RESEARCHER[Bear Researcher]
        end
        
        ANALYST_TEAM --> RESEARCH_MANAGER
        RESEARCH_MANAGER --> DEBATE_TEAM
        BULL_RESEARCHER -.-> BEAR_RESEARCHER
        BEAR_RESEARCHER -.-> BULL_RESEARCHER
    end

    %% Trading Layer
    subgraph TRADING["💼 Trading Execution Layer"]
        TRADER[Trader Agent]
        
        DEBATE_TEAM --> TRADER
        RESEARCH_MANAGER --> TRADER
    end

    %% Risk Management Layer
    subgraph RISK["🛡️ Risk Management Layer"]
        direction TB
        
        subgraph RISK_ASSESSORS["Risk Assessment Team"]
            RISKY_ANALYST[Aggressive Risk Analyst]
            SAFE_ANALYST[Conservative Risk Analyst]
            NEUTRAL_ANALYST[Neutral Risk Analyst]
        end
        
        RISK_MANAGER[Risk Manager/Judge]
        
        TRADER --> RISK_ASSESSORS
        RISK_ASSESSORS --> RISK_MANAGER
        RISKY_ANALYST -.-> SAFE_ANALYST
        SAFE_ANALYST -.-> NEUTRAL_ANALYST
    end

    %% Memory & Learning
    subgraph MEMORY["🧠 Memory & Learning"]
        direction TB
        
        subgraph MEMORY_STORES["Memory Stores"]
            BULL_MEMORY[Bull Memory]
            BEAR_MEMORY[Bear Memory]
            TRADER_MEMORY[Trader Memory]
            JUDGE_MEMORY[Judge Memory]
            RISK_MEMORY[Risk Manager Memory]
        end
        
        REFLECTOR[Reflector]
        SIGNAL_PROCESSOR[Signal Processor]
        
        BULL_RESEARCHER --> BULL_MEMORY
        BEAR_RESEARCHER --> BEAR_MEMORY
        TRADER --> TRADER_MEMORY
        RESEARCH_MANAGER --> JUDGE_MEMORY
        RISK_MANAGER --> RISK_MEMORY
        
        RISK_MANAGER --> REFLECTOR
        RISK_MANAGER --> SIGNAL_PROCESSOR
    end

    %% Output & Decision
    subgraph OUTPUT["📋 Output & Decision"]
        FINAL_DECISION[Final Trading Decision]
        STATE_LOGGING[State Logging]
        
        RISK_MANAGER --> FINAL_DECISION
        FINAL_DECISION --> STATE_LOGGING
    end

    %% Data Flow Connections
    CONFIG --> CORE
    CORE --> DATA
    DATA --> ANALYSTS
    ANALYSTS --> RESEARCH
    RESEARCH --> TRADING
    TRADING --> RISK
    RISK --> OUTPUT
    MEMORY -.-> ANALYSTS
    MEMORY -.-> RESEARCH
    MEMORY -.-> TRADING
    MEMORY -.-> RISK

    %% Styling
    classDef configStyle fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef coreStyle fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef dataStyle fill:#e8f5e8,stroke:#1b5e20,stroke-width:2px
    classDef analystStyle fill:#fff3e0,stroke:#e65100,stroke-width:2px
    classDef researchStyle fill:#fce4ec,stroke:#880e4f,stroke-width:2px
    classDef tradingStyle fill:#e0f2f1,stroke:#004d40,stroke-width:2px
    classDef riskStyle fill:#ffebee,stroke:#b71c1c,stroke-width:2px
    classDef memoryStyle fill:#f1f8e9,stroke:#33691e,stroke-width:2px
    classDef outputStyle fill:#e8eaf6,stroke:#1a237e,stroke-width:2px

    class CONFIG,MAIN,DEFAULT_CONFIG,LLM_PROVIDERS,LLM_TYPES configStyle
    class CORE,TRADING_GRAPH,GRAPH_SETUP,PROPAGATOR,CONDITIONAL_LOGIC coreStyle
    class DATA,TOOLKIT,DATA_SOURCES,TOOL_NODES dataStyle
    class ANALYSTS,ANALYST_TEAM analystStyle
    class RESEARCH,RESEARCH_MANAGER,DEBATE_TEAM researchStyle
    class TRADING,TRADER tradingStyle
    class RISK,RISK_ASSESSORS,RISK_MANAGER riskStyle
    class MEMORY,MEMORY_STORES,REFLECTOR,SIGNAL_PROCESSOR memoryStyle
    class OUTPUT,FINAL_DECISION,STATE_LOGGING outputStyle
```

## Detailed Component Flow

```mermaid
sequenceDiagram
    participant User as User/main.py
    participant TG as TradingAgentsGraph
    participant Analysts as Analyst Team
    participant Research as Research Team
    participant Trader as Trader Agent
    participant Risk as Risk Management
    participant Memory as Memory System

    %% Initialization
    User->>TG: Initialize with config & selected analysts
    TG->>TG: Setup LLM providers
    TG->>TG: Create tool nodes & memory stores
    TG->>TG: Build agent graph

    %% Analysis Phase
    User->>TG: propagate(stock_symbol, date)
    TG->>Analysts: Start parallel analysis
    
    %% Analyst Work (Parallel)
    par Market Analysis
        Analysts->>Analysts: Market Analyst + Tools
    and Fundamental Analysis
        Analysts->>Analysts: Fundamentals Analyst + Tools
    and News Analysis
        Analysts->>Analysts: News Analyst + Tools
    and Social Analysis
        Analysts->>Analysts: Social Media Analyst + Tools
    end

    %% Research & Debate Phase
    Analysts->>Research: Pass analysis reports
    Research->>Research: Research Manager coordinates
    
    loop Debate Rounds (max 3)
        Research->>Research: Bull Researcher argues
        Research->>Research: Bear Researcher counters
        Research->>Memory: Store debate points
    end
    
    Research->>Research: Generate consensus

    %% Trading Decision
    Research->>Trader: Pass research consensus
    Trader->>Trader: Synthesize all inputs
    Trader->>Trader: Generate trading plan
    Trader->>Memory: Store trading rationale

    %% Risk Assessment
    Trader->>Risk: Submit trading plan
    
    loop Risk Evaluation Rounds
        Risk->>Risk: Aggressive Risk Analyst
        Risk->>Risk: Conservative Risk Analyst
        Risk->>Risk: Neutral Risk Analyst
    end
    
    Risk->>Risk: Risk Manager evaluates
    Risk->>Memory: Store risk assessment

    %% Final Decision
    Risk->>TG: Final trading decision
    TG->>Memory: Log complete state
    TG->>User: Return decision & state

    %% Learning (Optional)
    User->>TG: reflect_and_remember(returns)
    TG->>Memory: Update memories with performance
```

## Agent Interaction Pattern

```mermaid
graph LR
    subgraph "🔄 Agent Flow Pattern"
        direction TB
        
        A1[Agent Node] --> C1{Conditional Logic}
        C1 -->|Need Tools| T1[Tool Node]
        C1 -->|Complete| M1[Message Clear]
        T1 --> A1
        M1 --> A2[Next Agent]
        
        A2 --> C2{Conditional Logic}
        C2 -->|Need Tools| T2[Tool Node]
        C2 -->|Complete| M2[Message Clear]
        T2 --> A2
        M2 --> A3[Next Agent]
    end

    subgraph "📊 Tool Categories"
        direction TB
        
        MARKET_T[Market Tools<br/>• Stock data<br/>• Technical indicators<br/>• Price history]
        FUND_T[Fundamental Tools<br/>• Financial statements<br/>• Company metrics<br/>• Insider data]
        NEWS_T[News Tools<br/>• Global news<br/>• Company news<br/>• Economic events]
        SOCIAL_T[Social Tools<br/>• Reddit sentiment<br/>• Social mentions<br/>• Community mood]
    end

    subgraph "🧠 Memory Integration"
        direction TB
        
        EXPERIENCE[Historical Performance]
        PATTERNS[Market Patterns]
        LESSONS[Learned Lessons]
        CONTEXT[Situational Context]
    end
```

## Configuration & Model Management

```mermaid
graph TD
    subgraph "⚙️ Model Configuration Flow"
        USER_CONFIG[User Configuration] --> LLM_SELECTION{LLM Provider Selection}
        
        LLM_SELECTION -->|openai| OPENAI_SETUP[OpenAI Setup<br/>• GPT-4o-mini (quick)<br/>• o4-mini (deep)]
        LLM_SELECTION -->|google| GOOGLE_SETUP[Google Setup<br/>• Gemini models<br/>• API key config]
        LLM_SELECTION -->|dashscope| ALIBABA_SETUP[Alibaba DashScope<br/>• Qwen models<br/>• Chinese optimization]
        LLM_SELECTION -->|deepseek| DEEPSEEK_SETUP[DeepSeek Setup<br/>• V3 models<br/>• Token tracking]
        LLM_SELECTION -->|anthropic| CLAUDE_SETUP[Anthropic Setup<br/>• Claude models<br/>• Constitutional AI]
        
        OPENAI_SETUP --> MODEL_INIT[Model Initialization]
        GOOGLE_SETUP --> MODEL_INIT
        ALIBABA_SETUP --> MODEL_INIT
        DEEPSEEK_SETUP --> MODEL_INIT
        CLAUDE_SETUP --> MODEL_INIT
        
        MODEL_INIT --> AGENT_ASSIGNMENT[Agent Assignment<br/>• Deep thinking LLM<br/>• Quick thinking LLM]
    end

    subgraph "🎯 Model Usage Strategy"
        DEEP_TASKS[Deep Thinking Tasks<br/>• Research management<br/>• Risk evaluation<br/>• Final decisions]
        QUICK_TASKS[Quick Thinking Tasks<br/>• Data analysis<br/>• Tool calls<br/>• Intermediate steps]
        
        AGENT_ASSIGNMENT --> DEEP_TASKS
        AGENT_ASSIGNMENT --> QUICK_TASKS
    end
```

## Key Configuration Points for Language Model Changes

### 1. **Primary Configuration (main.py)**
```python
config["llm_provider"] = "your_provider"     # Provider selection
config["deep_think_llm"] = "model_name"      # Complex reasoning
config["quick_think_llm"] = "model_name"     # Fast operations
config["backend_url"] = "api_endpoint"       # API endpoint
```

### 2. **Supported Providers & Models**
- **OpenAI**: GPT-4o, GPT-4o-mini, o1-preview, o4-mini
- **Google**: Gemini-2.0-flash, Gemini-pro
- **Alibaba DashScope**: Qwen-plus, Qwen-turbo, Qwen-max
- **DeepSeek**: DeepSeek-V3, DeepSeek-chat
- **Anthropic**: Claude-3.5-sonnet, Claude-3-haiku

### 3. **Model Assignment Strategy**
- **Deep Thinking LLM**: Used for Research Manager, Risk Manager, final decisions
- **Quick Thinking LLM**: Used for Analysts, Researchers, Trader, tool operations

### 4. **Key Customization Points**
- **Agent Selection**: Choose which analysts to include
- **Debate Rounds**: Control discussion depth
- **Tool Configuration**: Online vs offline data sources
- **Memory Integration**: Enable/disable learning
- **Risk Tolerance**: Adjust risk assessment parameters
