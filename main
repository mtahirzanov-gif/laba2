#include <iostream>
#include <string>
#include <vector>
#include <map>
#include <limits>
#include <algorithm>
#include <cctype>

using namespace std;

// Enum - виды работ
enum WorkType {
    PROGRAMMING = 1,
    TESTING,
    DESIGN,
    DOCUMENTATION,
    MANAGEMENT
};

// Enum - должности работников
enum Position {
    DEVELOPER = 1,
    MANAGER,
    TESTER,
    DESIGNER,
    ANALYST,
    INTERN
};

// Функции ввода
int inputNumber(string prompt, int min = 0, int max = 10000);
double inputDouble(string prompt, double max = 10000.0);
string inputString(string prompt);

// Функция для преобразования enum Position в строку
string positionToString(Position pos) {
    switch (pos) {
        case DEVELOPER: return "Разработчик";
        case MANAGER: return "Менеджер";
        case TESTER: return "Тестировщик";
        case DESIGNER: return "Дизайнер";
        case ANALYST: return "Аналитик";
        case INTERN: return "Стажер";
        default: return "Неизвестно";
    }
}

// Функция для преобразования enum WorkType в строку
string workTypeToString(WorkType type) {
    switch (type) {
        case PROGRAMMING: return "Программирование";
        case TESTING: return "Тестирование";
        case DESIGN: return "Дизайн";
        case DOCUMENTATION: return "Документирование";
        case MANAGEMENT: return "Управление";
        default: return "Неизвестно";
    }
}

// Функция для отображения списка должностей
void displayPositions() {
    cout << "Список должностей:\n";
    cout << "1. Разработчик\n";
    cout << "2. Менеджер\n";
    cout << "3. Тестировщик\n";
    cout << "4. Дизайнер\n";
    cout << "5. Аналитик\n";
    cout << "6. Стажер\n";
}

// Функция для отображения списка видов работ
void displayWorkTypes() {
    cout << "Виды работ:\n";
    cout << "1. Программирование\n";
    cout << "2. Тестирование\n";
    cout << "3. Дизайн\n";
    cout << "4. Документирование\n";
    cout << "5. Управление\n";
}

// Класс работника
class Employee {
private:
    string surname;
    Position position;
    vector<pair<WorkType, double>> works;
    
public:
    Employee(string sname, Position pos = DEVELOPER)
        : surname(sname), position(pos) {}
    
    void addWork(WorkType type, double hours) {
        works.push_back({type, hours});
    }
    
    double calculateSalary(const map<WorkType, double>& rates) const {
        double total = 0;
        for (const auto& work : works) {
            total += work.second * rates.at(work.first);
        }
        return total;
    }
    
    string getSurname() const { return surname; }
    Position getPosition() const { return position; }
    string getPositionString() const { return positionToString(position); }
    size_t getWorkCount() const { return works.size(); }
    
    void printInfo() const {
        cout << surname << " (" << positionToString(position)
             << ") - работ: " << works.size();
    }
    
    void printDetailedInfo(const map<WorkType, double>& rates) const {
        printInfo();
        double salary = calculateSalary(rates);
        cout << " - зарплата: " << salary << " руб.";
        
        // Детализация по работам
        if (!works.empty()) {
            cout << "\n    Детализация:\n";
            for (size_t i = 0; i < works.size(); i++) {
                double cost = works[i].second * rates.at(works[i].first);
                cout << "    - " << workTypeToString(works[i].first) << ": "
                     << works[i].second << " ч. × " << rates.at(works[i].first)
                     << " руб./ч. = " << cost << " руб.\n";
            }
        }
    }
    
    ~Employee() {
        works.clear();
    }
};

// Класс для управления ставками
class RateManager {
private:
    map<WorkType, double> rates;
    
public:
    RateManager() {
        // Стандартные ставки по умолчанию
        rates[PROGRAMMING] = 1000;
        rates[TESTING] = 800;
        rates[DESIGN] = 900;
        rates[DOCUMENTATION] = 600;
        rates[MANAGEMENT] = 1200;
    }
    
