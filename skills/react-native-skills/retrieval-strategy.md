# Rule Retrieval Strategy

## 🎯 Goal
Select the most relevant rules based on user input to generate high-quality, production-ready responses.

---

## 🧠 Step 1: Understand User Intent

Classify user query into one or more categories:

- performance
- ui
- animation
- state
- navigation
- architecture

---

## 🔍 Step 2: Match Triggers

For each rule in `rule-index.json`:

- Compare user query with `triggers`
- Count number of matches

---

## ⚖️ Step 3: Scoring System

Assign score to each rule:

- Trigger match → +3 per match
- Category match → +2
- Priority:
  - high → +3
  - medium → +2
  - low → +1

---

## 🏆 Step 4: Select Top Rules

- Sort rules by score
- Pick top 3–5 rules
- Always include at least:
  - 1 high priority rule
  - 1 rule directly matching query intent

---

## 🚫 Step 5: Filter Noise

Exclude rules if:
- No trigger match
- Category mismatch AND low priority

---

## 🧩 Step 6: Combine Rules

While generating response:

- Merge insights from selected rules
- Avoid repeating same idea
- Prioritize:
  1. Performance fixes
  2. Correct patterns
  3. Clean code practices

---

## ⚡ Special Handling

### Performance Queries
Always prioritize:
- list-performance-*
- animation-*

### UI Queries
Focus on:
- ui-*
- design-system-*

### State Issues
Focus on:
- react-state-*
- state-*

---

## 🧠 Output Guidelines

- Be practical, not theoretical
- Prefer code examples
- Suggest fixes, not just problems
- Combine multiple rules into one clean solution