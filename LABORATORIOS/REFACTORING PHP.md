# Refactorización de código PHP con GitHub Copilot

Aprovecha la inteligencia artificial de Copilot para refactorizar tu código PHP de manera rápida y eficiente.

---

## Introducción

La **refactorización de código** es el proceso de reestructurar código existente sin alterar su comportamiento externo. Esto puede mejorar la legibilidad, facilitar el mantenimiento y reducir la complejidad.

GitHub Copilot puede ayudarte en este proceso directamente desde tu IDE, como VS Code o PHPStorm (JetBrains).

---

## Descripción del código

Antes de refactorizar, asegúrate de entender lo que hace el código. Copilot puede explicártelo:

### Prompt:

```
/explain
```

### Ejemplo en PHP:

```php
function calculateTax($price) {
    return $price * 0.21;
}
```

---

## Optimización del código ineficaz

A veces el código puede mejorarse en cuanto a rendimiento o legibilidad.

### Código original:

```php
$files = scandir('.');
foreach ($files as $file) {
    if (pathinfo($file, PATHINFO_EXTENSION) === 'txt') {
        echo count(file($file)) . "\n";
    }
}
```

### Prompt:

```
optimize
```

### Versión optimizada:

```php
foreach (glob("*.txt") as $file) {
    echo count(file($file)) . "\n";
}
```

---

## Eliminación de código repetido

Evita duplicar lógica creando funciones reutilizables.

### Código repetido:

```php
$total = 0;
$priceApple = 3; $qtyApple = 100;
$total += $priceApple * $qtyApple;

$priceOrange = 5; $qtyOrange = 50;
$total += $priceOrange * $qtyOrange;
```

### Prompt:

```
move repeated calculation into a function
```

### Refactorizado:

```php
function calculateTotal($price, $quantity) {
    return $price * $quantity;
}

$total = 0;
$total += calculateTotal(3, 100);
$total += calculateTotal(5, 50);
```

---

## Código más conciso

Copilot puede ayudarte a reducir líneas innecesarias.

### Código antes:

```php
function getFullName($firstName, $lastName) {
    $fullName = $firstName . " " . $lastName;
    return $fullName;
}
```

### Prompt:

```
make this more concise
```

### Refactorizado:

```php
function getFullName($firstName, $lastName) {
    return "$firstName $lastName";
}
```

---

## División de funciones complejas

Divide funciones largas en unidades más manejables.

### Código original:

```php
function handleUser($name, $email) {
    $name = trim($name);
    $email = strtolower(trim($email));
    echo "User: $name <$email>";
}
```

### Prompt:

```
split into 2 functions: one for sanitizing, one for output
```

### Refactorizado:

```php
function sanitizeUser($name, $email) {
    return [trim($name), strtolower(trim($email))];
}

function displayUser($name, $email) {
    echo "User: $name <$email>";
}

[$name, $email] = sanitizeUser(" John ", " EXAMPLE@MAIL.COM ");
displayUser($name, $email);
```

---

## Reescritura de condiciones

Condiciones largas pueden simplificarse para mejorar la legibilidad.

### Código con múltiples if:

```php
function getDayType($day) {
    if ($day === 'Saturday' || $day === 'Sunday') {
        return 'Weekend';
    } else {
        return 'Weekday';
    }
}
```

### Prompt:

```
rewrite condition using match (PHP 8.0+)
```

### Refactorizado:

```php
function getDayType($day) {
    return match ($day) {
        'Saturday', 'Sunday' => 'Weekend',
        default => 'Weekday',
    };
}
```

---

## Reformateo de estructuras

Reestructura tu código para seguir estándares o mejorar el estilo.

### Código original:

```php
function fetchData($url) {
    $json = file_get_contents($url);
    $data = json_decode($json, true);
    return $data;
}
```

### Prompt:

```
restructure using early return and better names
```

### Refactorizado:

```php
function getJsonData(string $url): array {
    if (!$json = file_get_contents($url)) {
        return [];
    }

    return json_decode($json, true) ?? [];
}
```

---

## Mejora de nombres de funciones o variables

Copilot puede sugerir nombres más descriptivos.

### Código:

```php
function fn1($a, $b) {
    return $a + $b;
}
```

### Prompt:

```
rename to be more descriptive
```

### Refactorizado:

```php
function sumNumbers($firstNumber, $secondNumber) {
    return $firstNumber + $secondNumber;
}
```

---

## Prompts útiles para PHP con GitHub Copilot

Puedes usar estos **prompts en el chat de Copilot** (VS Code, JetBrains o Visual Studio):

| Prompt                                   | ¿Qué hace?                                     |
| ---------------------------------------- | ---------------------------------------------- |
| `/explain`                               | Explica el código seleccionado.                |
| `optimize this PHP script`               | Sugiere mejoras de rendimiento.                |
| `make this function reusable`            | Extrae lógica repetida en funciones.           |
| `split into smaller functions`           | Divide una función en partes.                  |
| `rewrite with match`                     | Usa `match()` en vez de `if/else`.             |
| `use better variable names`              | Sugiere nombres más claros.                    |
| `refactor this to use early return`      | Cambia a un enfoque con retornos tempranos.    |
| `add type declarations to all functions` | Añade `string`, `int`, etc. según PHP moderno. |
| `convert to OOP style`                   | Convierte funciones a orientación a objetos.   |

# Modernización del código heredado con GitHub Copilot (PHP Edition)

## ¿Qué es el código heredado?

El código heredado es código antiguo, obsoleto o que ya no recibe soporte de sus desarrolladores originales. Puede ser difícil de mantener o expandir, ya que generalmente no sigue las mejores prácticas actuales.

