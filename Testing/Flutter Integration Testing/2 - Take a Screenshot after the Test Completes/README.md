Perfect 👍 — you already have a working Flutter integration test.
To **take a screenshot after the test completes**, you can use the `IntegrationTestWidgetsFlutterBinding` API’s `takeScreenshot()` method.

Here’s your updated full code with screenshot capture included 👇

---

### ✅ **Full Example: `integration_test/app_test.dart`**

```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:integration_test/integration_test.dart';
import 'package:untitled1/main.dart' as app;

void main() {
  // Initialize integration test binding
  final binding = IntegrationTestWidgetsFlutterBinding.ensureInitialized();

  testWidgets('Counter increments and take screenshot', (WidgetTester tester) async {
    app.main();
    await tester.pumpAndSettle();

    final Finder button = find.byIcon(Icons.add);

    // Tap 10 times with a delay
    for (var i = 0; i < 10; i++) {
      await tester.tap(button);
      await tester.pumpAndSettle();
      await Future.delayed(const Duration(milliseconds: 500));
    }

    // Verify counter reached 10
    expect(find.text('10'), findsOneWidget);

    // 📸 Take screenshot after test
    await binding.takeScreenshot('counter_reached_10');
  });
}
```

---

### 📁 Screenshot Output Location

After running:

```bash
flutter test integration_test/app_test.dart
```

The screenshot will be saved under:

```
build/integration_test_screenshots/
```

Example path:

```
build/integration_test_screenshots/counter_reached_10.png
```

---

### 💡 Notes

* You can call `takeScreenshot()` multiple times during the test to capture progress:

  ```dart
  await binding.takeScreenshot('before_tap');
  await tester.tap(button);
  await binding.takeScreenshot('after_tap');
  ```

* If running on CI (like GitHub Actions), you can store these screenshots as **artifacts**.

---

Would you like me to show how to **automatically upload these screenshots to GitHub Actions artifacts** after each test run?
