#include <iostream>
#include <vector>
#include <fstream>
#include <sstream>
#include <string>

using namespace std;

struct Record {
    string date;
    double income;
    double expenses;
    double savings;
};

vector<Record> records;

void loadRecords() {
    ifstream infile("records.txt");
    string line;
    while (getline(infile, line)) {
        stringstream ss(line);
        Record r;
        ss >> r.date >> r.income >> r.expenses >> r.savings;
        records.push_back(r);
    }
    infile.close();
}

void saveRecords() {
    ofstream outfile("records.txt");
    for (const auto& r : records) {
        outfile << r.date << " " << r.income << " " << r.expenses << " " << r.savings << "\n";
    }
    outfile.close();
}

void addRecord() {
    Record r;
    cout << "Enter date (YYYY-MM-DD): ";
    cin >> r.date;
    cout << "Enter income: ";
    cin >> r.income;
    cout << "Enter expenses: ";
    cin >> r.expenses;
    r.savings = r.income - r.expenses;
    records.push_back(r);
    saveRecords();  
    cout << "Record added successfully!\n";
}

void viewRecords() {
    cout << "\nDate        Income  Expenses  Savings\n";
    for (size_t i = 0; i < records.size(); ++i) {
        cout << i + 1 << ". " << records[i].date << "  " << records[i].income << "  " << records[i].expenses << "  " << records[i].savings << "\n";
    }
}

void totalSavings() {
    double totalIncome = 0, totalExpenses = 0, totalSavings = 0;
    for (const auto& r : records) {
        totalIncome += r.income;
        totalExpenses += r.expenses;
        totalSavings += r.savings;
    }
    cout << "\nTotal Income: " << totalIncome;
    cout << "\nTotal Expenses: " << totalExpenses;
    cout << "\nTotal Savings: " << totalSavings << "\n";
}

void editRecord() {
    viewRecords();
    int recordIndex;
    cout << "Enter the record number to edit: ";
    cin >> recordIndex;
    
    if (recordIndex < 1 || recordIndex > records.size()) {
        cout << "Invalid record number.\n";
        return;
    }

    
    Record &r = records[recordIndex - 1];
    
    cout << "Editing record: " << r.date << " " << r.income << " " << r.expenses << " " << r.savings << "\n";
    
    
    cout << "Enter new date (current: " << r.date << "): ";
    cin >> r.date;
    

    cout << "Enter new income (current: " << r.income << "): ";
    cin >> r.income;
    cout << "Enter new expenses (current: " << r.expenses << "): ";
    cin >> r.expenses;
    
    
    r.savings = r.income - r.expenses;  
    saveRecords();  
    cout << "Record updated successfully!\n";
}

int main() {
    loadRecords();  

    for (;;) {
        int choice;
        cout << "\n1. Add Record";
        cout << "\n2. View Records";
        cout << "\n3. View Total Savings";
        cout << "\n4. Edit Record";
        cout << "\n5. Exit";
        cout << "\nEnter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1: addRecord(); break;
            case 2: viewRecords(); break;
            case 3: totalSavings(); break;
            case 4: editRecord(); break;
            case 5: return 0;
            default: cout << "Invalid choice!\n";
        }
    }

    return 0;
}