    void setRate(WorkType type, double rate) {
        rates[type] = rate;
    }
    
    double getRate(WorkType type) const {
        return rates.at(type);
    }
    
    const map<WorkType, double>& getAllRates() const {
        return rates;
    }
    
    void showRates() const {
        cout << "Текущие ставки оплаты:\n";
        for (int i = 1; i <= 5; i++) {
            WorkType type = static_cast<WorkType>(i);
            cout << i << ". " << workTypeToString(type) << ": "
                 << rates.at(type) << " руб./час\n";
        }
    }
    
    ~RateManager() {
        rates.clear();
    }
};

// Главный менеджер (Singleton)
class PayrollManager {
private:
    static PayrollManager* instance;
    vector<Employee> employees;
    RateManager rateManager;
    
    PayrollManager() {}
    
    // Вспомогательный метод для поиска всех сотрудников с фамилией
    vector<size_t> findEmployeesBySurname(const string& surname) const {
        vector<size_t> indices;
        for (size_t i = 0; i < employees.size(); i++) {
            if (employees[i].getSurname() == surname) {
                indices.push_back(i);
            }
        }
        return indices;
    }
    
    // Проверка существования работника с такой же фамилией и должностью
    bool employeeExists(const string& surname, Position position) const {
        for (const auto& emp : employees) {
            if (emp.getSurname() == surname && emp.getPosition() == position) {
                return true;
            }
        }
        return false;
    }
    
public:
    static PayrollManager* getInstance() {
        if (!instance) instance = new PayrollManager();
        return instance;
    }
    
    void addEmployee(const string& surname, Position position) {
        // Проверяем, нет ли уже работника с такой фамилией и должностью
        if (employeeExists(surname, position)) {
            cout << "✗ Ошибка: работник " << surname << " ("
                 << positionToString(position) << ") уже существует!\n";
            return;
        }
        
        employees.push_back(Employee(surname, position));
        cout << "✓ Работник " << surname << " (" << positionToString(position)
             << ") успешно добавлен!\n";
    }
    
    // Метод добавления работы по фамилии и должности
    void addWork(const string& surname, Position position, WorkType type, double hours) {
        for (size_t i = 0; i < employees.size(); i++) {
            if (employees[i].getSurname() == surname &&
                employees[i].getPosition() == position) {
                employees[i].addWork(type, hours);
                cout << "✓ Работа добавлена для " << surname
                     << " (" << positionToString(position) << ") - "
                     << workTypeToString(type) << ": " << hours << " ч.\n";
                return;
            }
        }
        cout << "✗ Ошибка: работник " << surname << " ("
             << positionToString(position) << ") не найден!\n";
    }
    
    // Расчет зарплаты по фамилии и должности
    void showEmployeeSalary(const string& surname, Position position) const {
        for (size_t i = 0; i < employees.size(); i++) {
            if (employees[i].getSurname() == surname &&
                employees[i].getPosition() == position) {
                double salary = employees[i].calculateSalary(rateManager.getAllRates());
                cout << " Зарплата " << surname << " (" << positionToString(position)
                     << "): " << salary << " руб.\n";
                return;
            }
        }
        cout << "✗ Ошибка: работник " << surname << " ("
             << positionToString(position) << ") не найден!\n";
    }
    
    // Детальная информация о работнике
    void showEmployeeDetails(const string& surname, Position position) const {
        for (size_t i = 0; i < employees.size(); i++) {
            if (employees[i].getSurname() == surname &&
                employees[i].getPosition() == position) {
                cout << " Детальная информация:\n";
                employees[i].printDetailedInfo(rateManager.getAllRates());
                cout << "\n";
                return;
            }
        }
        cout << "✗ Ошибка: работник " << surname << " ("
             << positionToString(position) << ") не найден!\n";
    }
    