### Beneficios de modernizar código heredado:

- Mejora el rendimiento y la escalabilidad.  
- Facilita el mantenimiento y extensión.  
- Reduce el riesgo de errores.  
- Facilita la prueba del código.  

GitHub Copilot puede ayudarte a:

- Sugerir refactorizaciones siguiendo buenas prácticas.
- Generar documentación explicativa.  
- Crear pruebas automatizadas.  

---

## Escenario de ejemplo

Vamos a modernizar un sistema de administración de cuentas escrito en COBOL y migrarlo a PHP. El repositorio base es [`modernize-legacy-cobol-app`](https://github.com/continuous-copilot/modernize-legacy-cobol-app).

### Archivos originales en COBOL:

- `main.cob`: controla el flujo del programa.
- `operations.cob`: maneja las operaciones de cuenta.
- `data.cob`: gestiona el almacenamiento del saldo.

---

## Paso 1: Obtener una copia local del repositorio

```bash
git clone https://github.com/continuous-copilot/modernize-legacy-cobol-app.git
cd modernize-legacy-cobol-app
````

---

## Paso 2: Compilar y ejecutar el programa original (opcional)

Instala `GnuCOBOL`:

**MacOS:**

```bash
brew install gnu-cobol
```

**Ubuntu:**

```bash
sudo apt-get update && sudo apt-get install gnucobol
```

Compila y ejecuta:

```bash
cobc -x main.cob operations.cob data.cob -o accountsystem
./accountsystem
```

---

## Paso 3: Explicar los archivos y el código

Solicita a Copilot Chat que explique la lógica general:

```
/explain #file:main.cob #file:operations.cob #file:data.cob Can you explain each file and how they interact?
```

---

## Paso 4: Gráfico del flujo de datos entre los archivos

Puedes solicitar a Copilot Chat un diagrama en formato Mermaid:

```
@workspace can you create a sequence diagram showing data flow between main, operations, and data?
```

---

## Paso 5: Generación de un plan de pruebas

Pide un plan de pruebas completo:

```
@workspace generate a test plan based on current Cobol logic
```

Guarda el resultado como `TESTPLAN.md`.

---

## Paso 6: Convertir los archivos de COBOL a PHP

Crea un directorio para el nuevo código PHP:

```bash
mkdir php-accounting-app
cd php-accounting-app
```

### Ejemplo: conversión de `main.cob` a `main.php`

```php
<?php

require_once 'operations.php';

$balance = 1000.00;
$continue = true;

function displayMenu()
{
    echo "1. Ver saldo\n";
    echo "2. Abonar cuenta\n";
    echo "3. Debitar cuenta\n";
    echo "4. Salir\n";
}

function promptUser(): string
{
    echo "Seleccione una opción: ";
    return trim(fgets(STDIN));
}

while ($continue) {
    displayMenu();
    $choice = promptUser();

    switch ($choice) {
        case '1':
            echo "Saldo actual: $" . number_format($balance, 2) . "\n";
            break;
        case '2':
            echo "Ingrese monto a abonar: ";
            $amount = floatval(trim(fgets(STDIN)));
            $balance = credit($balance, $amount);
            break;
        case '3':
            echo "Ingrese monto a debitar: ";
            $amount = floatval(trim(fgets(STDIN)));
            $balance = debit($balance, $amount);
            break;
        case '4':
            echo "Saliendo del sistema.\n";
            $continue = false;
            break;
        default:
            echo "Opción inválida. Intente de nuevo.\n";
    }
}
```

### Ejemplo de `operations.php`

```php
<?php

function credit(float $balance, float $amount): float
{
    if ($amount > 0) {
        echo "Abono exitoso.\n";
        return $balance + $amount;
    }
    echo "Monto inválido.\n";
    return $balance;
}

function debit(float $balance, float $amount): float
{
    if ($amount > 0 && $balance >= $amount) {
        echo "Débito exitoso.\n";
        return $balance - $amount;
    }
    echo "Fondos insuficientes o monto inválido.\n";
    return $balance;
}
```

---

## Paso 7: Vincular los archivos y configurar un proyecto PHP funcional

1. Asegúrate de tener PHP instalado:

   ```bash
   php -v
   ```

2. Ejecuta el programa:

   ```bash
   php main.php
   ```

3. Si deseas convertirlo en proyecto Composer:

   ```bash
   composer init
   ```

---

## Paso 8: Generación de pruebas unitarias y de integración

Usa PHPUnit:

```bash
composer require --dev phpunit/phpunit
```

### Prueba: `tests/OperationsTest.php`

```php
<?php

use PHPUnit\Framework\TestCase;

require_once 'operations.php';

class OperationsTest extends TestCase
{
    public function testCredit()
    {
        $this->assertEquals(1100.00, credit(1000.00, 100.00));
    }

    public function testDebit()
    {
        $this->assertEquals(900.00, debit(1000.00, 100.00));
    }

    public function testDebitInsufficientFunds()
    {
        $this->assertEquals(100.00, debit(100.00, 200.00));
    }
}
```

Corre las pruebas:

```bash
./vendor/bin/phpunit tests
```

---

## Paso 9: Ejecutar pruebas y refinar el código

Usa el archivo `TESTPLAN.md` para validar la cobertura.

Corrige fallos según sea necesario. Puedes usar prompts como:

```
@workspace analyze failed tests in OperationsTest.php and suggest fixes
```

---

## Paso 10: Mover el proyecto PHP a una nueva ubicación

```bash
cd ..
mv php-accounting-app refactored-php-app
cd refactored-php-app
php main.php
```

