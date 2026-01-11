#include <iostream>
#include <string>
using namespace std;

class InvalidBalanceException {
public:
    string message;
    InvalidBalanceException(string msg) { message = msg; }
};

class InsufficientFundsException {
public:
    string message;
    InsufficientFundsException(string msg) { message = msg; }
};

class BankAccount {
private:
    string holderName;
    double balance;

public:
    BankAccount(string name, double initialBalance) {
        holderName = name;
        cout << "Constructor called for " << holderName << endl;
        if (initialBalance < 0) throw InvalidBalanceException("Error: Initial balance cannot be negative!");
        balance = initialBalance;
        cout << "Account created successfully for " << holderName << " with balance " << balance << endl;
    }

    ~BankAccount() { cout << "Destructor called for " << holderName << endl; }

    void withdraw(double amount) {
        if (amount > balance) throw InsufficientFundsException("Error: Withdrawal amount exceeds balance!");
        balance -= amount;
        cout << "Withdrawal successful. New balance = " << balance << endl;
    }

    void display() { cout << "Account holder: " << holderName << ", Balance: " << balance << endl; }
};

int main() {
    BankAccount* acc = nullptr;

    try {
        acc = new BankAccount("Bushra", -100);
    }
    catch (InvalidBalanceException &e) {
        cout << e.message << endl;
    }

    try {
        acc = new BankAccount("Bushra", 1000);
        acc->display();
        acc->withdraw(500);
        acc->withdraw(600);
    }
    catch (InvalidBalanceException &e) { cout << e.message << endl; }
    catch (InsufficientFundsException &e) { cout << e.message << endl; }

    delete acc;
    acc = nullptr;

    return 0;
}
