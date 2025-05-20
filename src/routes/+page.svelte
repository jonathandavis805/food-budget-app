<h1>Welcome to SvelteKit</h1>
<script>

  let recipes = $state([
    {
      id: 1,
      name: 'Spaghetti Carbonara',
      description: 'A classic Italian pasta dish.',
      prepTime: '15 minutes',
      cookTime: '20 minutes',
      difficulty: 'Easy',
      servings: 4,
      image: 'https://via.placeholder.com/600x400.png?text=Spaghetti+Carbonara',
      ingredients: [
        { name: 'Spaghetti', quantity: '400g', notes: '' },
        { name: 'Guanciale or Pancetta', quantity: '150g', notes: 'cubed' },
        { name: 'Eggs', quantity: '3 large', notes: 'yolks' },
        { name: 'Pecorino Romano Cheese', quantity: '50g', notes: 'grated, plus extra for serving' },
        { name: 'Black Pepper', quantity: 'to taste', notes: 'freshly ground' },
        { name: 'Salt', quantity: 'to taste', notes: '' },
      ],
      instructions: [
        'Cook spaghetti according to package directions until al dente. Reserve 1 cup of pasta water.',
        'While pasta cooks, fry guanciale in a large skillet over medium heat until crisp. Remove from heat.',
        'In a bowl, whisk egg yolks, Pecorino Romano, and a generous amount of black pepper.',
        'Drain pasta and add it to the skillet with the guanciale. Toss to combine.',
        'Quickly pour the egg mixture over the pasta, stirring rapidly to prevent eggs from scrambling. If needed, add a little reserved pasta water to create a creamy sauce.',
        'Serve immediately with extra cheese and pepper.',
      ],
      nutritionalInfo: {
        calories: 'Approx. 600 kcal per serving',
        protein: '25g',
        fat: '35g',
        carbs: '45g',
      },
      costPerServing: 3.50
    },
    {
      id: 2,
      name: 'Chicken Stir-Fry',
      description: 'Quick and healthy chicken and vegetable stir-fry.',
      prepTime: '20 minutes',
      cookTime: '15 minutes',
      difficulty: 'Medium',
      servings: 3,
      image: 'https://via.placeholder.com/600x400.png?text=Chicken+Stir-Fry', // Placeholder image
      ingredients: [
        { name: 'Chicken Breast', quantity: '500g', notes: 'sliced' },
        { name: 'Broccoli Florets', quantity: '1 cup', notes: '' },
        { name: 'Carrots', quantity: '2', notes: 'julienned' },
        { name: 'Bell Pepper', quantity: '1', notes: 'sliced (any color)' },
        { name: 'Soy Sauce', quantity: '3 tbsp', notes: '' },
        { name: 'Ginger', quantity: '1 tbsp', notes: 'minced' },
        { name: 'Garlic', quantity: '2 cloves', notes: 'minced' },
        { name: 'Sesame Oil', quantity: '1 tsp', notes: '' },
        { name: 'Rice', quantity: 'to serve', notes: 'cooked' },
      ],
      instructions: [
        'Heat a wok or large skillet over high heat. Add a little oil.',
        'Add chicken and stir-fry until browned and cooked through. Remove and set aside.',
        'Add garlic and ginger to the wok, stir-fry for 30 seconds until fragrant.',
        'Add broccoli, carrots, and bell pepper. Stir-fry for 5-7 minutes until tender-crisp.',
        'Return chicken to the wok. Add soy sauce and sesame oil. Toss to combine and heat through.',
        'Serve immediately over cooked rice.',
      ],
      nutritionalInfo: {
        calories: 'Approx. 450 kcal per serving',
        protein: '40g',
        fat: '15g',
        carbs: '30g',
      },
      costPerServing: 4.20 // Example cost
    }
  ]);

  let newGroceryItemName = $state("");
  let newGroceryItemQuantity = $state(0);
  let selectedRecipe = $state(recipes[0]); // Default to showing the first recipe
  let currentView = $state('recipeDetail'); // 'recipeList', 'recipeDetail', 'budget', 'groceryList'

  // --- Budget Data (Placeholder) ---
  let budget = $state({
    totalAllocated: 500, // USD
    spentThisMonth: 250.75,
    remaining: 249.25,
    categories: [
      { name: 'Groceries', allocated: 300, spent: 180.50 },
      { name: 'Dining Out', allocated: 100, spent: 50.25 },
      { name: 'Snacks/Coffee', allocated: 50, spent: 20.00 },
    ],
    recentTransactions: [
      { date: '2025-05-15', description: 'Grocery Store A', amount: 45.20, category: 'Groceries' },
      { date: '2025-05-14', description: 'Local Cafe', amount: 5.75, category: 'Snacks/Coffee' },
      { date: '2025-05-12', description: 'Restaurant B', amount: 25.00, category: 'Dining Out' },
    ]
  });

  // --- Grocery List Data (Placeholder) ---
  let groceryList = $state([
    { item: 'Spaghetti', quantity: '400g', acquired: false, recipeId: 1 },
    { item: 'Guanciale', quantity: '150g', acquired: false, recipeId: 1 },
    { item: 'Eggs', quantity: '6 pack', acquired: false, recipeId: null }, // General grocery item
    { item: 'Milk', quantity: '1 gallon', acquired: false, recipeId: null },
    { item: 'Broccoli', quantity: '1 head', acquired: false, recipeId: 2 },
  ]);
  /**
   * Represents a book.
   * @param {number} recipeId - The id of the recipe
   */
  function selectRecipe(recipeId) {
    selectedRecipe = recipes.find(r => r.id === recipeId);
    currentView = 'recipeDetail';
  }

  function navigateTo(view) {
    currentView = view;
  }

  /**
  * @param {object} ingredient
  */
  function toggleAcquired(item) {
    item.acquired = !item.acquired;
    groceryList = groceryList
  }

  /**
  * @param {object} ingredient
  */
  function addIngredientToGroceryList(ingredient) {
    const existingItem = groceryList.find(item => item.item.toLowerCase() === ingredient.name.toLowerCase() && item.recipeId === selectedRecipe.id);
    if (!existingItem) {
      groceryList = [...groceryList, { item: ingredient.name, quantity: ingredient.quantity, acquired: false, recipeId: selectedRecipe.id }];
    } else {
      // Optionally handle quantity update or notify user it's already there
      alert(`${ingredient.name} is already on the grocery list.`);
    }
  }

  function addManualGroceryItem() {
    console.log("testing");
  }

  budget.remaining = budget.totalAllocated - budget.spentThisMonth; // Derived reactive state
