#include <fstream>
#include <iostream>
#include <string>
#define el endl
using namespace std;

// Structure for subjects data
struct subjects {
    string subjects;
    string student_grade;
};
subjects sub[5];

// Structure for student data
struct student {
    string username;
    string password;
    string student_id;
    subjects choose; // choose the subject
};
student students[1000]; // Array to store student data

// Function to check if username or id exists
bool username_and_id_Exists(string username,string StudentId) {
    ifstream infile("check.txt");
    if (!infile.is_open()) {
        return false; // Assume username or id doesn't exist if file can't be opened for reading
    }

    string tempUsername, tempPassword, tempStudentId;
    while (infile >> tempUsername >> tempPassword >> tempStudentId) {
        if (username == tempUsername || StudentId == tempStudentId) {
            infile.close();
            return true; // Username or id found!
        }
    }

    infile.close();
    return false; // Username or id not found
}

// Function to sign up a new student
bool sign_up() {
    ofstream outfile("check.txt", ios::app); // Open in writting mode
    if (!outfile.is_open()) {
        cout << "Error opening file for writing!" << el;
        return false;
    }

    student new_student;

    cout << "Sign Up" << el;
    cout << "username: ";
    cin >> new_student.username;

  

    cout << "password: ";
    cin >> new_student.password;
    cout << "Confirm password: ";  // Added password confirmation for security
    string confirmPassword;
    cin >> confirmPassword;
    if (new_student.password != confirmPassword) {
        cout << "Passwords do not match. Please try again." << el;
        return false;
    }
    cout << "id: ";
    cin >> new_student.student_id;
    // Check if username & id exists before proceeding
    if (username_and_id_Exists(new_student.username, new_student.student_id)) {
        cout << "Username Or ID already taken!. Please choose another." << el;
        return false;
    }

    outfile << new_student.username << el << new_student.password << el << new_student.student_id << el; // Store data in file

    outfile.close(); // Close the file
    cout << "Registration successful!" << el;
    return true;
}

// Function to log in a student
bool login() {
    ifstream infile("check.txt");
    if (!infile.is_open()) {
        cout << "Error opening file for reading!" << el;
        return false;
    }

    string username, password, student_id;

    cout << "Login" << el;
    cout << "username: ";
    cin >> username;
    cout << "password: ";
    cin >> password;
    cout << "id: ";
    cin >> student_id;

    bool found = false;
    for (int i = 0; i < 1000; i++) {
        infile >> students[i].username >> students[i].password >> students[i].student_id;  // Read all students' data
        if (username == students[i].username && password == students[i].password && student_id == students[i].student_id) {
            found = true;
            break;
        }
    }

    infile.close(); // Close file

    if (found) {
        cout << "Login successful!" << el;
        return true;
    }
    else {
        cout << "Wrong data. Please try again." << el;
        return false;
    }
}

int main() {
    char account_variable;

    cout << "Do you have an account? (Y/N): ";
    cin >> account_variable;

    while (true) {
        if (account_variable == 'N' || account_variable == 'n') {
            if (sign_up()) {
                break;
            }
        }
        else if (account_variable == 'Y' || account_variable == 'y') {
            if (login()) {
                break;
            }
        }
    }
}
