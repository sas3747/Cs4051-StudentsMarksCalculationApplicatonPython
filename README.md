# Cs4051-StudentsMarksCalculationApplicatonPython
# Name of Program: Student Marks Calculation and Analysis
# Purpose of Program: Gathers, edits and draws statistics on the marks; using statistical functions.
# It computes mean, median, mode and skewness commands and support multiple data entry modes.
# Author of the Program: 24015195
# Date Programmed: May 1, 2025
#Function: mean_calculate()
# this function represents the average value of the list.
# It does this by adding all numbers then dividing it into the number of numbers.
#This allows us to appreciate the power of the dataset at the centre.
def mean_calculate(data):
    return sum(data) / len(data)

# Function: median_calculate()
# This function finds the median value and it occurs if the dataset is ordered.
# If the dataset is an odd it picks the middle value.
# If the dataset comes with an even, then an average value of the two middle numbers is computed.
# The median is of value when the dataset is outliere.
def median_calculate(data):
    sorted_data = sorted(data)
    n = len(sorted_data)
    mid = n // 2
    if n % 2 == 0:
        return (sorted_data[mid - 1] + sorted_data[mid]) / 2
    else:
        return sorted_data[mid]

# Function: calculate_mode()
# This function finds the number which is often repeated in the list.
# If a set of numbers repeats the same highest numbers of times, it returns the message there is no unique mode.
# Mode serves to identify trends or scores, in occurrence.
def calculate_mode(data):
    frequency = {}
    for num in data:
        frequency[num] = frequency.get(num, 0) + 1
    max_freq = max(frequency.values())
    modes = [k for k, v in frequency.items() if v == max_freq]
    if len(modes) == 1:
        return modes[0]
    else:
        return "No unique mode"

# Function: standard_deviation_calulation()
# This helper function calculates standard deviation of data set.
# Through standard deviation we are told how wide the numbers are from the mean.
def standard_deviation_calulation(data):
    mean_val = mean_calculate(data)
    variance = sum((x - mean_val) ** 2 for x in data) / len(data)
    return variance ** 0.5

# Function: skewness_calculate()
# Using this function, you will know how 'skewed' your data is – skewed to the left or right.
# It uses the industry formula on statistics (Pearson’s moment coefficient of skewness).
# Skew positive = long tail on the right; negative long tail, on the left.
def skewness_calculate(data):
    n = len(data)
    if n < 2:
        return 0
    mean_val = mean_calculate(data)
    sd = standard_deviation_calulation(data)
    if sd == 0:
        return 0
    skew = (sum((x - mean_val) ** 3 for x in data) / n) / (sd ** 3)
    return skew

# Function: get_marks_individually()
# With this function the user can key mark at a time.
# If typed ‘done’ will terminate the input process.
# Valid numbers only can be keyed in – errors covered.
def get_marks_individually():
    marks = []
    print("Insert marks one at a time and type `done` when finished inserting.")
    while True:
        value = input("Enter mark: ").strip()
        if value.lower() == 'done':
            break
        try:
            number = float(value)
            marks.append(number)
        except ValueError:
            print("Invalid input. Please enter a valid number.")
    return marks

# Function: get_marks_comma_separated()
# This function allows the user to be able to key in different marks in one line separated by commas.
# It checks whether every entry is a number and removes unwanted spaces.
def get_marks_comma_separated():
    while True:
        values = input("Enter comma-separated marks: ").strip()
        try:
            mark_list = [float(x.strip()) for x in values.split(",") if x.strip() != '']
            return mark_list
        except ValueError:
            print("Please enter only numbers separated by commas.")

# Function: get_marks_from_file()
# This operation loads a file with comma separated numbers on it.
# After that, it reads the content of the file and makes its values float structures.
# It tells the user if the file doesn’t exist or has bad data.
def get_marks_from_file():
    filename = input("Enter the path or name of the file (e.g., marks.txt): ").strip().strip('"')
    try:
        with open(filename, 'r') as file:
            contents = file.read()
            return [float(x.strip()) for x in contents.split(",") if x.strip() != '']
    except FileNotFoundError:
        print(f"File '{filename}' not found.")
        return []
    except ValueError:
        print("File contains non-numeric or invalid data.")
        return []

# Function: display_menu()
# This function prints a menu to a user showing the user what they can do.
# The user can do a calculation of statistics, restart, put data or quit.
def display_menu():
    print("\nMenu Options:")
    print("1. Print the mean of the numbers")
    print("2. Print the median of the numbers")
    print("3. Print the mode of the numbers")
    print("4. Print the skewness of the numbers")
    print("5. Enter a NEW set of numbers")
    print("6. Add MORE numbers to the existing list")
    print("7. Exit the application")

# Function: main()
#That is what the program is all about, everything comes together.
# It allows the user to have choice in that he/she can choose to enter his/ her marks one by one or comma separated or from a file.
# After Data gathering, it displays a menu, which will enable the user to perform statistics, or to update his data.
# The program continues until the person quit.
def main():
    print("===== Student Marks Calculation Application =====")
    marks = []

    while True:
        input_mode = input("\nChoose input mode:\n1 - One by one\n2 - Comma-separated\nfile - Read from file\nYour choice: ").strip()

        if input_mode == '1':
            marks = get_marks_individually()
        elif input_mode == '2':
            marks = get_marks_comma_separated()
        elif input_mode.lower() == 'file':
            marks = get_marks_from_file()
        else:
            print("Invalid option. Please enter 1, 2, or 'file'.")
            continue

        if len(marks) < 2:
            print("You must enter at least two marks to proceed.")
            continue

        while True:
            print(f"\nYou have entered {len(marks)} marks: {marks}")
            display_menu()
            choice = input("Enter your choice: ").strip()

            if choice == '1':
                print("Mean of the numbers:", round(mean_calculate(marks), 2))
            elif choice == '2':
                print("Median of the numbers:", median_calculate(marks))
            elif choice == '3':
                print("Mode of the numbers:", calculate_mode(marks))
            elif choice == '4':
                print("Skewness of the numbers:", round(skewness_calculate(marks), 2))
            elif choice == '5':
                print("Restarting input...")
                break
            elif choice == '6':
                more_data = get_marks_comma_separated()
                marks.extend(more_data)
                print("Marks updated.")
            elif choice == '7':
                print("Goodbye!")
                return
            else:
                print("Invalid menu choice. Please select from 1 to 7.")


# Run the program

if __name__ == "__main__":
    main()
