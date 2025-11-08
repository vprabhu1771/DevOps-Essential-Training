Here’s a complete **Flutter integration test** for your `SettingScreen` and `UiProvider` setup.
This test verifies that:
✅ The settings screen loads correctly
✅ The dark mode switch toggles the theme state
✅ The UI updates when the switch is toggled

---

### 🧪 Step 1: Folder structure

Create a folder named:

```
integration_test/
```

Then create the file:

```
integration_test/setting_screen_test.dart
```

---

### 🧾 Step 2: Integration test code

```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:integration_test/integration_test.dart';
import 'package:provider/provider.dart';
import 'package:untitled/services/UiProvider.dart';
import 'package:untitled/screens/SettingScreen.dart';

void main() {
  IntegrationTestWidgetsFlutterBinding.ensureInitialized();

  group('SettingScreen Integration Test', () {
    testWidgets('should toggle dark theme when switch is pressed',
        (WidgetTester tester) async {
      // Build the app with UiProvider
      await tester.pumpWidget(
        ChangeNotifierProvider(
          create: (context) => UiProvider()..init(),
          child: Consumer<UiProvider>(
            builder: (context, UiProvider notifier, child) {
              return MaterialApp(
                themeMode:
                    notifier.isDark ? ThemeMode.dark : ThemeMode.light,
                darkTheme: notifier.darkTheme,
                theme: notifier.lightTheme,
                home: const SettingScreen(title: 'Settings'),
              );
            },
          ),
        ),
      );

      // Let async operations (like SharedPreferences) complete
      await tester.pumpAndSettle();

      // Verify title
      expect(find.text('Settings'), findsOneWidget);

      // Find the dark theme switch
      final switchFinder = find.byType(Switch);
      expect(switchFinder, findsOneWidget);

      // Get the provider
      UiProvider provider =
          Provider.of<UiProvider>(tester.element(switchFinder), listen: false);
      final initialTheme = provider.isDark;

      // Tap on the switch
      await tester.tap(switchFinder);
      await tester.pumpAndSettle();

      // Check the provider state has changed
      final toggledTheme = provider.isDark;
      expect(toggledTheme, isNot(equals(initialTheme)));

      // Verify the switch visually updates
      Switch themeSwitch = tester.widget<Switch>(switchFinder);
      expect(themeSwitch.value, toggledTheme);
    });
  });
}
```

---

### 🧩 Step 3: Run the test

In your terminal, run:

```bash
flutter test integration_test/setting_screen_test.dart
```

Or to run all integration tests:

```bash
flutter test integration_test
```

---

### ✅ What this test does:

* Launches your `SettingScreen` wrapped in the correct `UiProvider`
* Confirms that the screen title “Settings” is displayed
* Locates the dark mode `Switch`
* Taps it to toggle dark mode
* Verifies that the `UiProvider`’s `isDark` value changes
* Checks that the switch UI reflects the new state

---

Would you like me to extend this test to also verify that the **theme colors** (e.g., background or AppBar color) actually change when toggling the switch?
