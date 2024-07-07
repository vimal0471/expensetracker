# expensetracker
import java.io.*;
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

class Expense {
    private String date;
    private String category;
    private double amount;
    private String description;

    public Expense(String date, String category, double amount, String description) {
        this.date = date;
        this.category = category;
        this.amount = amount;
        this.description = description;
    }

    public String getDate() {
        return date;
    }

    public String getCategory() {
        return category;
    }

    public double getAmount() {
        return amount;
    }

    public String getDescription() {
        return description;
    }

    @Override
    public String toString() {
        return "Date: " + date + ", Category: " + category + ", Amount: $" + amount + ", Description: " + description;
    }
}

class ExpenseTracker {
    private List<Expense> expenses;
    private String filename;

    public ExpenseTracker(String filename) {
        this.filename = filename;
        this.expenses = new ArrayList<>();
        loadExpenses();
    }

    public void addExpense(String date, String category, double amount, String description) {
        expenses.add(new Expense(date, category, amount, description));
    }

    public void viewExpenses() {
        for (Expense expense : expenses) {
            System.out.println(expense);
        }
    }

    public void saveExpenses() {
        try (PrintWriter writer = new PrintWriter(new FileWriter(filename))) {
            for (Expense expense : expenses) {
                writer.println(expense.getDate() + "," + expense.getCategory() + "," + expense.getAmount() + "," + expense.getDescription());
            }
        } catch (IOException e) {
            System.err.println("Error saving expenses: " + e.getMessage());
        }
    }

    private void loadExpenses() {
        File file = new File(filename);
        if (file.exists()) {
            try (Scanner scanner = new Scanner(file)) {
                while (scanner.hasNextLine()) {
                    String[] fields = scanner.nextLine().split(",");
                    if (fields.length == 4) {
                        expenses.add(new Expense(fields[0], fields[1], Double.parseDouble(fields[2]), fields[3]));
                    }
                }
            } catch (IOException e) {
                System.err.println("Error loading expenses: " + e.getMessage());
            }
        }
    }
}

public class Main {
    public static void main(String[] args) {
        ExpenseTracker tracker = new ExpenseTracker("expenses.txt");
        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.println("\nExpense Tracker");
            System.out.println("1. Add Expense");
            System.out.println("2. View Expenses");
            System.out.println("3. Save and Exit");
            System.out.print("Choose an option: ");
            String choice = scanner.nextLine();

            if (choice.equals("1")) {
                System.out.print("Enter date (YYYY-MM-DD): ");
                String date = scanner.nextLine();
                System.out.print("Enter category: ");
                String category = scanner.nextLine();
                System.out.print("Enter amount: ");
                double amount = Double.parseDouble(scanner.nextLine());
                System.out.print("Enter description: ");
                String description = scanner.nextLine();
                tracker.addExpense(date, category, amount, description);
            } else if (choice.equals("2")) {
                tracker.viewExpenses();
            } else if (choice.equals("3")) {
                tracker.saveExpenses();
                System.out.println("Expenses saved. Exiting...");
                break;
            } else {
                System.out.println("Invalid choice. Please try again.");
            }
        }

        scanner.close();
    }
}
                                         
