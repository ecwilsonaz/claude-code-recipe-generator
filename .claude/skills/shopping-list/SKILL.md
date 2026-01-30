# Shopping List Generator

Generate a consolidated shopping list from multiple recipes, organized by grocery aisle.

## Usage

/shopping-list [recipe files...]

Examples:
- /shopping-list recipes/1-serving/beef-dish.md
- /shopping-list recipes/1-serving/*.md recipes/2-serving/chicken.md

## Process

1. **Parse recipes**: Extract the "Shopping List" section from each referenced recipe file

2. **Identify compound ingredients**: Flag items that are made from other ingredients:
   - Known compounds: garlic-infused olive oil, cauliflower rice, bone broth
   - Pattern matches: anything with "infused", "homemade"

3. **Interactive prompt for compounds**: For each compound ingredient found, ask:
   - "Buy components" → expand into base ingredients
   - "Already have it" → omit from list
   - "Buy pre-made" → add as-is to appropriate aisle

4. **Consolidate**: Combine duplicate ingredients across recipes
   - Sum quantities where units match (3 eggs + 4 eggs = 7 eggs)
   - List separately when units differ or quantities can't be combined
   - Identify pantry staples as MIGHT HAVE candidates

5. **Categorize by aisle**: Reference `data/grocery-aisles.md` for aisle assignments
   - Match items to aisles by keyword/category
   - Unknown items go to a "Other" section at the end

6. **Generate output**: Create shopping list file and display in terminal

## Output Format

File: `lists/shopping-list-YYYY-MM-DD.md`

```
# Shopping List
[N] recipes · [Date]

## [Aisle Name]
- [Item] ([quantity])
- [Item] ([quantity]) - MIGHT HAVE

## [Next Aisle]
...
```

## MIGHT HAVE Logic

Mark items as "MIGHT HAVE" if they are:
- Common pantry staples: salt, pepper, olive oil, basic spices
- Items with no quantity specified (implying "to taste" or "as needed")
- Spices and dried herbs (bulk section items used in small amounts)

## Aisle Reference

Use `data/grocery-aisles.md` for aisle names and categorization guidance.

## Dependencies

- Requires recipe files in the standard format with "Shopping List" section
- Requires `data/grocery-aisles.md` for aisle configuration
