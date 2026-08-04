---
name: recipe-box
description: Use when the user wants to save a recipe (from a TikTok/video link, screenshot, photo, recipe website URL, or pasted text) into their recipe collection, or ask questions about their saved recipes: measurements, scaling servings, ingredient substitutions, nutrition and macros, searching by ingredient, or meal planning and grocery lists.
---

# Recipe Box

A personal recipe collection. Save recipes from anywhere into one folder as clean, standardized files, then answer cooking questions against them: measurements, scaling, substitutions, nutrition and macros, ingredient search, and meal planning.

Writing style: never use em dashes anywhere in recipe files, summaries, or replies. Use commas, colons, or separate sentences instead.

## Where recipes live

All recipes are stored as Markdown files in a folder named `recipes/` inside the user's connected folder.

- If no folder is connected, ask the user to connect one, then create `recipes/` inside it.
- Each recipe is one file: `recipes/<slug>.md` (e.g. `recipes/garlic-butter-shrimp-pasta.md`).
- Keep an index file at `recipes/_index.md`: a simple table of every recipe (title, tags, servings, calories per serving, source) that you update whenever you add or edit a recipe. This makes searching and meal planning fast.

## Adding a recipe

You accept four kinds of input. In all cases, produce ONE standardized recipe file.

1. **Recipe website URL**: fetch the page. If it returns an empty or JavaScript-only shell, render it with browser tools. Extract the recipe, ignoring blog filler, ads, and life stories.
2. **TikTok / video link**: you cannot watch video. Fetch the page and pull the recipe from the caption/description and any on-screen text in the metadata. If the link yields no usable recipe text, tell the user and ask them to paste the caption or the steps.
3. **Screenshot / photo**: read the image directly and transcribe it into the standard format.
4. **Pasted or typed text**: parse and standardize it.

Before saving, normalize:
- Convert vague amounts to explicit measurements where the source gives them. Never invent quantities. If an amount is missing, write `to taste` or `[amount not given]`.
- Record servings/yield. If absent, estimate and mark it `(est.)`.
- Add 3 to 6 tags (cuisine, protein, meal type, dietary, effort) to power searching and planning.
- Always keep the original source link/handle for attribution.

### Nutrition: always ask

Every recipe must have a Nutrition section with per-serving calories, protein, carbs, and fat. Handle it in this order:

1. If the source (caption, video text, website, or image) already lists any nutrition numbers, capture them and mark them `(from source)`.
2. For anything the source does not give, compute a per-serving estimate from the ingredient list and servings, and mark it `(est.)`. You cannot read nutrition off a video image, so estimate from ingredients when the caption has no numbers.
3. After drafting the recipe, ALWAYS ask the user a short prompt to confirm or improve the nutrition and gather anything else worth storing, for example: "I estimated about 520 cal, 34g protein, 45g carbs, 18g fat per serving. Do you have exact numbers from the video, and any allergens or dietary notes to add?" Update the file with whatever they provide.

Never present an estimate as if it were exact. Keep the `(est.)` and `(from source)` labels.

### Standard recipe file format

```markdown
# <Recipe Title>

- **Servings:** <n>
- **Time:** <prep + cook>
- **Tags:** <tag1>, <tag2>, <tag3>
- **Source:** <url or @handle or "pasted">

## Nutrition (per serving)
- **Calories:** <n> (est. | from source)
- **Protein:** <n> g
- **Carbs:** <n> g
- **Fat:** <n> g
- **Notes:** <allergens, dietary flags, or blank>

## Ingredients
- <amount> <ingredient>
- <amount> <ingredient>

## Steps
1. <step>
2. <step>

## Notes
<substitutions the source mentioned, tips, or leave blank>
```

After writing the file, update `recipes/_index.md` and confirm to the user with the title and a one-line summary.

## Answering questions

Read the relevant recipe file(s) from `recipes/` (use `_index.md` to locate them) before answering. Never guess amounts. Quote what the file says.

- **Measurements**: quote exact amounts from the file. If asked for a single ingredient, give just that line.
- **Scaling**: multiply every ingredient by the ratio (new servings / original servings). Show the scaled ingredient list. Scale the nutrition totals too if asked. Round to sensible kitchen amounts (e.g. 1.33 cups becomes 1 1/3 cups) and flag anything awkward (like eggs) rather than showing a fraction of one.
- **Substitutions**: first check the recipe's Notes. Then suggest standard swaps with the correct ratio (e.g. 1 cup buttermilk = 1 tbsp lemon juice plus milk to 1 cup). Note when a swap changes texture, taste, or macros.
- **Nutrition and macros**: answer from the Nutrition section. If the user is hitting a protein or calorie target, you can total macros across a planned set of recipes.
- **Search by ingredient**: scan `_index.md` and the ingredient lists; return matching recipe titles with their source. Handle "what can I make with X and Y" by ranking recipes that use the most of the named ingredients.
- **Meal planning**: pick recipes from the collection for the requested days, balancing variety and tags. Total the calories and macros for the plan if the user cares about targets. Then compile a consolidated grocery list, combining duplicate ingredients across recipes and grouping by store section (produce, protein, dairy, pantry). Offer to save the plan as `recipes/_mealplan-<date>.md`.

## Sharing with others

This skill is self-contained. Anyone can install it, connect their own folder, and start adding recipes. Their collection lives in their own `recipes/` folder. Nothing is hard-coded to one user.