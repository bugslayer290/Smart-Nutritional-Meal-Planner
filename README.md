1. app.py (Streamlit app)

This is an interactive web app for generating meal and grocery recommendations.

Main parts of the code:

Dataset selection: Lets the user choose which CSV files to load.

INDB.csv → user profiles (age, gender, weight, etc.)

diet_recommendations_dataset.csv → food & nutrition data

User input form: Name, age, gender, height, weight, disease, allergies, plan type.

BMI calculation: Computes BMI and suggests daily nutrients (calories, vitamins).

Preview datasets: Shows a few rows from the chosen CSVs.

Feature extraction:

Uses TF-IDF + NMF on recipe/ingredient text to find food topics (e.g., “chicken, garlic, rice” → “protein meals”).

Personalized meal plan:

Tries to match the input user to a row in INDB.csv for a recommended diet type.

Filters diet_recommendations_dataset.csv based on:

Allergies (removes foods containing allergens).

Disease (maps to diet type: Diabetes → Low Sugar, Hypertension → Low Sodium, etc.).

Selects top foods (low-carb, low-sodium, or balanced).

7-day meal plan:

Randomly assigns recommended foods to breakfast/lunch/dinner for each day.

👉 Datasets used here:

INDB.csv: user demographic + diet recommendation mapping.

diet_recommendations_dataset.csv: foods with nutrition values (food_name, protein_g, carb_g, etc.).

2. main.py (FastAPI backend)

This is an API/web backend version of the same idea.

Main parts of the code:

Loads nutrition database:

INDB.csv → food nutrient database (required).

Optionally loads profile-based suggestions:

Food_and_Nutrition__.csv (or other fallback CSVs).

This gives direct meal suggestions (columns like Dietary Preference, Breakfast Suggestion, Lunch Suggestion, Dinner Suggestion).

User input handling:

Receives data from an HTML form (index.html).

Calculates BMI and nutrient needs.

Meal plan logic:

If profile dataset exists: Matches user by age, gender, disease, and dietary preference. Then directly picks suggested meals.

If profile dataset missing: Falls back to INDB.csv and filters foods:

Remove allergen foods.

Apply vegetarian/vegan restrictions.

Apply disease-based filters (Low-Carb, Low-Sodium, etc.).

Output: Renders result.html with:

BMI info

Recommended diet type

Weekly 7-day meal plan

Grocery list (unique foods used in the plan)

👉 Datasets used here:

INDB.csv: always needed for food nutrition.

Food_and_Nutrition__.csv (or fallback CSVs): provides direct meal suggestions if available.

3. requirements.txt

Dependencies for both apps:

streamlit (for app.py) isn’t listed but is implied.

fastapi, uvicorn, jinja2 (for main.py).

pandas, numpy, scikit-learn (data analysis).

python-multipart (form handling in FastAPI).

✅ Summary of dataset roles:

INDB.csv → User profile data (for Streamlit) or Nutrition DB (for FastAPI).

diet_recommendations_dataset.csv → Food & nutrition values (for Streamlit).

Food_and_Nutrition__.csv → Profile-based meal suggestions (for FastAPI).
