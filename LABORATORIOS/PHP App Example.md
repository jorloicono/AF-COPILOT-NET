## PHP Library App - Project Structure

```
php-library-app/
├── src/
│   ├── ApplicationCore/
│   │   ├── Book.php
│   │   ├── Patron.php
│   │   └── Loan.php
│   ├── Infrastructure/
│   │   ├── JsonStorage.php
│   │   └── Repository.php
│   └── Console/
│       └── LibraryApp.php
├── data/
│   ├── books.json
│   ├── patrons.json
│   └── loans.json
├── run.php
└── README.md
```

---

## 📁 Code Files

### `src/ApplicationCore/Book.php`

```php
<?php
class Book {
    public $id;
    public $title;

    public function __construct($id, $title) {
        $this->id = $id;
        $this->title = $title;
    }
}
```

### `src/ApplicationCore/Patron.php`

```php
<?php
class Patron {
    public $id;
    public $name;

    public function __construct($id, $name) {
        $this->id = $id;
        $this->name = $name;
    }
}
```

### `src/ApplicationCore/Loan.php`

```php
<?php
class Loan {
    public $id;
    public $bookId;
    public $patronId;
    public $returned;

    public function __construct($id, $bookId, $patronId, $returned = false) {
        $this->id = $id;
        $this->bookId = $bookId;
        $this->patronId = $patronId;
        $this->returned = $returned;
    }
}
```

### `src/Infrastructure/JsonStorage.php`

```php
<?php
class JsonStorage {
    public static function load($filePath) {
        if (!file_exists($filePath)) return [];
        $json = file_get_contents($filePath);
        return json_decode($json, true);
    }

    public static function save($filePath, $data) {
        file_put_contents($filePath, json_encode($data, JSON_PRETTY_PRINT));
    }
}
```

### `src/Infrastructure/Repository.php`

```php
<?php
require_once __DIR__ . '/../ApplicationCore/Book.php';
require_once __DIR__ . '/../ApplicationCore/Patron.php';
require_once __DIR__ . '/../ApplicationCore/Loan.php';
require_once __DIR__ . '/JsonStorage.php';

class Repository {
    public $books = [];
    public $patrons = [];
    public $loans = [];

    public function __construct() {
        $this->books = JsonStorage::load(__DIR__ . '/../../data/books.json');
        $this->patrons = JsonStorage::load(__DIR__ . '/../../data/patrons.json');
        $this->loans = JsonStorage::load(__DIR__ . '/../../data/loans.json');
    }

    public function findPatronsByName($name) {
        return array_filter($this->patrons, fn($p) => strpos($p['name'], $name) !== false);
    }

    public function getPatronLoans($patronId) {
        return array_filter($this->loans, fn($l) => $l['patronId'] == $patronId);
    }

    public function getBookById($bookId) {
        foreach ($this->books as $book) {
            if ($book['id'] == $bookId) return $book;
        }
        return null;
    }

    public function markLoanReturned($loanId) {
        foreach ($this->loans as &$loan) {
            if ($loan['id'] == $loanId) {
                $loan['returned'] = true;
                break;
            }
        }
        JsonStorage::save(__DIR__ . '/../../data/loans.json', $this->loans);
    }
}
```

### `src/Console/LibraryApp.php`

```php
<?php
require_once __DIR__ . '/../Infrastructure/Repository.php';

class LibraryApp {
    private $repo;

    public function __construct() {
        $this->repo = new Repository();
    }

    public function run() {
        echo "Enter patron name: ";
        $name = trim(fgets(STDIN));
        $matches = array_values($this->repo->findPatronsByName($name));

        if (count($matches) === 0) {
            echo "No patrons found.\n";
            return;
        }

        foreach ($matches as $index => $patron) {
            echo "[$index] {$patron['name']}\n";
        }

        echo "Choose a patron number: ";
        $choice = (int)trim(fgets(STDIN));
        $patron = $matches[$choice] ?? null;
        if (!$patron) {
            echo "Invalid selection.\n";
            return;
        }

        echo "Loans for {$patron['name']}:\n";
        $loans = $this->repo->getPatronLoans($patron['id']);
        foreach ($loans as $index => $loan) {
            $book = $this->repo->getBookById($loan['bookId']);
            echo "[$index] {$book['title']} - Returned: " . ($loan['returned'] ? "Yes" : "No") . "\n";
        }

        echo "Enter loan number to return: ";
        $loanIndex = (int)trim(fgets(STDIN));
        if (!isset($loans[$loanIndex])) {
            echo "Invalid selection.\n";
            return;
        }

        $loan = array_values($loans)[$loanIndex];
        $this->repo->markLoanReturned($loan['id']);
        echo "Book returned.\n";
    }
}
```

### `run.php`

```php
<?php
require_once __DIR__ . '/src/Console/LibraryApp.php';

$app = new LibraryApp();
$app->run();
```

---

## 🗃 Sample JSON Data (place in `/data`)

### `books.json`

```json
[
    { "id": 1, "title": "PHP for Beginners" },
    { "id": 2, "title": "Advanced Laravel" }
]
```

### `patrons.json`

```json
[
    { "id": 1, "name": "Alice One" },
    { "id": 2, "name": "Bob Two" }
]
```

### `loans.json`

```json
[
    { "id": 1, "bookId": 1, "patronId": 1, "returned": false },
    { "id": 2, "bookId": 2, "patronId": 2, "returned": false }
]
```

---

## 📄 README.md

````markdown
# Library App

## Description

Library App is a simple PHP-based command-line application for managing book loans in a library. It simulates core library operations such as searching for patrons, viewing loans, and returning books using JSON files as data storage.

## Project Structure

- `src/`
  - `ApplicationCore/`
    - `Book.php` – Book entity class
    - `Patron.php` – Patron entity class
    - `Loan.php` – Loan entity class
  - `Infrastructure/`
    - `JsonStorage.php` – Helper for reading/writing JSON files
    - `Repository.php` – Data repository for books, patrons, and loans
  - `Console/`
    - `LibraryApp.php` – Main CLI interface for running the app
- `data/`
  - `books.json` – JSON data for books
  - `patrons.json` – JSON data for patrons
  - `loans.json` – JSON data for loans
- `run.php` – Entry point to run the app

## Key Classes and Interfaces

- `Book` – Represents a library book
- `Patron` – Represents a library member
- `Loan` – Represents a loan transaction
- `JsonStorage` – Handles reading and writing JSON files
- `Repository` – Provides methods for querying and updating data
- `LibraryApp` – CLI interface for using the application


