# Spec Kit and Beyond Workshop - Flow Diagrams

## Main Workshop Flow

This diagram shows the complete 2-hour workshop flow with all agent types clearly labeled.

```mermaid
flowchart TD
    Start["🔥 CRISIS START: 3:00 PM<br/>NULL_DIETARY_BUG: Search crashes for 30% users"]
    
    subgraph Exp1["<b>EXPERIMENT 1: Crisis Response</b><br/>⏱️ 3:00 PM - 3:20 PM (20 min)"]
        E1A["📊 Agent Skill<br/>.github/skills/bug-analyzer/SKILL.md<br/>Autonomous error log analysis"]
        E1B["🔗 GitHub MCP<br/>Programmatic GitHub operations<br/>Document NULL_DIETARY_BUG"]
        E1C["✅ Outcome: Issue documented<br/>with root cause analysis"]
        E1A --> E1B --> E1C
    end
    
    subgraph Exp2["<b>EXPERIMENT 2: Root Cause Analysis</b><br/>⏱️ 3:20 PM - 3:45 PM (25 min)"]
        E2A["🤖 Custom Agent<br/>.github/agents/search-architect.agent.md<br/>Architecture expert analysis"]
        E2B["🔍 Deep Codebase Scan<br/>Analyzes 289-line file<br/>Recommends patch + validation"]
        E2C["✅ Outcome: Practical recommendation<br/>Add validation, keep architecture"]
        E2A --> E2B --> E2C
    end
    
    subgraph Exp3["<b>EXPERIMENT 3: Solution Design</b><br/>⏱️ 3:45 PM - 4:10 PM (25 min)<br/>🎯 DESIGN + SPEC"]
        E3A["📝 Instruction Files<br/>.github/instructions/search-domain.instructions.md<br/>Domain knowledge for all agents"]
        E3B["⚖️ /speckit.constitution<br/>Establish governance principles<br/>from architect recommendations"]
        E3C["📋 /speckit.specify<br/>Generate detailed specification<br/>Validation changes + success criteria"]
        E3D["✅ Outcome: Complete blueprint<br/>with governance layer"]
        E3A --> E3B --> E3C --> E3D
    end
    
    subgraph Exp4["<b>EXPERIMENT 4: Implementation</b><br/>⏱️ 4:10 PM - 4:40 PM (30 min)<br/>🎯 PLAN + CODE"]
        E4A["📝 /speckit.tasks<br/>Break spec into ordered tasks<br/>5 phases with dependencies"]
        E4B["⚙️ Copilot CLI<br/>gh copilot suggest<br/>Multi-step setup automation"]
        E4C["🔧 /speckit.implement<br/>Spec-driven code generation<br/>QueryParser + Tests"]
        E4D["💬 @workspace Coding Agent<br/>Conversational code generation<br/>FilterEngine + Custom logic"]
        E4E["✅ Outcome: Bug fixed + system<br/>refactored with tests passing"]
        E4A --> E4B --> E4C --> E4D --> E4E
    end
    
    subgraph Exp5["<b>EXPERIMENT 5: Validation</b><br/>⏱️ 4:40 PM - 5:00 PM (20 min)<br/>🎯 BEYOND: Quality Gates"]
        E5A["🔎 /speckit.analyze<br/>Validate constitution compliance<br/>Identify gaps"]
        E5B["✓ /speckit.checklist<br/>Generate quality checklist<br/>Systematic verification"]
        E5C["✅ Outcome: Production-ready<br/>87% coverage, <100ms performance"]
        E5A --> E5B --> E5C
    end
    
    End["🎉 CRISIS RESOLVED: 5:00 PM<br/>System modernized in 2 hours<br/>vs 3-4 weeks traditional"]
    
    Start --> Exp1
    Exp1 --> Exp2
    Exp2 --> Exp3
    Exp3 --> Exp4
    Exp4 --> Exp5
    Exp5 --> End
    
    classDef crisisStyle fill:#ff6b6b,stroke:#c92a2a,stroke-width:3px,color:#fff
    classDef agentSkillStyle fill:#4dabf7,stroke:#1971c2,stroke-width:2px,color:#fff
    classDef customAgentStyle fill:#69db7c,stroke:#2b8a3e,stroke-width:2px,color:#fff
    classDef speckitStyle fill:#ffd43b,stroke:#f08c00,stroke-width:2px,color:#000
    classDef cliStyle fill:#da77f2,stroke:#9c36b5,stroke-width:2px,color:#fff
    classDef codingAgentStyle fill:#ff922b,stroke:#e8590c,stroke-width:2px,color:#fff
    classDef outcomeStyle fill:#51cf66,stroke:#2f9e44,stroke-width:2px,color:#fff
    classDef successStyle fill:#20c997,stroke:#087f5b,stroke-width:3px,color:#fff
    
    class Start,End crisisStyle
    class E1A agentSkillStyle
    class E1B agentSkillStyle
    class E2A,E2B customAgentStyle
    class E3A,E3B,E3C speckitStyle
    class E4A,E4C speckitStyle
    class E4B cliStyle
    class E4D codingAgentStyle
    class E5A,E5B speckitStyle
    class E1C,E2C,E3D,E4E,E5C outcomeStyle
```

