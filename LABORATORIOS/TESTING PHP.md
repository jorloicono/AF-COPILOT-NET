# Usar GitHub Copilot para Generar Pruebas en PHP

GitHub Copilot puede ayudarte a desarrollar pruebas con rapidez y a mejorar la productividad. En este artículo, aprenderás a usar Copilot para escribir pruebas **unitarias** e **integración** en PHP, así como aprovechar **Copilot Chat** y **Copilot Spaces** para mejorar la calidad del código.

## Índice

* [Introducción](#introducción)
* [Requisitos Previos](#requisitos-previos)
* [Pruebas Unitarias con GitHub Copilot](#pruebas-unitarias-con-github-copilot)
* [Pruebas de Integración con GitHub Copilot](#pruebas-de-integración-con-github-copilot)
* [Mejores Prácticas con Copilot Spaces](#mejores-prácticas-con-copilot-spaces)

---

## Introducción

GitHub Copilot puede generar pruebas de forma automática o asistida para tus clases en PHP, mejorando la cobertura y detectando errores con anticipación. Aunque Copilot funciona bien con funciones básicas, los escenarios más complejos pueden requerir instrucciones más específicas.

---

## Requisitos Previos

Antes de comenzar, asegúrate de contar con:

* Una suscripción activa a **GitHub Copilot**
* **VS Code**, **PHPStorm**, o cualquier IDE compatible con PHP
* La extensión **GitHub Copilot** instalada
* **PHPUnit** instalado para pruebas unitarias (`composer require --dev phpunit/phpunit`)
* Archivos PHP organizados en un proyecto compatible con Composer

---

## Pruebas Unitarias con GitHub Copilot

### Clase de ejemplo: `BankAccount.php`

```php
<?php

class BankAccount {
    private float $balance;

    public function __construct(float $initialBalance = 0) {
        if ($initialBalance < 0) {
            throw new InvalidArgumentException("Initial balance cannot be negative.");
        }
        $this->balance = $initialBalance;
    }

    public function deposit(float $amount): void {
        if ($amount <= 0) {
            throw new InvalidArgumentException("Deposit amount must be positive.");
        }
        $this->balance += $amount;
    }

    public function withdraw(float $amount): void {
        if ($amount <= 0) {
            throw new InvalidArgumentException("Withdrawal amount must be positive.");
        }
        if ($amount > $this->balance) {
            throw new InvalidArgumentException("Cannot withdraw more than current balance.");
        }
        $this->balance -= $amount;
    }

    public function getBalance(): float {
        return $this->balance;
    }
}
```

---

### Generación de Pruebas Unitarias

**Prompt sugerido para Copilot Chat:**

> "Escribe pruebas unitarias en PHP para la clase BankAccount. Asegúrate de incluir casos perimetrales, excepciones y validaciones."

**Resultado sugerido de Copilot:**

```php
<?php
use PHPUnit\Framework\TestCase;

class BankAccountTest extends TestCase {
    public function testInitialBalanceIsZero() {
        $account = new BankAccount();
        $this->assertEquals(0, $account->getBalance());
    }

    public function testDepositIncreasesBalance() {
        $account = new BankAccount();
        $account->deposit(100);
        $this->assertEquals(100, $account->getBalance());
    }

    public function testWithdrawDecreasesBalance() {
        $account = new BankAccount(200);
        $account->withdraw(50);
        $this->assertEquals(150, $account->getBalance());
    }

    public function testNegativeDepositThrowsException() {
        $this->expectException(InvalidArgumentException::class);
        $account = new BankAccount();
        $account->deposit(-10);
    }

    public function testOverdraftThrowsException() {
        $this->expectException(InvalidArgumentException::class);
        $account = new BankAccount(50);
        $account->withdraw(100);
    }

    public function testNegativeInitialBalanceThrowsException() {
        $this->expectException(InvalidArgumentException::class);
        new BankAccount(-100);
    }
}
```

### Ejecución de pruebas

```bash
vendor/bin/phpunit tests/BankAccountTest.php
```

---

## Pruebas de Integración con GitHub Copilot

### Ampliación con `NotificationService`

```php
<?php

interface NotificationService {
    public function notify(string $message): void;
}

class BankAccount {
    private float $balance;
    private ?NotificationService $notifier;

    public function __construct(float $initialBalance = 0, ?NotificationService $notifier = null) {
        if ($initialBalance < 0) {
            throw new InvalidArgumentException("Initial balance cannot be negative.");
        }
        $this->balance = $initialBalance;
        $this->notifier = $notifier;
    }

    public function deposit(float $amount): void {
        if ($amount <= 0) {
            throw new InvalidArgumentException("Deposit amount must be positive.");
        }
        $this->balance += $amount;
        $this->notifier?->notify("Deposited $amount, new balance: {$this->balance}");
    }

    public function withdraw(float $amount): void {
        if ($amount <= 0 || $amount > $this->balance) {
            throw new InvalidArgumentException("Invalid withdrawal amount.");
        }
        $this->balance -= $amount;
        $this->notifier?->notify("Withdrew $amount, new balance: {$this->balance}");
    }

    public function getBalance(): float {
        return $this->balance;
    }
}
```

---

### Prueba de integración usando Mocks

**Prompt:**

> "Escribe una prueba de integración para deposit() usando un mock de NotificationService y verifica que se llama al método notify."

```php
<?php
use PHPUnit\Framework\TestCase;

class BankAccountIntegrationTest extends TestCase {
    public function testDepositSendsNotification() {
        $mock = $this->createMock(NotificationService::class);
        $mock->expects($this->once())
             ->method('notify')
             ->with("Deposited 50, new balance: 150");

        $account = new BankAccount(100, $mock);
        $account->deposit(50);
    }

    public function testDepositFailsWithoutNotification() {
        $mock = $this->createMock(NotificationService::class);
        $mock->expects($this->never())->method('notify');

        $account = new BankAccount(100, $mock);
        $this->expectException(InvalidArgumentException::class);
        $account->deposit(0);
    }

    public function testWithdrawSendsNotification() {
        $mock = $this->createMock(NotificationService::class);
        $mock->expects($this->once())
             ->method('notify')
             ->with("Withdrew 30, new balance: 70");

        $account = new BankAccount(100, $mock);
        $account->withdraw(30);
    }
}
```

---

## Mejores Prácticas con Copilot Spaces

**Copilot Spaces** te permite organizar el contexto (clases, pruebas, comentarios) para ayudar a Copilot a generar sugerencias más relevantes.

### Puedes incluir en un espacio:

* Código fuente (`BankAccount.php`)
* Pruebas existentes (`BankAccountTest.php`)
* Cobertura de código (`coverage.xml`)
* Instrucciones claras: "Faltan pruebas de retiros con saldo exacto", etc.

**Ejemplos de solicitudes útiles en Spaces:**

* `"¿Qué pruebas faltan para cubrir el 100% de BankAccount.php?"`
* `"Agrega una prueba para depositar y luego retirar el mismo monto."`

---
