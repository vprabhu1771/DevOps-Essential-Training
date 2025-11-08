Here’s your Flutter code fixed with a working **Switch UI** that toggles between **on/off** states — UI only, no app-wide theme logic yet:

```dart
import 'package:flutter/material.dart';

class SettingScreen extends StatefulWidget {
  final String title;

  const SettingScreen({super.key, required this.title});

  @override
  State<SettingScreen> createState() => _SettingScreenState();
}

class _SettingScreenState extends State<SettingScreen> {
  bool _isDarkTheme = false; // local switch state

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Column(
        children: [
          ListTile(
            leading: const Icon(Icons.dark_mode),
            title: const Text("Dark Theme"),
            trailing: Switch(
              value: _isDarkTheme,
              onChanged: (value) {
                setState(() {
                  _isDarkTheme = value;
                });
              },
            ),
          ),
        ],
      ),
    );
  }
}
```

✅ **What’s added:**

* `_isDarkTheme` state variable for the switch.
* `setState()` updates UI when toggled.

Would you like me to extend it so that it actually changes the app’s theme (light/dark) globally?



Here’s a simple **Flutter integration (widget) test** for your `SettingScreen` that verifies:
✅ The screen title displays correctly
✅ The switch toggles between `false` and `true` when tapped

---

### 🧪 File: `test/setting_screen_test.dart`

```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:your_app/setting_screen.dart'; // <-- Update with your actual import path

void main() {
  testWidgets('Settings screen toggle works', (WidgetTester tester) async {
    // Build the SettingScreen widget
    await tester.pumpWidget(
      const MaterialApp(
        home: SettingScreen(title: 'Settings'),
      ),
    );

    // Verify title appears
    expect(find.text('Settings'), findsOneWidget);

    // Find the Switch widget
    final switchFinder = find.byType(Switch);

    // Verify the switch exists and is initially off
    Switch toggle = tester.widget(switchFinder);
    expect(toggle.value, isFalse);

    // Tap the switch
    await tester.tap(switchFinder);
    await tester.pumpAndSettle();

    // Verify switch state changed to true
    toggle = tester.widget(switchFinder);
    expect(toggle.value, isTrue);
  });
}
```

---

### 💡 Run this test:

```bash
flutter test test/setting_screen_test.dart
```

---

Would you like me to extend the test to check **persistent theme state (using SharedPreferences)** so that the switch remembers its value after app restart?