---

## Legend & Theme Mapping

```mermaid
graph LR
    subgraph Legend["🎨 AGENT & TOOL LEGEND"]
        AS["📊 Agent Skills<br/>.github/skills/*.md<br/>Autonomous analysis"]
        MCP["🔗 GitHub MCP<br/>Programmatic GitHub ops"]
        CA["🤖 Custom Agent<br/>.github/agents/*.agent.md<br/>Specialized expertise"]
        IF["📝 Instruction Files<br/>.github/instructions/*.md<br/>Domain knowledge"]
        SK["⚖️ Spec Kit Commands<br/>/speckit.constitution<br/>/speckit.specify<br/>/speckit.tasks<br/>/speckit.implement<br/>/speckit.analyze"]
        CLI["⚙️ Copilot CLI<br/>gh copilot suggest<br/>Multi-step automation"]
        WS["💬 @workspace<br/>Coding Agent<br/>Conversational generation"]
    end
    
    subgraph Theme["🎯 THEME MAPPING"]
        D["DESIGN<br/>Instructions + Constitution"]
        S["SPEC<br/>/speckit.specify"]
        P["PLAN<br/>/speckit.tasks"]
        C["CODE<br/>/speckit.implement<br/>+ @workspace"]
        B["BEYOND<br/>Agent Skills + MCP<br/>+ Custom Agents<br/>+ Validation"]
    end
    
    subgraph Timing["⏱️ TIMING BREAKDOWN"]
        T1["Exp 1: 20 min"]
        T2["Exp 2: 25 min"]
        T3["Exp 3: 25 min"]
        T4["Exp 4: 30 min"]
        T5["Exp 5: 20 min"]
        Total["Total: 2 hours<br/>vs 3-4 weeks traditional"]
    end
    
    classDef agentSkillStyle fill:#4dabf7,stroke:#1971c2,stroke-width:2px,color:#fff
    classDef customAgentStyle fill:#69db7c,stroke:#2b8a3e,stroke-width:2px,color:#fff
    classDef speckitStyle fill:#ffd43b,stroke:#f08c00,stroke-width:2px,color:#000
    classDef cliStyle fill:#da77f2,stroke:#9c36b5,stroke-width:2px,color:#fff
    classDef codingAgentStyle fill:#ff922b,stroke:#e8590c,stroke-width:2px,color:#fff
    classDef themeStyle fill:#20c997,stroke:#087f5b,stroke-width:2px,color:#fff
    classDef timingStyle fill:#868e96,stroke:#495057,stroke-width:2px,color:#fff
    
    class AS,MCP agentSkillStyle
    class CA customAgentStyle
    class IF,SK speckitStyle
    class CLI cliStyle
    class WS codingAgentStyle
    class D,S,P,C,B themeStyle
    class T1,T2,T3,T4,T5,Total timingStyle
```

---

## Color Key

| Color | Agent/Tool Type |
|-------|----------------|
| 🔵 **Blue** | Agent Skills + GitHub MCP |
| 🟢 **Green** | Custom Agents (.agent.md) |
| 🟡 **Yellow** | Spec Kit Commands + Instruction Files |
| 🟣 **Purple** | Copilot CLI (gh copilot suggest) |
| 🟠 **Orange** | @workspace Coding Agent |
| 🔴 **Red** | Crisis Start/End |
| ✅ **Light Green** | Outcomes/Results |
| 🎯 **Teal** | Theme Mapping (DESIGN/SPEC/PLAN/CODE/BEYOND) |
| ⚪ **Gray** | Timing Information |

---

## How to Export as PNG

### Option 1: View in GitHub (Recommended)
1. Push this file to GitHub
2. GitHub automatically renders Mermaid diagrams
3. Right-click on the diagram → "Save image as..." → PNG

### Option 2: Use Mermaid Live Editor
1. Visit: https://mermaid.live/
2. Copy the mermaid code above
3. Paste into the editor
4. Click "Download" → Select PNG format

### Option 3: Use VS Code Extension
1. Install "Markdown Preview Mermaid Support" extension
2. Open this file in VS Code
3. Press `Ctrl+Shift+V` to preview
4. Right-click diagram → Export as PNG

### Option 4: Use CLI Tool
```bash
npm install -g @mermaid-js/mermaid-cli
mmdc -i WORKSHOP-FLOW-DIAGRAM.md -o workshop-flow.png
```

---

## Summary of Agents Used

| Experiment | Agents/Tools | Purpose |
|------------|-------------|---------|
| **1** | Agent Skills + GitHub MCP | Autonomous bug analysis + Issue creation |
| **2** | Custom Agent (@search-architect) | Architecture expert analysis |
| **3** | Instruction Files + Spec Kit | Domain knowledge + Governance + Spec |
| **4** | Copilot CLI + /speckit.implement + @workspace | Setup + Spec-driven + Conversational coding |
| **5** | /speckit.analyze + checklist | Constitution validation + Quality gates |

**Total: 7 distinct agent types working together**