</script>

<div class="app-container">

  <header class="app-header">
    <h1>Recipe & Food Budget Hub</h1>
    <nav>
      <button class="{currentView === 'recipeList' || currentView === 'recipeDetail' ? 'active':''}" onclick={() => navigateTo('recipeList')}>Recipes</button>
      <button class="{currentView === 'budget' ? 'active':''}" onclick={() => navigateTo('budget')}>Budget</button>
      <button class="{currentView === 'groceryList' ? 'active':''}" onclick={() => navigateTo('groceryList')}>Grocery List</button>
    </nav>
  </header>

  <main class="main-content">

    {#if currentView === 'recipeList'}
      <section class="recipe-list-view card">
        <h2>All Recipes</h2>
        <div class="recipe-grid">
          {#each recipes as recipe (recipe.id)}
            <div class="recipe-card-small" onclick={() => selectRecipe(recipe.id)} onkeypress={() => selectRecipe(recipe.id)} role="button" tabindex="0">
              <img src={recipe.image} alt={recipe.name} class="recipe-card-small-img"/>
              <h3>{recipe.name}</h3>
              <p>{recipe.description}</p>
              <p><strong>Difficulty:</strong> {recipe.difficulty}</p>
            </div>
          {/each}
        </div>
      </section>
    {/if}

    {#if currentView === 'recipeDetail' && selectedRecipe}
      <section class="recipe-detail-view card">
        <div class="recipe-header">
          <img src={selectedRecipe.image} alt={selectedRecipe.name} class="recipe-image-large"/>
          <div>
            <h2>{selectedRecipe.name}</h2>
            <p>{selectedRecipe.description}</p>
            <div class="recipe-meta">
              <span><strong>Prep:</strong> {selectedRecipe.prepTime}</span>
              <span><strong>Cook:</strong> {selectedRecipe.cookTime}</span>
              <span><strong>Difficulty:</strong> {selectedRecipe.difficulty}</span>
              <span><strong>Servings:</strong> {selectedRecipe.servings}</span>
              <span><strong>Est. Cost/Serving:</strong> ${selectedRecipe.costPerServing.toFixed(2)}</span>
            </div>
          </div>
        </div>

        <div class="recipe-content-grid">
            <div class="ingredients-panel">
                <h3>Ingredients</h3>
                <ul>
                {#each selectedRecipe.ingredients as ingredient}
                    <li>
                    {ingredient.quantity} {ingredient.name}
                    {#if ingredient.notes}({ingredient.notes}){/if}
                    <button class="add-to-grocery-btn" 
                    onclick={
                    () => addIngredientToGroceryList(ingredient)
                    } title="Add to grocery list">🛒</button>
                    </li>
                {/each}
                </ul>
            </div>

            <div class="instructions-panel">
                <h3>Instructions</h3>
                <ol>
                {#each selectedRecipe.instructions as step, i}
                    <li>{step}</li>
                {/each}
                </ol>
            </div>
        </div>


        {#if selectedRecipe.nutritionalInfo}
        <div class="nutritional-info">
            <h3>Nutritional Information (Approximate)</h3>
            <p><strong>Calories:</strong> {selectedRecipe.nutritionalInfo.calories}</p>
            <p><strong>Protein:</strong> {selectedRecipe.nutritionalInfo.protein}</p>
            <p><strong>Fat:</strong> {selectedRecipe.nutritionalInfo.fat}</p>
            <p><strong>Carbs:</strong> {selectedRecipe.nutritionalInfo.carbs}</p>
        </div>
        {/if}
      </section>
    {/if}

    {#if currentView === 'budget'}
      <section class="budget-view card">
        <h2>Food Budget Overview</h2>
        <div class="budget-summary">
          <p><strong>Total Allocated:</strong> ${budget.totalAllocated.toFixed(2)}</p>
          <p class="spent"><strong>Spent This Month:</strong> ${budget.spentThisMonth.toFixed(2)}</p>
          <p class="remaining"><strong>Remaining:</strong> ${budget.remaining.toFixed(2)}</p>
        </div>

        <div class="budget-progress">
          <label for="budget-progress-bar">Monthly Spending:</label>
          <progress id="budget-progress-bar" value={budget.spentThisMonth} max={budget.totalAllocated}></progress>
          <span>{((budget.spentThisMonth / budget.totalAllocated) * 100).toFixed(1)}%</span>
        </div>

        <h3>Spending by Category</h3>
        <div class="category-budgets">
          {#each budget.categories as category}
            <div class="category-item">
              <h4>{category.name}</h4>
              <p>Spent: ${category.spent.toFixed(2)} / Allocated: ${category.allocated.toFixed(2)}</p>
              <progress value={category.spent} max={category.allocated}></progress>
            </div>
          {/each}
        </div>

        <h3>Recent Transactions</h3>
        <ul class="transaction-list">
          {#each budget.recentTransactions as trx}
            <li>
              <span>{trx.date}</span>
              <span>{trx.description}</span>
              <span class="transaction-category">{trx.category}</span>
              <span class="transaction-amount">-${trx.amount.toFixed(2)}</span>
            </li>
          {/each}
        </ul>
        <button class="primary-button">Add New Expense</button>
        <button>View Full Report</button>
      </section>
    {/if}

    {#if currentView === 'groceryList'}
      <section class="grocery-list-view card">
        <h2>My Grocery List</h2>
        {#if groceryList.length === 0}
          <p>Your grocery list is empty. Add items from recipes or manually.</p>
        {/if}
        <ul>
          {#each groceryList as item (item.item + item.recipeId)}
            <li class="{item.acquired ? 'acquired label' : ''}">
              <input type="checkbox" bind:checked={item.acquired}  id="item-{item.item.replace(/\s+/g, '-')}-{item.recipeId || 'general'}"/>
              <label for="item-{item.item.replace(/\s+/g, '-')}-{item.recipeId || 'general'}">
                {item.quantity} {item.item}
                {#if item.recipeId}
                  <span class="recipe-tag">(For: {recipes.find(r => r.id === item.recipeId)?.name || 'Recipe'})</span>
                {/if}
              </label>
              <button class="remove-item-btn" onclick={() => groceryList = groceryList.filter(i => i !== item)}>Remove</button>
            </li>
          {/each}
        </ul>
        <div class="grocery-actions">
          <input type="text" placeholder="New item name" bind:value={newGroceryItemName} />
          <input type="text" placeholder="Quantity" bind:value={newGroceryItemQuantity} />
          <button class="primary-button" onclick={addManualGroceryItem}>Add Item Manually</button>
          <button onclick={() => groceryList = groceryList.filter(i => !i.acquired)}>Clear Acquired</button>
        </div>
      </section>
    {/if}

  </main>

  <footer class="app-footer">
    <p>&copy; {new Date().getFullYear()} Your App Name. Happy Cooking & Budgeting!</p>
  </footer>

</div>

<style>
  /* --- Global Styles --- */
  :root {
    --primary-color: #4CAF50; /* Green */
    --secondary-color: #FFC107; /* Amber */
    --accent-color: #03A9F4; /* Light Blue */
    --text-color: #333;
    --bg-color: #f4f4f4;
    --card-bg: #fff;
    --border-radius: 8px;
    --box-shadow: 0 2px 10px rgba(0,0,0,0.1);
  }

  .app-container {
    font-family: 'Arial', sans-serif;
    color: var(--text-color);
    background-color: var(--bg-color);
    min-height: 100vh;
    display: flex;
    flex-direction: column;
  }

  .card {
    background-color: var(--card-bg);
    border-radius: var(--border-radius);
    box-shadow: var(--box-shadow);
    padding: 20px;
    margin-bottom: 20px;
  }

  button {
    padding: 10px 15px;
    border: none;
    border-radius: var(--border-radius);
    cursor: pointer;
    font-size: 1em;
    margin: 0 5px;
    background-color: var(--secondary-color);
    color: var(--text-color);
    transition: background-color 0.2s ease;
  }

  button.primary-button {
    background-color: var(--primary-color);
    color: white;
  }

  button:hover, button.primary-button:hover {
    opacity: 0.85;
  }

  input[type="text"], input[type="number"] {
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: var(--border-radius);
    margin-right: 10px;
    font-size: 1em;
  }

  /* --- Header --- */
  .app-header {
    background-color: var(--primary-color);
    color: white;
    padding: 15px 30px;
    text-align: center;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  }
  .app-header h1 {
    margin: 0 0 10px 0;
    font-size: 2em;
  }
  .app-header nav button {
    background-color: transparent;
    color: white;
    border: 1px solid white;
    margin: 0 8px;
  }
  .app-header nav button.active, .app-header nav button:hover {
    background-color: white;
    color: var(--primary-color);
  }

  /* --- Main Content --- */
  .main-content {
    flex-grow: 1;
    padding: 20px;
    max-width: 1200px;
    margin: 0 auto;
    width: 100%;
  }

  /* --- Recipe List View --- */
  .recipe-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 20px;
  }
  .recipe-card-small {
    border: 1px solid #eee;
    border-radius: var(--border-radius);
    padding: 15px;
    cursor: pointer;
    transition: transform 0.2s ease, box-shadow 0.2s ease;
    background-color: var(--card-bg);
  }
  .recipe-card-small:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 15px rgba(0,0,0,0.15);
  }
  .recipe-card-small-img {
    width: 100%;
    height: 180px;
    object-fit: cover;
    border-radius: var(--border-radius) var(--border-radius) 0 0;
    margin-bottom: 10px;
  }
  .recipe-card-small h3 {
    margin-top: 0;
    color: var(--primary-color);
  }

  /* --- Recipe Detail View --- */
  .recipe-header {
    display: flex;
    gap: 20px;
    margin-bottom: 20px;
    align-items: flex-start;
  }
  .recipe-image-large {
    width: 40%;
    max-width: 400px;
    height: auto;
    object-fit: cover;
    border-radius: var(--border-radius);
  }
  .recipe-header div {
    flex: 1;
  }
  .recipe-header h2 {
    margin-top: 0;
    color: var(--primary-color);
    font-size: 2.2em;
  }
  .recipe-meta {
    display: flex;
    flex-wrap: wrap;
    gap: 10px 20px;
    margin: 10px 0;
    font-size: 0.9em;
    color: #555;
  }
  .recipe-meta span strong {
    color: var(--text-color);
  }

  .recipe-content-grid {
    display: grid;
    grid-template-columns: 1fr; /* Default to single column for smaller screens */
    gap: 30px;
    margin-top: 20px;
  }

  @media (min-width: 768px) { /* Two columns for medium screens and up */
    .recipe-content-grid {
      grid-template-columns: 1fr 2fr; /* Ingredients on left, instructions on right */
    }
  }

  .ingredients-panel ul, .instructions-panel ol {
    list-style: none;
    padding-left: 0;
  }
  .ingredients-panel li, .instructions-panel li {
    padding: 8px 0;
    border-bottom: 1px solid #eee;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .ingredients-panel li:last-child, .instructions-panel li:last-child {
    border-bottom: none;
  }
  .add-to-grocery-btn {
    background: none;
    border: none;
    font-size: 1.2em;
    cursor: pointer;
    padding: 5px;
  }
  .add-to-grocery-btn:hover {
    color: var(--primary-color);
  }

  .nutritional-info {
    margin-top: 30px;
    padding-top: 20px;
    border-top: 1px solid #eee;
  }
  .nutritional-info h3 {
    margin-bottom: 10px;
  }
  .nutritional-info p {
    margin: 5px 0;
    font-size: 0.95em;
  }


  /* --- Budget View --- */
  .budget-summary {
    display: flex;
    justify-content: space-around;
    margin-bottom: 20px;
    text-align: center;
    font-size: 1.1em;
  }
  .budget-summary p.spent { color: #D32F2F; /* Red */ }
  .budget-summary p.remaining { color: var(--primary-color); }

  .budget-progress {
    margin-bottom: 30px;
    display: flex;
    align-items: center;
    gap: 10px;
  }
  .budget-progress label {
    font-weight: bold;
  }
  .budget-progress progress {
    width: 100%;
    height: 25px;
    border-radius: var(--border-radius);
  }
  .budget-progress progress::-webkit-progress-bar {
    background-color: #e0e0e0;
    border-radius: var(--border-radius);
  }
  .budget-progress progress::-webkit-progress-value {
    background-color: var(--primary-color);
    border-radius: var(--border-radius);
    transition: width 0.5s ease;
  }
  .budget-progress progress::-moz-progress-bar { /* Firefox */
    background-color: var(--primary-color);
    border-radius: var(--border-radius);
    transition: width 0.5s ease;
  }


  .category-budgets {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 15px;
    margin-bottom: 20px;
  }
  .category-item {
    border: 1px solid #ddd;
    padding: 15px;
    border-radius: var(--border-radius);
  }
  .category-item h4 {
    margin-top: 0;
  }
  .category-item progress {
    width: 100%;
    height: 10px;
  }

  .transaction-list {
    list-style: none;
    padding: 0;
  }
  .transaction-list li {
    display: flex;
    justify-content: space-between;
    padding: 10px 0;
    border-bottom: 1px solid #eee;
  }
  .transaction-list li:last-child {
    border-bottom: none;
  }
  .transaction-category {
    font-style: italic;
    color: #777;
    font-size: 0.9em;
  }
  .transaction-amount {
    font-weight: bold;
  }

  /* --- Grocery List View --- */
  .grocery-list-view ul {
    list-style: none;
    padding: 0;
  }
  .grocery-list-view li {
    display: flex;
    align-items: center;
    padding: 10px 0;
    border-bottom: 1px solid #eee;
  }
  .grocery-list-view li.acquired label {
    text-decoration: line-through;
    color: #aaa;
  }
  .grocery-list-view input[type="checkbox"] {
    margin-right: 10px;
    width: 20px;
    height: 20px;
    cursor: pointer;
  }
  .grocery-list-view label {
    flex-grow: 1;
    cursor: pointer;
  }
  .recipe-tag {
    font-size: 0.8em;
    color: #777;
    margin-left: 10px;
    background-color: #efefef;
    padding: 2px 6px;
    border-radius: 4px;
  }
  .remove-item-btn {
    background: none;
    border: 1px solid #ccc;
    color: #D32F2F;
    padding: 5px 10px;
    margin-left: 10px;
    border-radius: var(--border-radius);
  }
  .remove-item-btn:hover {
    background-color: #D32F2F;
    color: white;
  }
  .grocery-actions {
    margin-top: 20px;
    display: flex;
    flex-wrap: wrap; /* Allow wrapping on smaller screens */
    gap: 10px; /* Add some space between items */
  }
  .grocery-actions input[type="text"] {
    flex-grow: 1; /* Allow input fields to take available space */
    min-width: 150px; /* Ensure they don't get too small */
  }

  /* --- Footer --- */
  .app-footer {
    text-align: center;
    padding: 20px;
    background-color: #333;
    color: #aaa;
    font-size: 0.9em;
  }

  /* Responsive adjustments */
  @media (max-width: 768px) {
    .app-header {
      padding: 15px;
    }
    .app-header h1 {
      font-size: 1.5em;
    }
    .app-header nav {
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
    }
    .app-header nav button {
        margin-bottom: 5px;
    }
    .main-content {
      padding: 15px;
    }
    .recipe-header {
        flex-direction: column;
    }
    .recipe-image-large {
        width: 100%;
        max-width: none;
        margin-bottom: 15px;
    }
    .budget-summary {
        flex-direction: column;
        gap: 10px;
    }
  }

  @media (max-width: 480px) {
    .recipe-grid {
        grid-template-columns: 1fr; /* Single column for very small screens */
    }
    .transaction-list li {
        flex-direction: column;
        align-items: flex-start;
        gap: 5px;
    }
    .transaction-list li span {
        width: 100%;
    }
    .transaction-amount {
        text-align: left;
    }
    .grocery-actions {
        flex-direction: column;
    }
    .grocery-actions input[type="text"], .grocery-actions button {
        width: 100%;
        margin-bottom: 10px;
    }
  }

</style>
