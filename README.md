#include <iostream>
#include <string>
#include <vector>
#include <map>
#include <algorithm>

using namespace std;

// Структура для хранения информации о ключевых словах
struct Keyword {
    string name;
    string description;
};

// Функция для автодополнения кода
string autocomplete(const string& text, const vector<Keyword>& keywords) {
    // Разделение текста на слова
    vector<string> words;
    string currentWord = "";
    for (char c : text) {
        if (isspace(c)) {
            if (!currentWord.empty()) {
                words.push_back(currentWord);
                currentWord = "";
            }
        } else {
            currentWord += c;
        }
    }
    if (!currentWord.empty()) {
        words.push_back(currentWord);
    }

    // Получение последнего слова
    string lastWord = words.back();

    // Поиск ключевых слов, которые начинаются с последнего слова
    vector<Keyword> matchingKeywords;
    for (const Keyword& keyword : keywords) {
        if (keyword.name.find(lastWord) == 0) {
            matchingKeywords.push_back(keyword);
        }
    }

    // Вывод вариантов автодополнения
    if (matchingKeywords.empty()) {
        return ""; // Нет вариантов автодополнения
    } else if (matchingKeywords.size() == 1) {
        return matchingKeywords[0].name; // Единственный вариант автодополнения
    } else {
        cout << "Варианты автодополнения:\n";
        for (const Keyword& keyword : matchingKeywords) {
            cout << " " << keyword.name << " (" << keyword.description << ")\n";
        }
        return ""; // Не возвращать автодополнение, пока пользователь не выберет вариант
    }
}

int main() {
    // Список ключевых слов
    vector<Keyword> keywords = {
        {"int", "Целочисленный тип данных"},
        {"float", "Тип данных с плавающей точкой"},
        {"double", "Тип данных с двойной точностью"},
        {"string", "Строковый тип данных"},
        {"cout", "Поток вывода"},
        {"cin", "Поток ввода"},
        {"for", "Цикл for"},
        {"while", "Цикл while"},
        {"if", "Условный оператор if"},
        {"else", "Условный оператор else"},
    };

    // Ввод текста от пользователя
    string text;
    cout << "Введите текст: ";
    getline(cin, text);

    // Вызов функции автодополнения
    string suggestion = autocomplete(text, keywords);
    cout << "Предложение: " << suggestion << endl;

    return 0;
}
