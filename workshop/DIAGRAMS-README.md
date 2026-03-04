# Workshop Flow Diagrams - README

## 📊 Generated Diagrams

### 1. **workshop-flow.png** (180 KB)
The complete 2-hour workshop flow showing all 5 experiments with color-coded agent types.

**Shows:**
- ⏱️ Timeline: 3:00 PM → 5:00 PM (2 hours)
- 🔵 **Blue**: Agent Skills + GitHub MCP (Experiment 1)
- 🟢 **Green**: Custom Agents (Experiment 2)
- 🟡 **Yellow**: Spec Kit + Instruction Files (Experiments 3, 4, 5)
- 🟣 **Purple**: Copilot CLI (Experiment 4)
- 🟠 **Orange**: @workspace Coding Agent (Experiment 4)
- ✅ **Light Green**: Outcomes at each stage
- 🔴 **Red**: Crisis start and resolution

**Key Points:**
- Each experiment box shows time allocation
- Clear labeling of which agent/tool is used
- Visual flow from problem → analysis → design → implementation → validation
- Theme markers show DESIGN + SPEC (Exp 3) and PLAN + CODE (Exp 4)

---

### 2. **workshop-legend.png** (94 KB)
Comprehensive legend explaining all agents, tools, theme mapping, and timing.

**Contains:**
- 🎨 **Agent & Tool Legend**: All 7 agent types with file locations
- 🎯 **Theme Mapping**: How workshop maps to "Spec, Plan, Design & Code + BEYOND"
- ⏱️ **Timing Breakdown**: Individual experiment durations + total speedup (168x)

---

## 🎯 Theme Alignment

| Theme Element | Workshop Location | Color |
|--------------|-------------------|-------|
| **DESIGN** | Experiment 3: Instruction Files + Constitution | 🟡 Yellow |
| **SPEC** | Experiment 3: /speckit.specify | 🟡 Yellow |
| **PLAN** | Experiment 4: /speckit.tasks | 🟡 Yellow |
| **CODE** | Experiment 4: /speckit.implement + @workspace | 🟡🟠 Yellow/Orange |
| **BEYOND** | Experiments 1, 2, 5: Skills, Custom, MCP, Validation | 🔵🟢 Blue/Green |

---

## 📋 Complete Agent Ecosystem (7 Types)

1. **📊 Agent Skills** - `.github/skills/*.md` - Autonomous analysis
2. **🔗 GitHub MCP** - Programmatic GitHub operations
3. **🤖 Custom Agents** - `.github/agents/*.agent.md` - Specialized expertise
4. **📝 Instruction Files** - `.github/instructions/*.md` - Domain knowledge
5. **⚖️ Spec Kit** - `/speckit.*` commands - Governance & generation
6. **⚙️ Copilot CLI** - `gh copilot suggest` - Multi-step automation
7. **💬 @workspace** - Coding agent - Conversational generation

---

## 🚀 Usage in Presentations

### For Slides
- **workshop-flow.png**: Use as main flow diagram in presentation
- **workshop-legend.png**: Use as reference slide or appendix

### For Documentation
- Both diagrams are referenced in WORKSHOP-FLOW-DIAGRAM.md
- Include in README or workshop introduction

### For Training
- Start with legend to explain agent types
- Walk through flow diagram showing crisis → resolution
- Emphasize 2-hour timeline vs 3-4 week traditional approach

---

## 📁 File Structure

```
workshop/
├── workshop-flow.png          # Main flow diagram (2400x1800)
├── workshop-legend.png        # Legend & theme (1800x1200)
├── workshop-flow.mmd          # Source Mermaid for flow
├── workshop-legend.mmd        # Source Mermaid for legend
└── WORKSHOP-FLOW-DIAGRAM.md   # Documentation + Mermaid code
```

---

## 🔄 Regenerating Diagrams

If you need to modify and regenerate:

```bash
cd workshop

# Edit the .mmd files as needed
# Then regenerate:

npx -p @mermaid-js/mermaid-cli mmdc -i workshop-flow.mmd -o workshop-flow.png -w 2400 -H 1800 -b transparent

npx -p @mermaid-js/mermaid-cli mmdc -i workshop-legend.mmd -o workshop-legend.png -w 1800 -H 1200 -b transparent
```

---

## ✅ Quality Features

- **High Resolution**: 2400x1800 (flow) and 1800x1200 (legend)
- **Transparent Background**: Works on any slide background
- **Color-Coded**: Each agent type has distinct color
- **Professional**: Clear labels, proper spacing, readable fonts
- **Print-Ready**: High DPI suitable for printing

---

## 📌 Key Messaging Points

Use these diagrams to communicate:

✅ **Speed**: 2 hours vs 3-4 weeks (168x faster)  
✅ **Comprehensive**: Not just one tool, but 7 agent types working together  
✅ **Realistic**: Brownfield crisis scenario, not greenfield tutorial  
✅ **Theme-Aligned**: Clear mapping to Spec, Plan, Design, Code + Beyond  
✅ **Complete Workflow**: Problem → Analysis → Design → Implementation → Validation  
✅ **Dual Coding**: Shows both spec-driven AND conversational coding approaches  

---

*Diagrams generated: March 2, 2026 using Mermaid CLI v11.12.0*
