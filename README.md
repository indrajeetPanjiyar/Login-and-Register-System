#include <iostream>
#include <fstream>
#include <string>
#include <vector>

using namespace std;

// Structure to store user data
struct User {
    string username;
    string password;
};

// Function to register a new user
void registerUser() {
    User newUser;
    cout << "Enter a username: ";
    cin >> newUser.username;
    cout << "Enter a password: ";
    cin >> newUser.password;
    // Check if username already exists
    ifstream file("users.txt");
    string line;
    bool usernameExists = false;
    while (getline(file, line)) {
        if (line == newUser.username) {
            usernameExists = true;
            break;
        }
    }
    file.close();

    if (usernameExists) {
        cout << "Username already exists. Please try again." << endl;
    } else {
        // Append new user to file
        ofstream outFile("users.txt", ios_base::app);
        outFile << newUser.username << endl;
        outFile << newUser.password << endl;
        outFile.close();
        cout << "Registration successful!" << endl;
    }
}

// Function to login an existing user
void loginUser() {
    string username, password;
    cout << "Enter your username: ";
    cin >> username;
    cout << "Enter your password: ";
    cin >> password;

    // Check credentials
    ifstream file("users.txt");
    string line;
    bool loginSuccessful = false;
    string storedUsername, storedPassword;
    while (getline(file, line)) {
        storedUsername = line;
        getline(file, line);
        storedPassword = line;
        if (storedUsername == username && storedPassword == password) {
            loginSuccessful = true;
            break;
        }
    }
    file.close();

    if (loginSuccessful) {
        cout << "Login successful! Welcome, " << username << endl;
    } else {
        cout << "Invalid username or password. Please try again." << endl;
    }
}

int main() {
    int choice;
    while (true) {
        cout << "1. Register" << endl;
        cout << "2. Login" << endl;
        cout << "3. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                registerUser();
                break;
            case 2:
                loginUser();
                break;
            case 3:
                cout << "Exiting program. Goodbye!" << endl;
                return 0;
            default:
                cout << "Invalid choice. Please try again." << endl;
        }
    }

    return 0;
}
