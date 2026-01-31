---
name: recipe
description: Generate personalized recipes respecting your dietary constraints and preferences. Use when you want meal ideas or recipe suggestions.
---

# Recipe Generator

Generate delicious, personalized recipes that respect your dietary requirements.

## Workflow

### Step 1: Load Context (Silent)

**First, verify the data files exist** by running Glob with pattern `data/*.md`.

Then read ALL found data files using the Read tool. Do not summarize or output their contents to the user.

Expected files:
- `data/dietary-requirements.md` - Medical constraints (NEVER violate)
- `data/preferences.md` - Personal preferences
- `data/macros.md` - Calorie and macro targets
- `data/flavor-profiles.md` - Available flavor options
- `data/recipe-format.md` - Output template
- `data/recipe-best-practices.md` - Culinary principles
- `data/prep-timings.md` - Adjusted time estimates for user's cooking profile

**Important:** Do not assume files are missing without actually checking. If Glob returns files, read them. If any expected files are genuinely missing, inform the user which ones and offer to continue with the embedded Critical Constraints as a fallback.

Internalize these constraints. Do not summarize them to the user.

### Step 2: Gather Requirements

Use AskUserQuestion to ask the following questions. Ask all questions in a single AskUserQuestion call with multiple questions:

**Question 1: Meal Type**
- Header: "Meal type"
- Options: Lunch, Dinner

**Question 2: Servings**
- Header: "Servings"
- Options:
  - "1 serving" (description: "~500 calories, 35g protein")
  - "2 servings" (description: "~1000 calories total, 70g protein")

**Question 3: Time Available**
- Header: "Time"
- Options:
  - "Quick (under 35 min)" (description: "Minimal prep, simple cooking")
  - "Medium (35-50 min)" (description: "Moderate prep, standard cooking")
  - "More time (50+ min)" (description: "More involved prep or slower methods")

**Question 4: Ingredients Approach**
- Header: "Ingredients"
- Options:
  - "Use what I have" (description: "I'll tell you what's in my fridge")
  - "Suggest anything" (description: "I'll shop for what's needed")

### Step 3: Follow-up Questions (If Needed)

If user selected "Use what I have", ask a follow-up:

**Question: Available Ingredients**
- Header: "On hand"
- Options:
  - "Chicken"
  - "Salmon/Fish"
  - "Beef"
  - "Eggs"
- Enable multiSelect: true
- Note: User can select "Other" to type specific ingredients

Then ask about vegetables on hand (multiSelect):
- Header: "Vegetables"
- Options: "Broccoli", "Cauliflower", "Zucchini", "Asparagus"
- Enable multiSelect: true

### Step 4: Flavor Profile Selection

Present flavor profiles from `data/flavor-profiles.md`:

**Question: Flavor Profile**
- Header: "Flavor"
- Options (show 4, user can select Other for more):
  - "Mediterranean" (description: "Olive oil, herbs, garlic, bright")
  - "Asian-Inspired" (description: "Ginger, sesame, coconut aminos")
  - "Spicy & Bold" (description: "Heat with complexity, chilies, ginger")
  - "Rich & Savory" (description: "Deep umami, mushrooms, slow-cooked")

### Step 5: Additional Guidance (Optional)

Ask the user if they have any specific ideas, cravings, or requirements:

**Question: Special Requests**
- Header: "Ideas"
- Options:
  - "No specific requests" (description: "Surprise me with your best ideas")
  - "Crispy/crunchy textures" (description: "Browned edges, crispy skin, toasted elements")
  - "Comfort food vibes" (description: "Warm, cozy, satisfying")
  - "Light and fresh" (description: "Bright, not too heavy")
- Note: User can select "Other" to type specific guidance like "browned and bubbly cheese" or "something I can eat with my hands"

If the user provides custom guidance, incorporate it into the brainstorm criteria.

### Step 6: Brainstorm Recipes

Generate exactly 5 recipe ideas that:
- ✅ Meet ALL dietary requirements (keto, APOE4, MCAS) — NO EXCEPTIONS
- ✅ Respect personal preferences where possible
- ✅ Match the selected flavor profile
- ✅ Fit within the time constraint
- ✅ Use available ingredients (if specified)
- ✅ Hit macro targets for selected serving size

Present each idea with:
```
### [Number]. [Recipe Name]
[One-sentence description]

- **Time:** [realistic total from prep-timings.md]
- **Key ingredients:** [3-4 main items]
- **Difficulty:** [Easy/Medium/Advanced]
- **Focus level:** [Low/Medium/High] — how much active attention required
- **Why it works:** [Brief note on flavor/texture appeal]
```

**Focus level guide:**
- **Low:** Mostly hands-off (roasting, simmering). Timer-friendly.
- **Medium:** Some active steps but forgiving timing. Good flow.
- **High:** Requires sustained attention or precise timing. Multiple simultaneous tasks.

### Step 7: User Selection

Ask the user to pick a recipe:

**Question: Recipe Choice**
- Header: "Recipe"
- Options: List the 5 recipe names
- Include "Other" option for modifications or re-brainstorming

### Step 8: Generate Full Recipe

Create the complete recipe following `data/recipe-format.md` template:

