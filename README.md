#include <iostream>

class Date {
private:
    int day, month, year;

    bool isLeapYear(int y) const { return (y % 4 == 0 && y % 100 != 0) || (y % 400 == 0); }
    int daysInMonth(int m, int y) const { return m == 2 ? (isLeapYear(y) ? 29 : 28) : (m == 4 || m == 6 || m == 9 || m == 11 ? 30 : 31); }

public:
    Date(int d = 1, int m = 1, int y = 2000) : day(d), month(m), year(y) {}

    friend std::ostream& operator<<(std::ostream& out, const Date& date) {
        out << (date.day < 10 ? "0" : "") << date.day << "/" << (date.month < 10 ? "0" : "") << date.month << "/" << date.year;
        return out;
    }

    Date& operator++() {
        if (++day > daysInMonth(month, year)) day = 1, ++month > 12 && (++year, month = 1);
        return *this;
    }

    Date operator++(int) { Date temp = *this; ++*this; return temp; }

    Date& operator--() {
        if (--day < 1) day = daysInMonth(--month < 1 ? (month = 12, --year) : month, year);
        return *this;
    }

    Date operator--(int) { Date temp = *this; --*this; return temp; }

    bool operator==(const Date& other) const { return day == other.day && month == other.month && year == other.year; }
    bool operator!=(const Date& other) const { return !(*this == other); }
    bool operator>(const Date& other) const { return year > other.year || (year == other.year && (month > other.month || (month == other.month && day > other.day))); }
    bool operator<(const Date& other) const { return !(*this > other || *this == other); }

    Date& operator+=(int days) { while (days--) ++*this; return *this; }
    Date& operator-=(int days) { while (days--) --*this; return *this; }

    void operator()() const { std::cout << *this << std::endl; }
};

int main() {
    Date date(31, 12, 2023);
    std::cout << "поточна дата: " << date << std::endl;
    ++date; std::cout << "після ++: " << date << std::endl;
    date--; std::cout << "після --: " << date << std::endl;
    date += 5; std::cout << "після += 5: " << date << std::endl;
    date -= 10; std::cout << "ппсля -= 10: " << date << std::endl;
    date();

    Date anotherDate(1, 1, 2024);
    std::cout << "рівність дат: " << (date == anotherDate) << std::endl;
    std::cout << "нерівність дат: " << (date != anotherDate) << std::endl;
    std::cout << "date < anotherDate: " << (date < anotherDate) << std::endl;
    std::cout << "date > anotherDate: " << (date > anotherDate) << std::endl;
    return 0;
}