    // Поиск работников по фамилии
    void searchEmployeesBySurname(const string& surname) const {
        vector<size_t> indices = findEmployeesBySurname(surname);
        
        if (indices.empty()) {
            cout << "Работники с фамилией '" << surname << "' не найдены.\n";
            return;
        }
        
        cout << "Найдено " << indices.size() << " работников:\n";
        for (size_t i = 0; i < indices.size(); i++) {
            cout << (i + 1) << ". ";
            employees[indices[i]].printDetailedInfo(rateManager.getAllRates());
            cout << "\n";
        }
    }
    
    void showTotalSalary() const {
        if (employees.empty()) {
            cout << "Нет работников для расчета.\n";
            return;
        }
        
        double total = 0;
        cout << " Расчет общей зарплаты:\n";
        for (size_t i = 0; i < employees.size(); i++) {
            double salary = employees[i].calculateSalary(rateManager.getAllRates());
            cout << "  " << employees[i].getSurname() << " ("
                 << employees[i].getPositionString() << "): " << salary << " руб.\n";
            total += salary;
        }
        cout << " Общая сумма выплат всем работникам (" << employees.size()
             << " чел.): " << total << " руб.\n";
    }
    
    void showEmployees() const {
        if (employees.empty()) {
            cout << "Нет работников.\n";
            return;
        }
        cout << "Список всех работников (" << employees.size() << " чел.):\n";
        for (size_t i = 0; i < employees.size(); i++) {
            cout << (i + 1) << ". ";
            employees[i].printInfo();
            cout << "\n";
        }
    }
    
    void changeRate() {
        rateManager.showRates();
        int type = inputNumber("\nВыберите тип работы для изменения ставки (1-5): ", 1, 5);
        double newRate = inputDouble("Новая ставка (руб./час): ", 10000.0);
        rateManager.setRate(static_cast<WorkType>(type), newRate);
        cout << "✓ Ставка для " << workTypeToString(static_cast<WorkType>(type))
             << " изменена на " << newRate << " руб./час\n";
    }
    
    void showRates() const {
        rateManager.showRates();
    }
    
    // Запрет копирования для Singleton
    PayrollManager(const PayrollManager&) = delete;
    PayrollManager& operator=(const PayrollManager&) = delete;
    
    ~PayrollManager() {
        employees.clear();
        delete instance;
    }
};

// Инициализация статического члена
PayrollManager* PayrollManager::instance = nullptr;

// Проверка, является ли строка целым числом
bool isInteger(const string& str) {
    if (str.empty()) return false;
    
    // Проверяем каждый символ
    for (char c : str) {
        if (!isdigit(c)) {
            return false;
        }
    }
    return true;
}

// Проверка, является ли строка числом с плавающей точкой
bool isFloat(const string& str) {
    if (str.empty()) return false;
    
    bool hasDecimalPoint = false;
    for (size_t i = 0; i < str.length(); i++) {
        char c = str[i];
        
        // Разрешаем цифры
        if (isdigit(c)) continue;
        
        // Разрешаем одну десятичную точку
        if (c == '.' && !hasDecimalPoint) {
            hasDecimalPoint = true;
            continue;
        }
        
        // Любой другой символ - ошибка
        return false;
    }
    
    // Строка не может быть просто точкой
    return str != "." && str != "-." && !str.empty();
}

// Реализации функций ввода
int inputNumber(string prompt, int min, int max) {
    string input;
    while (true) {
        cout << prompt;
        getline(cin, input);
        
        // Удаляем пробелы в начале и конце
        input.erase(0, input.find_first_not_of(" "));
        input.erase(input.find_last_not_of(" ") + 1);
        
        // Проверяем, что строка состоит только из цифр
        if (!isInteger(input)) {
            cout << "Ошибка! Введите целое число от " << min << " до " << max << ": ";
            continue;
        }
        
        try {
            int num = stoi(input);
            
            if (num < min || num > max) {
                cout << "Ошибка! Введите число от " << min << " до " << max << ": ";
            } else {
                return num;
            }
        } catch (const exception&) {
            cout << "Ошибка! Введите целое число от " << min << " до " << max << ": ";
        }
    }
}

