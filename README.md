import csv
import os

class EmployeeManager:
    def __init__(self, filename='employees.csv'):
        self.filename = filename
        self.employees = {}
        self.load_data()
    
    def load_data(self):
        if os.path.exists(self.filename):
            with open(self.filename, 'r', newline='') as file:
                reader = csv.DictReader(file)
                for row in reader:
                    self.employees[row['ID']] = {
                        'Name': row['Name'],
                        'Position': row['Position'],
                        'Salary': row['Salary'],
                        'Email': row['Email']
                    }
    
    def save_data(self):
        with open(self.filename, 'w', newline='') as file:
            fieldnames = ['ID', 'Name', 'Position', 'Salary', 'Email']
            writer = csv.DictWriter(file, fieldnames=fieldnames)
            writer.writeheader()
            for emp_id, details in self.employees.items():
                writer.writerow({
                    'ID': emp_id,
                    'Name': details['Name'],
                    'Position': details['Position'],
                    'Salary': details['Salary'],
                    'Email': details['Email']
                })
    
    def add_employee(self):
        print("\n--- Add New Employee ---")
        
        emp_id = input("Enter Employee ID: ").strip()
        if emp_id in self.employees:
            print("Error: Employee ID already exists!")
            return
        
        name = input("Enter Name: ").strip()
        if not name:
            print("Error: Name cannot be empty!")
            return
        
        position = input("Enter Position: ").strip()
        if not position:
            print("Error: Position cannot be empty!")
            return
        
        salary = input("Enter Salary: ").strip()
        if not salary.isdigit():
            print("Error: Salary must be a number!")
            return
        
        email = input("Enter Email: ").strip()
        if not email or '@' not in email:
            print("Error: Invalid email format!")
            return
        
        self.employees[emp_id] = {
            'Name': name,
            'Position': position,
            'Salary': salary,
            'Email': email
        }
        self.save_data()
        print("Employee added successfully!")
    
    def view_all_employees(self):
        print("\n--- All Employees ---")
        if not self.employees:
            print("No employees found.")
            return
        
        for emp_id, details in self.employees.items():
            print(f"ID: {emp_id}")
            print(f"Name: {details['Name']}")
            print(f"Position: {details['Position']}")
            print(f"Salary: ${details['Salary']}")
            print(f"Email: {details['Email']}")
            print("-" * 30)
    
    def update_employee(self):
        print("\n--- Update Employee ---")
        emp_id = input("Enter Employee ID to update: ").strip()
        
        if emp_id not in self.employees:
            print("Error: Employee ID not found!")
            return
    
        details = self.employees[emp_id]
        print(f"Current Name: {details['Name']}")
        print(f"Current Position: {details['Position']}")
        print(f"Current Salary: {details['Salary']}")
        print(f"Current Email: {details['Email']}")
        
        print("\nEnter new values (leave blank to keep current):")
        name = input("Enter Name: ").strip()
        position = input("Enter Position: ").strip()
        salary = input("Enter Salary: ").strip()
        email = input("Enter Email: ").strip()
        
        if name:
            details['Name'] = name
        if position:
            details['Position'] = position
        if salary:
            if not salary.isdigit():
                print("Error: Salary must be a number!")
                return
            details['Salary'] = salary
        if email:
            if '@' not in email:
                print("Error: Invalid email format!")
                return
            details['Email'] = email
        
        self.save_data()
        print("Employee updated successfully!")
    
    def delete_employee(self):
        """Delete an employee from the system"""
        print("\n--- Delete Employee ---")
        emp_id = input("Enter Employee ID to delete: ").strip()
        
        if emp_id not in self.employees:
            print("Error: Employee ID not found!")
            return
        
        confirm = input(f"Are you sure you want to delete employee {emp_id}? (y/n): ").strip().lower()
        if confirm == 'y':
            del self.employees[emp_id]
            self.save_data()
            print("Employee deleted successfully!")
        else:
            print("Deletion cancelled.")
    
    def search_employee(self):
        print("\n--- Search Employee ---")
        emp_id = input("Enter Employee ID to search: ").strip()
        
        if emp_id in self.employees:
            details = self.employees[emp_id]
            print(f"ID: {emp_id}")
            print(f"Name: {details['Name']}")
            print(f"Position: {details['Position']}")
            print(f"Salary: ${details['Salary']}")
            print(f"Email: {details['Email']}")
        else:
            print("Employee not found!")

def main():
    manager = EmployeeManager()
    
    while True:
        print("\n=== Employee Management System ===")
        print("1. Add Employee")
        print("2. View All Employees")
        print("3. Update Employee")
        print("4. Delete Employee")
        print("5. Search Employee")
        print("6. Exit")
        
        choice = input("Enter your choice (1-6): ").strip()
        
        if choice == '1':
            manager.add_employee()
        elif choice == '2':
            manager.view_all_employees()
        elif choice == '3':
            manager.update_employee()
        elif choice == '4':
            manager.delete_employee()
        elif choice == '5':
            manager.search_employee()
        elif choice == '6':
            print("Thank you for using the Employee Management System!")
            break
        else:
            print("Invalid choice! Please enter a number between 1-6.")

if __name__ == "__main__":
    main()
