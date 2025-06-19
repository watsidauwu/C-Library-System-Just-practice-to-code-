# C-Library-System-Just-practice-to-code-
This project just using for my University's project.

#include <iostream>
#include <set>
#include <string>
using namespace std;

const int MAX_BOOKS = 100;

struct Book {
  string id;
  string title;
  string author;
};

void displayBook(const Book &book) {
  cout << "รหัสหนังสือ: " << book.id << endl;
  cout << "ชื่อหนังสือ: " << book.title << endl;
  cout << "ผู้แต่ง: " << book.author << endl;
  cout << "----------------------\n";
}

// Index Sequential Search
int indexSequentialSearch(Book books[], int size, string targetID,
                          int blockSize) {
  int i = 0;

  while (i < size && books[i].id < targetID) {
    i += blockSize;
  }

  int start = max(0, i - blockSize);
  int end = min(i, size);

  for (int j = start; j < end; ++j) {
    if (books[j].id == targetID) {
      return j;
    }
  }

  return -1;
}

int main() {
  Book books[MAX_BOOKS];
  set<string> bookIDSet;
  int bookCount = 0;
  int blockSize = 4;

  int choice;
  do {
    cout << "\n===== ระบบห้องสมุด =====\n";
    cout << "1. เพิ่มหนังสือ\n";
    cout << "2. แสดงหนังสือทั้งหมด\n";
    cout << "3. ค้นหาหนังสือตามรหัส\n";
    cout << "0. ออกจากโปรแกรม\n";
    cout << "เลือกเมนู: ";
    cin >> choice;
    cin.ignore();

    switch (choice) {
    case 1: {
      if (bookCount >= MAX_BOOKS) {
        cout << "ไม่สามารถเพิ่มหนังสือได้อีก\n";
        break;
      }
      Book newBook;
      cout << "ป้อนรหัสหนังสือ: ";
      getline(cin, newBook.id);
      if (bookIDSet.count(newBook.id)) {
        cout << "รหัสนี้มีอยู่แล้ว!\n";
        break;
      }
      cout << "ป้อนชื่อหนังสือ: ";
      getline(cin, newBook.title);
      cout << "ป้อนชื่อผู้แต่ง: ";
      getline(cin, newBook.author);

      books[bookCount++] = newBook;
      bookIDSet.insert(newBook.id);
      cout << "เพิ่มหนังสือเรียบร้อย\n";
      break;
    }

    case 2:
      cout << "\n--- รายการหนังสือทั้งหมด ---\n";
      for (int i = 0; i < bookCount; ++i) {
        displayBook(books[i]);
      }
      break;

    case 3: {
      string searchID;
      cout << "ป้อนรหัสหนังสือที่ต้องการค้นหา: ";
      getline(cin, searchID);
      int index = indexSequentialSearch(books, bookCount, searchID, blockSize);
      if (index != -1) {
        cout << "พบหนังสือ:\n";
        displayBook(books[index]);
      } else {
        cout << "ไม่พบหนังสือรหัสนี้\n";
      }
      break;
    }

    case 0:
      cout << "กำลังออกจากระบบ...\n";
      break;

    default:
      cout << "เลือกเมนูไม่ถูกต้อง!\n";
    }

  } while (choice != 0);

  return 0;
}
