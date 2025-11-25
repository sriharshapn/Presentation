# Presentation
Smart budget planner with multiple currencies in C
    
    #include <stdio.h>
    #include <stdlib.h>
    #include <string.h>

    // Define exchange rates relative to INR (base currency)
    // Update these with current rates for accuracy
    #define INR_TO_INR 1.0
    #define USD_TO_INR 83.0  // 1 USD ≈ 83 INR
    #define EUR_TO_INR 90.0  // 1 EUR ≈ 90 INR
    #define GBP_TO_INR 105.0  // 1 GBP ≈ 105 INR
    #define JPY_TO_INR 0.55  // 1 JPY ≈ 0.55 INR

    // Function to get exchange rate for a given currency
    double get_exchange_rate(const char* currency) 
    {
      if (strcmp(currency, "INR") == 0) return INR_TO_INR;
    if (strcmp(currency, "USD") == 0) return USD_TO_INR;
    if (strcmp(currency, "EUR") == 0) return EUR_TO_INR;
    if (strcmp(currency, "GBP") == 0) return GBP_TO_INR;
    if (strcmp(currency, "JPY") == 0) return JPY_TO_INR;
    printf("Unsupported currency: %s. Using INR as default.\n", currency);
    return INR_TO_INR;
    }

    // Function to convert amount to INR
    double convert_to_inr(double amount, const char* currency) 
    {
    return amount * get_exchange_rate(currency);
    }

    // Main function
    int main() 
    {
    double budget;
    double expenses[4] = {0.0};  // Array for categories: 0=Travel Charges, 1=Accommodation, 2=Food, 3=Activities
    char categories[4][20] = {"Travel Charges", "Accommodation", "Food", "Activities"};
    double total_expenses = 0.0;
    char add_more;

    printf("=== Smart Travel Budget Planner ===\n");
    printf("Base currency: INR\n");
    printf("Supported currencies: INR, USD, EUR, GBP, JPY\n\n");

    // Input budget
    printf("Enter your total travel budget in INR: ");
    scanf("%lf", &budget);
    if (budget <= 0) 
    {
        printf("Invalid budget. Exiting.\n");
        return 1;
    }

    // Interactive loop to add expenses
    printf("\nYou can now add expenses interactively. Enter 'y' to add an expense or 'n' to finish and view summary.\n");
    while (1) 
    {
          printf("\nDo you want to add an expense? (y/n): ");
          scanf(" %c", &add_more);  // Note: space before %c to consume newline
           if (add_more == 'n' || add_more == 'N') 
        {
            break;
        }
          else if (add_more != 'y' && add_more != 'Y') 
        {
            printf("Invalid input. Please enter 'y' or 'n'.\n");
            continue;
        }

        int category;
        double amount;
        char currency[4];

        printf("Category (0=Travel Charges, 1=Accommodation, 2=Food, 3=Activities): ");
        scanf("%d", &category);
        if (category < 0 || category > 3) 
        {
            printf("Invalid category. Skipping.\n");
            continue;
        }

        printf("Amount: ");
        scanf("%lf", &amount);
        if (amount <= 0) 
        {
            printf("Invalid amount. Skipping.\n");
            continue;
        }

        printf("Currency (e.g., INR, USD): ");
        scanf("%s", currency);

        // Convert to INR and add to category
        double inr_amount = convert_to_inr(amount, currency);
        expenses[category] += inr_amount;
        total_expenses += inr_amount;

        printf("Added %.2f INR (converted from %.2f %s) to %s.\n", inr_amount, amount, currency, categories[category]);
    }

    // Calculate and display results
    printf("\n=== Budget Summary ===\n");
    printf("Total Budget: %.2f INR\n", budget);
    printf("Total Expenses: %.2f INR\n", total_expenses);
    printf("Remaining Budget: %.2f INR\n", budget - total_expenses);

    // Breakdown by category
    printf("\nExpense Breakdown:\n");
    for (int i = 0; i < 4; i++) {
        if (expenses[i] > 0) {
            double percentage = (expenses[i] / total_expenses) * 100;
            printf("- %s: %.2f INR (%.1f%% of total)\n", categories[i], expenses[i], percentage);
        }
    }

    // Smart alerts and suggestions
    double budget_used_percentage = (total_expenses / budget) * 100;
    printf("\nBudget Used: %.1f%%\n", budget_used_percentage);

    if (total_expenses > budget) 
    {
        printf("ALERT: You are over budget by %.2f INR!\n", total_expenses - budget);
        printf("Suggestion: Review high-cost categories and cut non-essentials.\n");
    }   
      else if (budget_used_percentage > 80) 
      {
        printf("WARNING: You're close to your budget limit.\n");
        printf("Suggestion: Consider cheaper alternatives for remaining plans.\n");
    } 
      else 
      {
        printf("Great! You're within budget.\n");
        printf("Suggestion: Allocate any savings to unexpected costs or upgrades.\n");
    }

    return 0;
}
