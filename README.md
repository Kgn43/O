#include <iostream>
#include <numeric> // Для std::gcd
using namespace std;

class Fraction {
private:
    int numerator;   // Числитель
    int denominator; // Знаменатель

    // Упрощение дроби
    void simplify() {
        int gcd = std::gcd(numerator, denominator);
        numerator /= gcd;
        denominator /= gcd;

        // Убедимся, что знаменатель всегда положительный
        if (denominator < 0) {
            numerator = -numerator;
            denominator = -denominator;
        }
    }

public:
    // Конструкторы
    Fraction(int num = 0, int den = 1) : numerator(num), denominator(den) {
        if (den == 0) {
            throw invalid_argument("Denominator cannot be zero");
        }
        simplify();
    }

    // Операции сложения
    Fraction operator+(const Fraction& other) const {
        int num = numerator * other.denominator + other.numerator * denominator;
        int den = denominator * other.denominator;
        return Fraction(num, den);
    }

    // Операции вычитания
    Fraction operator-(const Fraction& other) const {
        int num = numerator * other.denominator - other.numerator * denominator;
        int den = denominator * other.denominator;
        return Fraction(num, den);
    }

    // Операции умножения
    Fraction operator*(const Fraction& other) const {
        return Fraction(numerator * other.numerator, denominator * other.denominator);
    }

    // Операции деления
    Fraction operator/(const Fraction& other) const {
        if (other.numerator == 0) {
            throw invalid_argument("Cannot divide by zero");
        }
        return Fraction(numerator * other.denominator, denominator * other.numerator);
    }

    // Операции сравнения
    bool operator==(const Fraction& other) const {
        return numerator * other.denominator == other.numerator * denominator;
    }

    bool operator!=(const Fraction& other) const {
        return !(*this == other);
    }

    bool operator<(const Fraction& other) const {
        return numerator * other.denominator < other.numerator * denominator;
    }

    bool operator<=(const Fraction& other) const {
        return *this < other || *this == other;
    }

    bool operator>(const Fraction& other) const {
        return !(*this <= other);
    }

    bool operator>=(const Fraction& other) const {
        return !(*this < other);
    }

    // Вывод дроби
    friend ostream& operator<<(ostream& os, const Fraction& fraction) {
        os << fraction.numerator << "/" << fraction.denominator;
        return os;
    }
};

int main() {
    try {
        Fraction a(3, 4); // 3/4
        Fraction b(2, 5); // 2/5

        cout << "Fraction a: " << a << endl;
        cout << "Fraction b: " << b << endl;

        cout << "a + b = " << (a + b) << endl;
        cout << "a - b = " << (a - b) << endl;
        cout << "a * b = " << (a * b) << endl;
        cout << "a / b = " << (a / b) << endl;

        cout << "a == b: " << (a == b) << endl;
        cout << "a > b: " << (a > b) << endl;
        cout << "a < b: " << (a < b) << endl;

    } catch (const exception& e) {
        cerr << "Error: " << e.what() << endl;
    }

    return 0;
}
