# Phonebook-Management-System
The Phone Book Project aims to develop a comprehensive application for managing contact information, benefiting individuals and organizations alike. The application serves as a central repository for storing and retrieving contact details, including names, phone numbers, email addresses, and other relevant information.

Target Audience:
This application is designed for individuals, professionals, and organizations seeking efficient contact management solutions.

Application Structure:
The Phone Book application comprises the following components:

Contact List
Contact Details
Contact Groups
Search Functionality
Add, Edit, and Delete Contacts
Import and Export Contacts
User Settings
Features and Functionality:
The application offers functionalities such as:

Contact Management: Add, edit, and delete contacts with ease.
Contact Groups: Organize contacts into groups for better categorization.
Search Functionality: Quickly find contacts using search filters.
Import and Export: Import contacts from external sources and export them for backup or sharing.
User Settings: Customize preferences and settings for a personalized experience.
Benefits:

Simplified Contact Management: Easily manage and organize contact information.
Improved Accessibility: Access contacts anytime, anywhere, from any device.
Enhanced Efficiency: Streamline communication and collaboration with efficient contact management tools.
Data Security: Safeguard contact information with robust security measures.
Customization: Tailor the application to suit individual or organizational needs.
Conclusion:
The Phone Book Project provides a user-friendly and efficient solution for managing contact information, catering to the diverse needs of individuals and organizations. By centralizing contact details and offering comprehensive functionalities, the application enhances productivity, communication, and collaboration.
#include <iostream>
#include <fstream>
#include <string>
#include <conio.h>
using namespace std;
const int MAX_SIZE = 100;
string names[MAX_SIZE];
string emails[MAX_SIZE];
string phNums[11];
int curr = 0;
const string fileName = "contacts.txt";
void readFromFile() {
	// Check if the file exists, then read data from it and store it. Otherwise simply return
	ifstream f(fileName);
	if (!f) {
		return; // Simply returning if the file doesn't exist
	}	string tmp;
	while (getline(f, tmp)) {
		string cur = "";
		int it = 0;
		for (int i = 0; i < tmp.size(); i++) {
			if (tmp[i] == ':') {
				it++;
				if (it == 1) names[curr] = cur;
				else emails[curr] = cur;
				cur = "";
				continue;
			}
			cur += tmp[i];
		}
		phNums[curr++] = cur;
	}
	f.close();
}
void writeToFile() {
	// Writing to file:
	ofstream f(fileName, ios::in);
	for (int i = 0; i < curr; i++) {
		f << names[i] << ":" << emails[i] << ":" << phNums[i] << endl;
	}
	f.close();
}

void printContacts() {
	cout << "--> Total Contacts - " << curr << endl;

	for (int i = 0; i < curr; i++)
		cout << i + 1 << ") " << names[i] << " - " << emails[i] << " - " << phNums[i] << endl;

	cout << "======================" << endl;
}

void searchNumber() {
	string num;
	int index = -1;

	cout << "Enter the number you want to search details of: ";
	cin >> num;

	for (int i = 0; i < curr; i++) {
		if (phNums[i] == num)
			index = i;
		continue;
	}

	if (index == -1) {
		cout << "Number " << num << " doesn't exist!" << endl;
		return;
	}

	cout << "Details found!\n\n";
	cout << "Name         : " << names[index] << endl;
	cout << "Email Address: " << emails[index] << endl;
	cout << "Phone Number : " << phNums[index] << endl;
	cout << "==============================" << endl;
}
void updateNumber() {
	string num, tmp;
	int index = -1;
	bool emailUpdt = false;
	bool uNameUpdt = false;
	cout << "Enter the number you want to search details of: ";
	cin >> num;
	for (int i = 0; i < curr; i++) {
		if (phNums[i] == num)
			index = i;
		continue;
	}
	if (index == -1) {
		cout << "Number " << num << " doesn't exist!" << endl;
		return;
	}
	cout << "Enter the new name for " << num << ": ";
	getline(cin, tmp);
	if (tmp != "" && tmp != "\n") {
		// In case of non-empty strings, i would change
		names[index] = tmp;
		uNameUpdt = true;
	}

	cout << "Enter the new email address for " << num << ": ";
	getline(cin, tmp);
	if (tmp != "" && tmp != "\n") {
		// In case of non-empty strings, i would change
		emails[index] = tmp;
		emailUpdt = true;
	}
	if (uNameUpdt) {
		cout << "Name has been updated!" << endl;
	}
	if (emailUpdt) {
		cout << "Email updated successfully" << endl;
	}
	if (!uNameUpdt && !emailUpdt) {
		cout << "No changes detected!" << endl;
	}
	writeToFile();
}
void deleteNumber() {
	string num, tmp;
	int index = -1;
	cout << "Enter the number you want to delete details of: ";
	cin >> num;
	for (int i = 0; i < curr; i++) {
		if (phNums[i] == num)
			index = i;
		continue;
	}
	if (index == -1) {
		cout << "Number " << num << " doesn't exist!" << endl;
		return;
	}
	if (index != (curr - 1)) {
		for (int i = index; i < curr - 1; i++) {
			names[i] = names[i + 1];
			emails[i] = emails[i + 1];
			phNums[i] = phNums[i + 1];
		}
	}
	curr--;
	emails[curr] = "";
	names[curr] = "";
	phNums[curr] = "";
	writeToFile();
	cout << num << " deleted from contact list!" << endl;
}
void addNumber() {
	string c;
	cin.ignore();
	cout << "Enter the name: ";
	getline(cin, c);
	names[curr] = c;
	cout << "Enter the email: ";
	getline(cin, c);
	emails[curr] = c;
	cout << "Enter the phone Number: ";
	getline(cin, c);
	phNums[curr++] = c;
	// File handling
	ofstream f(fileName, ios::app);
	f << names[curr - 1] << ":" << emails[curr - 1] << ":" << phNums[curr - 1] << endl;
	f.close();
	cout << names[curr - 1] << " has been stored in the contacts list!" << std::endl;
}

int main() {
	char choice;
	readFromFile(); // Updating the current arrays
	do {
		system("cls");
		cout << ".......PHONEBOOK.......\n";
		cout << "1. Add a new number" << endl;
		cout << "2. List existing numbers" << endl;
		cout << "3. Search for a number" << endl;
		cout << "4. Update an existing contact" << endl;
		cout << "5. Delete a contact" << endl;
		cout << "0. Exit" << endl << endl;
		cout << ">> ";
		cin >> choice;
		switch (choice) {
		case '0':
			cout << "Thank you for using our program!" << endl;
			return 0;
		case '1':
			cout << "\n\tAdd New Member\n";
			addNumber();
			break;
		case '2':
			cout << "\n\tAll Numbers\n";
			printContacts();
			break;
		case '3':
			cout << "\n\tSearch Numbers\n";
			searchNumber();
			break;
		case '4':
			cout << "\n\tUpdate Numbers\n";
			updateNumber();
			break;
		case '5':
			cout << "\n\tDelete Numbers\n";
			deleteNumber();
			break;
		default:
			cout << "An invalid value has been entered!" << endl;
			cin.get();
			break;
		}
		cout << "Press any key to continue...";
		_getch();
		cout << "Choice: " << choice << endl;
		cout << "Condition: " << boolalpha << (choice < '0') << endl;
	} while (!(choice < '0' || choice > '5'));