1. Apply culinary best practices from `data/recipe-best-practices.md`
2. **Calculate realistic times using `data/prep-timings.md`:**
   - Use the adjusted prep task times (not standard chef times)
   - Include transition overhead (gathering ingredients, prep-to-cook switch)
   - Add buffer time (5 min)
   - Display the adjusted total time in the recipe header
3. Calculate accurate nutrition for the serving size
4. Include practical substitutions
5. Add helpful cooking notes and tips
6. Verify dietary compliance (keto, APOE4, MCAS)
7. **Note ADHD-friendliness** in Cooking Notes if relevant:
   - Highlight "set and forget" steps (roasting, simmering)
   - Flag steps requiring close attention
   - Suggest batching prep tasks together
   - Recommend timer usage for specific steps

### Step 9: Save Recipe

Save the recipe to a subfolder based on servings:
- **1 serving:** `recipes/1-serving/`
- **2 servings:** `recipes/2-serving/`

Filename format: `YYYY-MM-DD-recipe-name.md`
- Use lowercase, hyphens for spaces
- Example: `recipes/1-serving/2024-01-15-mediterranean-salmon.md`

Create the subfolder if it doesn't exist.

### Step 10: Export to Paprika

Generate a `.paprikarecipe` file for import into Paprika app.

**File location:** Same subfolder as the markdown file
**Filename:** Same as markdown but with `.paprikarecipe` extension

**Paprika format** is a gzipped JSON file. Use Bash to create it:

```bash
# Create the JSON content, then gzip it
echo '<json_content>' | gzip > "recipes/[subfolder]/YYYY-MM-DD-recipe-name.paprikarecipe"
```

**Required JSON structure:**
```json
{
  "name": "[Recipe Name]",
  "servings": "[1 or 2 servings]",
  "prep_time": "[X minutes]",
  "cook_time": "[X minutes]",
  "total_time": "[X minutes]",
  "difficulty": "[Easy/Medium/Advanced]",
  "ingredients": "[All ingredients, one per line, no checkboxes or category headers]",
  "directions": "[All steps as numbered list, one per line, include step titles]",
  "notes": "[Cooking notes, substitutions summary, and storage info]",
  "nutritional_info": "Calories: [X] | Protein: [X]g | Fat: [X]g | Net Carbs: [X]g | Fiber: [X]g",
  "categories": ["Keto", "[Lunch or Dinner]", "[Flavor Profile]"],
  "description": "[The description paragraph from the recipe]",
  "source": "Keto Recipe Generator",
  "uid": "[Generate a UUID]"
}
```

**Ingredient formatting for Paprika:**
- Strip `- [ ]` checkbox syntax
- Remove section headers (Main Components, Seasonings, etc.)
- Keep amounts and notes (e.g., "6 oz beef sirloin — sliced thin")
- One ingredient per line, separated by `\n`

**Directions formatting for Paprika:**
- Number each step
- **Keep markdown bold syntax** for step titles: `1. **Step title:** Instructions...`
- One step per line, separated by `\n\n` (double newline for readability)

Inform the user both files were saved and that they can import the `.paprikarecipe` file directly into Paprika.

## Critical Constraints

**NEVER include these ingredients (medical requirements):**
- Processed meats (bacon, sausage, deli meat)
- Aged cheeses (parmesan, cheddar, blue cheese)
- Fermented foods (soy sauce, miso, sauerkraut)
- Canned fish (use fresh only)
- Tomatoes or tomato products
- Spinach, eggplant
- Shellfish
- Vinegar (except small amounts of apple cider vinegar)
- High-saturated fat oils (coconut oil, excessive butter)

**ALWAYS verify:**
- Net carbs under 20g per meal
- Prioritize omega-3s and unsaturated fats
- All proteins are fresh, not processed
- No aged or fermented ingredients

## Example Interaction Flow

```
User: /recipe

[Claude silently reads all data files]

Claude: I'll help you create a keto recipe! Let me ask a few questions.

[AskUserQuestion with meal type, servings, time, ingredients approach]

User: Dinner, 2 servings, Medium time, Use what I have

[AskUserQuestion about proteins and vegetables on hand]

User: Chicken, Broccoli and Zucchini

[AskUserQuestion about flavor profile]

User: Mediterranean

[AskUserQuestion about additional guidance]

User: No specific requests (or: "something with browned and bubbly cheese")

Claude: Here are 5 Mediterranean-inspired chicken recipes...

[Shows 5 options]

User: #2

Claude: [Generates full recipe and saves to file]

Recipe saved to: recipes/2-serving/2024-01-15-herb-crusted-chicken-thighs.md
Paprika export: recipes/2-serving/2024-01-15-herb-crusted-chicken-thighs.paprikarecipe
```

## Notes

- Always be enthusiastic about food while maintaining accuracy
- If a user's request conflicts with medical constraints, explain why and offer alternatives
- If user wants something outside their constraints, gently redirect
- Recipes should be practical and achievable for home cooks
- Focus on flavor — dietary restrictions shouldn't mean bland food
- **Time estimates are realistic, not aspirational.** They account for the user's prep-timings profile (currently: ADHD + moderate experience). This means times may look longer than standard recipes—that's intentional and sets the user up for success.
- When possible, favor recipes with lower focus requirements (more roasting/simmering, less simultaneous pan management)