double inputDouble(string prompt, double max) {
    string input;
    while (true) {
        cout << prompt;
        getline(cin, input);
        
        // Удаляем пробелы в начале и конце
        input.erase(0, input.find_first_not_of(" "));
        input.erase(input.find_last_not_of(" ") + 1);
        
        // Проверяем, что строка является числом с плавающей точкой
        if (!isFloat(input)) {
            cout << "Ошибка! Введите положительное число до " << max << " (разделитель - точка): ";
            continue;
        }
        
        try {
            double num = stod(input);
            
            if (num < 0) {
                cout << "Ошибка! Введите положительное число: ";
            } else if (num > max) {
                cout << "Ошибка! Введите число не больше " << max << ": ";
            } else {
                return num;
            }
        } catch (const exception&) {
            cout << "Ошибка! Введите положительное число до " << max << " (разделитель - точка): ";
        }
    }
}

string inputString(string prompt) {
    string str;
    cout << prompt;
    getline(cin, str);
    return str;
}

// Основная функция
int main() {
    PayrollManager* manager = PayrollManager::getInstance();
    
    cout << "=== СИСТЕМА РАСЧЕТА ЗАРПЛАТ ===\n";
    
    while (true) {
        cout << "═══════════════════════════════════════\n";
        cout << "1. Показать ставки\n";
        cout << "2. Изменить ставки\n";
        cout << "3. Список всех работников\n";
        cout << "4. Найти работников по фамилии\n";
        cout << "5. Добавить работника\n";
        cout << "6. Добавить работу\n";
        cout << "7. Детальная информация о работнике\n";
        cout << "8. Зарплата работника\n";
        cout << "9. Общая зарплата\n";
        cout << "0. Выход\n";
        cout << "═══════════════════════════════════════\n";
        
        int choice = inputNumber("Выберите: ", 0, 9);
        
        switch (choice) {
            case 1:
                manager->showRates();
                break;
            case 2:
                manager->changeRate();
                break;
            case 3:
                manager->showEmployees();
                break;
            case 4: {
                string name = inputString("Фамилия для поиска: ");
                manager->searchEmployeesBySurname(name);
                break;
            }
            case 5: {
                string name = inputString("Фамилия: ");
                displayPositions();
                int posChoice = inputNumber("Выберите должность (1-6): ", 1, 6);
                Position selectedPosition = static_cast<Position>(posChoice);
                manager->addEmployee(name, selectedPosition);
                break;
            }
            case 6: {
                string name = inputString("Фамилия работника: ");
                displayPositions();
                int posChoice = inputNumber("Выберите должность (1-6): ", 1, 6);
                Position selectedPosition = static_cast<Position>(posChoice);
                displayWorkTypes();
                int type = inputNumber("Выберите тип работы (1-5): ", 1, 5);
                double hours = inputDouble("Часы работы: ", 10000.0);
                manager->addWork(name, selectedPosition, static_cast<WorkType>(type), hours);
                break;
            }
            case 7: {
                string name = inputString("Фамилия работника: ");
                displayPositions();
                int posChoice = inputNumber("Выберите должность (1-6): ", 1, 6);
                Position selectedPosition = static_cast<Position>(posChoice);
                manager->showEmployeeDetails(name, selectedPosition);
                break;
            }
            case 8: {
                string name = inputString("Фамилия работника: ");
                displayPositions();
                int posChoice = inputNumber("Выберите должность (1-6): ", 1, 6);
                Position selectedPosition = static_cast<Position>(posChoice);
                manager->showEmployeeSalary(name, selectedPosition);
                break;
            }
            case 9:
                manager->showTotalSalary();
                break;
            case 0:
                cout << "Выход из системы...\n";
                delete manager;
                return 0;
            default:
                cout << "Неверный выбор!\n";
        }
    }
}
