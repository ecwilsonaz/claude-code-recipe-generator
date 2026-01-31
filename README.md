# Claude Code Recipe Generator

A Claude Code skill that generates personalized recipes with realistic time estimates, dietary restriction compliance, and Paprika app export.

## Demo

https://github.com/user-attachments/assets/42fefe12-2be2-45f0-a452-ba4141f5d422

## What This Is

This is a **Claude Code custom skill** — a structured prompt system that turns Claude into a specialized recipe generator. When you type `/recipe`, Claude walks you through creating a meal that fits your exact dietary needs, cooking style, and available time.

**Key features:**
- Fully customizable dietary requirements (keto, paleo, vegan, allergy-friendly, medical diets, etc.)
- Realistic time estimates adjusted for ADHD and different experience levels
- Exports directly to [Paprika Recipe Manager](https://www.paprikaapp.com/)
- Organizes recipes by serving size
- Includes shopping lists, substitutions, and storage instructions

## Requirements

- [Claude Code](https://claude.ai/code) (Anthropic's CLI tool)
- A Claude Pro or API subscription

## Quick Start

1. **Clone this repository** into your projects folder:
   ```bash
   git clone https://github.com/ecwilsonaz/claude-code-recipe-generator.git
   cd claude-code-recipe-generator
   ```

2. **Customize the data files** for your dietary needs (see [Customization](#customizing-for-your-needs) below)

3. **Open Claude Code** in this directory:
   ```bash
   claude
   ```

4. **Generate a recipe:**
   ```
   /recipe
   ```

5. Claude will ask you about meal type, servings, time available, ingredients, and flavor profile, then generate a complete recipe.

## Project Structure

```
claude-code-recipe-generator/
├── .claude/
│   └── skills/
│       ├── recipe/
│       │   └── SKILL.md          # Recipe generation skill
│       └── shopping-list/
│           └── SKILL.md          # Shopping list aggregation skill
├── data/
│   ├── dietary-requirements.md   # Medical/dietary constraints (customize this!)
│   ├── preferences.md            # Personal preferences
│   ├── macros.md                 # Calorie and macro targets
│   ├── flavor-profiles.md        # Cuisine and flavor options
│   ├── prep-timings.md           # Realistic time estimates
│   ├── recipe-format.md          # Output template
│   ├── recipe-best-practices.md  # Culinary principles
│   └── grocery-aisles.md         # Store aisle configuration
├── recipes/
│   ├── 1-serving/                # Single-serving recipes
│   │   ├── *.md                  # Markdown recipes
│   │   └── *.paprikarecipe       # Paprika exports
│   └── 2-serving/                # Two-serving recipes
│       ├── *.md
│       └── *.paprikarecipe
├── lists/                        # Generated shopping lists
│   └── *.md
└── README.md
```

## Customizing for Your Needs

**This is a template.** The included data files are configured for a specific diet (keto + APOE4 + MCAS + low-FODMAP), but the system is designed to be fully customizable for any dietary approach.

### 1. Dietary Requirements (`data/dietary-requirements.md`)

This is the most important file to customize. It defines:
- **Allowed ingredients** by category (proteins, fats, vegetables, etc.)
- **Forbidden ingredients** (allergens, medical restrictions, preferences)
- **Special rules** (e.g., "no leftovers" for histamine issues, "fresh only" for certain proteins)

**Examples of diets you could configure:**
- **Standard keto** — allow aged cheeses, bacon, remove histamine restrictions
- **Paleo** — no dairy, no grains, no legumes
- **Vegan** — plant proteins only, no animal products
- **Allergy-specific** — exclude nuts, shellfish, eggs, etc.
- **Medical diets** — low-sodium, renal diet, diabetes-friendly
- **Whole30** — no sugar, alcohol, grains, legumes, soy, dairy

### 2. Macro Targets (`data/macros.md`)

Adjust the per-serving targets to match your nutritional goals:

```markdown
| Macro | Target | Range |
|-------|--------|-------|
| Calories | 900 | 850-1,000 |
| Protein | 55g | 50-65g |
| Fat | 65g | 60-75g |
| Net Carbs | 20g | 15-25g |
| Fiber | 15g | 12-18g |
```

Modify these for your approach — high-protein, low-fat, balanced macros, or just calorie targets.

### 3. Personal Preferences (`data/preferences.md`)

Set your:
- Favorite and disliked proteins/vegetables
- Spice and heat tolerance
- Cooking style (one-pan preferred, hands-on time limits)
- Available equipment (Instant Pot, air fryer, sheet pans, etc.)
- Seasonal and regional preferences

### 4. Prep Timings (`data/prep-timings.md`)

This file adjusts time estimates based on cooking experience and cognitive style. The default profile is **ADHD + Moderate Experience**, which includes:
- Realistic prep times (not chef-speed estimates)
- Task-switching overhead
- Setup and transition time
- Distinction between active and passive time

**To customize:** Adjust the time multipliers, or create different profiles:
- "Beginner cook" — more time for all tasks
- "Experienced cook" — faster times, less buffer
- "Low energy day" — extra buffer, simpler techniques preferred

### 5. Flavor Profiles (`data/flavor-profiles.md`)

Defines cuisine options (Mediterranean, Asian-Inspired, Mexican, etc.) with:
- Key ingredients for each cuisine
- Substitutions for restricted ingredients
- Recommended techniques
- DIY spice blend recipes (useful when store-bought blends contain forbidden ingredients)

### 6. Recipe Format (`data/recipe-format.md`)

The template structure for generated recipes. Modify if you want different sections, more/fewer details, or a different organization.

### 7. Recipe Best Practices (`data/recipe-best-practices.md`)

Culinary principles Claude follows — the "Salt Fat Acid Heat" framework, browning techniques, protein cooking guides, etc. Add your own tips or modify existing ones.

## Features

### Paprika Export

Every recipe is saved in two formats:
- **Markdown** (`.md`) — human-readable, version-controllable
- **Paprika** (`.paprikarecipe`) — importable directly into Paprika app

To import into Paprika:
1. Open Paprika
2. File → Import
3. Select the `.paprikarecipe` file

### ADHD-Friendly Design

The skill is designed with executive function challenges in mind:
- **Focus Level** rating (Low/Medium/High) for each recipe
- **Active vs. Passive time** breakdown
- Timer reminders in instructions
- Recipes favor "set and forget" techniques (roasting, pressure cooking)
- Clear sequencing to minimize task-switching

### Realistic Time Estimates

Unlike most recipes that assume professional chef speed, times here account for:
- Reading and understanding the recipe
- Gathering ingredients and equipment
- Actual prep speeds for home cooks
- Transition time between tasks
- A buffer for real-world cooking

### Flexible Serving Sizes

Recipes are organized by serving count:
- `recipes/1-serving/` — solo meals
- `recipes/2-serving/` — cooking for two

You can modify the skill to add more options (meal prep batches, family-sized, etc.).

### Shopping List Aggregation

Use `/shopping-list` to combine ingredients from multiple recipes into a single, organized shopping list:

```
/shopping-list recipes/1-serving/beef-dish.md recipes/2-serving/chicken.md
```

Features:
- **Organized by store aisle** — customize `data/grocery-aisles.md` for your store layout
- **Consolidates duplicates** — "3 eggs" + "4 eggs" = "7 eggs"
- **Handles compound ingredients** — asks whether to expand "garlic-infused olive oil" into garlic + olive oil
- **Flags pantry staples** — items marked "MIGHT HAVE" for things you likely already own
- **Copy-paste ready** — output format works with Todoist, Things 3, Apple Reminders, or any to-do app

## How Claude Code Skills Work

Claude Code skills are defined in `.claude/skills/*/SKILL.md` files. When you type a slash command (like `/recipe`), Claude:

1. Reads the `SKILL.md` file for instructions
2. Follows the defined workflow (gathering requirements, loading context, generating output)
3. Uses the tools available (reading files, asking questions, writing output)

Skills are essentially structured prompts that give Claude specialized knowledge and a consistent workflow for specific tasks.

### Creating Your Own Skills

You can create additional skills in `.claude/skills/`. Each skill needs:
- A folder with a `SKILL.md` file
- Frontmatter with `name` and `description`
- A workflow that Claude should follow

Example structure:
```markdown
---
name: my-skill
description: What this skill does
---

# My Skill

## Workflow

### Step 1: Do something
...
```

## Example Workflow

```
You: /recipe

Claude: I'll help you create a recipe! Let me ask a few questions.

        [Asks about meal type, servings, time, ingredients]

You: Dinner, 2 servings, Quick, Suggest anything

Claude: What flavor direction sounds good?

        [Shows flavor profile options]

You: Mediterranean

Claude: Here are 5 recipe ideas...

        1. Lemon-Herb Salmon with Roasted Broccoli
        2. Greek Chicken Thighs with Zucchini
        ...

You: #1

Claude: [Generates complete recipe with instructions, shopping list, nutrition]

        Recipe saved to: recipes/2-serving/2024-01-15-lemon-herb-salmon.md
        Paprika export: recipes/2-serving/2024-01-15-lemon-herb-salmon.paprikarecipe
```

### Shopping List Example

```
You: /shopping-list recipes/1-serving/beef-dish.md recipes/2-serving/chicken.md

Claude: I found 1 compound ingredient:

        Garlic-infused olive oil (used in 2 recipes)
        ├─ Buy components (garlic + olive oil)
        ├─ Already have it
        └─ Buy pre-made

You: Already have it

Claude: # Shopping List
        2 recipes · January 15, 2024

        ## Produce
        - Fresh ginger (2 small knobs)
        - Zucchini, medium (2)
        - Broccoli (2 heads)
        ...

        Shopping list saved to: lists/shopping-list-2024-01-15.md
```

## Limitations

- **Requires Claude Code** — This is not a standalone app; it's a skill for Claude Code
- **Time estimates are approximations** — Actual times vary based on your kitchen and energy level
- **Nutritional info is calculated, not verified** — Values are estimates based on ingredient databases
- **You must customize the data files** — The defaults are specific to one person's medical diet

## Contributing

Feel free to fork this project and customize it for your own dietary needs. If you make improvements to the skill logic (not the personal dietary data), PRs are welcome.

Ideas for contributions:
- Additional prep-timing profiles (beginner, expert, low-spoon)
- More flavor profiles / cuisines
- Meal prep batch cooking support
- Integration with other recipe apps

## License

MIT License — use this however you'd like.

---

Built with [Claude Code](https://claude.ai/code) by Anthropic.
