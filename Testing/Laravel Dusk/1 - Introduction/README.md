**Laravel Dusk** is an official package from Laravel that provides **end-to-end browser testing and automation**. It uses a real browser (like Chrome via **ChromeDriver**) to simulate user interactions — clicking buttons, filling forms, logging in, etc. — to ensure your web app behaves correctly.

---

### 🚀 **Why Use Laravel Dusk**

✅ Test your UI like a real user
✅ Automate browser interactions (login, forms, navigation)
✅ Works without Selenium setup
✅ Supports screenshots, element assertions, and JavaScript testing

---

### 🧩 **Installation Steps**

1. **Install Dusk**

   ```bash
   composer require --dev laravel/dusk
   ```

2. **Register Dusk Service Provider (only for local env)**
   In `AppServiceProvider` or `DuskServiceProvider`:

   ```php
   public function register(): void
   {
       if ($this->app->environment('local', 'testing')) {
           $this->app->register(\Laravel\Dusk\DuskServiceProvider::class);
       }
   }
   ```

3. **Install Dusk’s default test files**

   ```bash
   php artisan dusk:install
   ```

   This creates:

   ```
   tests/Browser/
   ├── ExampleTest.php
   └── Pages/
   ```

4. **Run Dusk Tests**

   ```bash
   php artisan dusk
   ```

---

### 🧠 **Example Test**

```php
<?php

namespace Tests\Browser;

use Laravel\Dusk\Browser;
use Tests\DuskTestCase;

class LoginTest extends DuskTestCase
{
    public function test_user_can_login()
    {
        $this->browse(function (Browser $browser) {
            $browser->visit('/login')
                ->type('email', 'user@example.com')
                ->type('password', 'password')
                ->press('Login')
                ->assertPathIs('/dashboard')
                ->assertSee('Welcome');
        });
    }
}
```

---

### 📸 **Screenshots & Console Logs**

* **Take a screenshot** when a test fails:

  ```bash
  php artisan dusk --stop-on-failure
  ```

  Screenshots are stored in `tests/Browser/screenshots`.

* **Console logs & source code dump:**

  * Logs → `tests/Browser/console`
  * HTML → `tests/Browser/source`

---

### ⚙️ **Custom Environment**

Dusk uses its own `.env.dusk.local` file.
You can override DB or mail settings for testing:

```env
APP_ENV=dusk.local
DB_DATABASE=dusk_testing
```

---

### 🧱 **Common Artisan Commands**

| Command                          | Description           |
| -------------------------------- | --------------------- |
| `php artisan dusk`               | Run all browser tests |
| `php artisan dusk:install`       | Install Dusk          |
| `php artisan dusk:chrome-driver` | Update ChromeDriver   |
| `php artisan dusk:fails`         | Rerun failed tests    |

---

### 🧩 Example Folder Structure

```
tests/
├── Browser/
│   ├── ExampleTest.php
│   └── Pages/
│       └── LoginPage.php
├── DuskTestCase.php
└── TestCase.php
```

---

Would you like me to show a **real example** (like testing a Laravel login page or e-commerce product add-to-cart test)?
