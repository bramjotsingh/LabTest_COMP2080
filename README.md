### Lab Test v2 COMP2080  

#### Instructions:  
1. Write your solution and test your program for the specification given.  
2. Copy and Paste the code for your solution directly into D2L (you may submit only ONCE).  

---

### Employee Class Definition  
A company requires a system to manage information on Employee names as outlined below. The UML for the Employee class is shown below.

#### Employee UML:  
| Employee |  
|-------------|  
| + employeeCode: string |  
| + name: string |  
| + position: string |  
| + phone: string |  
| + officeNum: int |  
| + Employee(employeeCode: string, name: string, position: string, phone: string, officeNum: int) |  

A company requires a system to manage information on objects of type Employee as outlined.  
- Note: No accessors, mutators, or other methods are required, and all state attributes are public.  

#### Task:  
Create a class Employee as outlined above and paste the code for the Employee class definition in the space provided in the submission document. [1 mark]  

---

### OrderedObjects Class Definition  
You must create a simple collection class called OrderedObjects that uses a basic array to store objects of the type Employee as defined above. The list of objects must be stored in sorted order at all times.  

- The objects must be stored in ascending order by name.  
- The maximum number of objects that can be stored is 200, and this must be set in the constructor.  
- The constructor will take no parameters.  

[1.5 marks]  

---

### Methods to be Created in OrderedObjects  
(Only create these methods. No extra ones.)  

#### *Method 1: addObject*  
A method called addObject that adds an object of the type outlined above. The method must:  
- Take all the parameters required to create the Employee object.  
- Create a new object and add it ONLY if no other object exists with the same name and there is space to add it in the array.  
- Sort the array using selection sort.  
- This method returns a Boolean value to indicate a successful addition or not.  

(Hint: You may want to create and use another method first to make this more efficient and easy.)  

[3.5 marks]  

---

#### *Method 2: binarySearch*  
A private method called binarySearch that takes a parameter representing an employee’s name as a search value and efficiently returns:  
- The location in the array of an object with this value if found.  
- *-1 if not found*.  

[3.5 marks]  

---

#### *Method 3: employeeExists*  
A method called employeeExists that takes a parameter representing an employee name as a search value and returns a Boolean value indicating whether or not an object with a matching employee name exists.  

[0.5 marks]

// ****SOLUTION=>

// Employee class as per the given UML
public class Employee {
    public String employeeCode;
    public String name;
    public String position;
    public String phone;
    public int officeNum;

    // Constructor
    public Employee(String employeeCode, String name, String position, String phone, int officeNum) {
        this.employeeCode = employeeCode;
        this.name = name;
        this.position = position;
        this.phone = phone;
        this.officeNum = officeNum;
    }
}

// OrderedObjects class to store and manage Employee objects
class OrderedObjects {
    private Employee[] employees; // Array to store Employee objects
    private int size; // Tracks the number of stored objects
    private static final int MAX_SIZE = 200; // Maximum allowed objects

    // Constructor
    public OrderedObjects() {
        employees = new Employee[MAX_SIZE];
        size = 0;
    }

    // Method to add an Employee object
    public boolean addObject(String employeeCode, String name, String position, String phone, int officeNum) {
        // Check if name already exists
        if (employeeExists(name) || size >= MAX_SIZE) {
            return false; // Employee with the same name exists OR array is full
        }

        // Add the new Employee object
        employees[size] = new Employee(employeeCode, name, position, phone, officeNum);
        size++;

        // Sort the array using selection sort
        selectionSort(); // insertionSort(); 
        return true; // Successfully added
    }

    // Selection Sort to sort employees by name
    private void selectionSort() {
        for (int i = 0; i < size - 1; i++) {
            int minIndex = i;
            for (int j = i + 1; j < size; j++) {
                if (employees[j].name.compareTo(employees[minIndex].name) < 0) {// >0 desc
                    minIndex = j;
                }
            }
            // Swap
            Employee temp = employees[i];
            employees[i] = employees[minIndex];
            employees[minIndex] = temp;
        }
    }

    // Alternative Insertion Sort (more efficient for adding elements dynamically)
    private void insertionSort() {
        for (int i = 1; i < size; i++) {
            Employee key = employees[i];
            int j = i - 1;

            while (j >= 0 && employees[j].name.compareTo(key.name) > 0) { //< 0 desc
                employees[j + 1] = employees[j];
                j = j - 1;
            }
            employees[j + 1] = key;
        }
    }

    // Private binary search method
    private int binarySearch(String name) {
        int left = 0, right = size - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            int cmp = employees[mid].name.compareTo(name);
            if (cmp == 0) {
                return mid; // Found
            } else if (cmp < 0) { //> 0 desc
                left = mid + 1; // Search right
            } else {
                right = mid - 1; // Search left
            }
        }
        return -1; // Not found
    }

    // Method to check if an employee exists
    public boolean employeeExists(String name) {
        return binarySearch(name) != -1;
    }
}
