නියමයි! සුපිරි වෙළඳසැලක (Supermarket) එළවළු සහ පළතුරු අපතේ යාම (Wastage) කළමනාකරණය කරන Android App එකක් හදන්න අවශ්‍ය සම්පූර්ණ මගපෙන්වීම මෙන්න.

මම මේ සඳහා තෝරාගත්තේ **Flutter** තාක්ෂණයයි. මොකද ඒකෙන් ඉතාම ලස්සන (User-friendly) UI එකක් සහ පහසුවෙන් PDF ලබාගැනීමේ හැකියාව තියෙනවා.

---

# 📝 Supermarket Wastage Management System (Android App)

මෙම පද්ධතිය මගින් දිනපතා අපතේ යන එළවළු සහ පළතුරු දත්ත ඇතුළත් කර, දවස අවසානයේ සවිස්තරාත්මක PDF වාර්තාවක් ලබාගත හැක.

## 🚀 පියවර 01: පරිගණකය සූදානම් කරගැනීම (Setup)

මුලින්ම ඔයාගේ Computer එකේ මේවා Install කරලා තියෙන්න ඕනේ:

1. **Flutter SDK:** [flutter.dev](https://docs.flutter.dev/get-started/install) වෙතින් download කර setup කරගන්න.
2. **Android Studio:** Android App එක run කරන්න මෙය අවශ්‍යයි.
3. **VS Code:** Code එක ලියන්න ලේසිම tool එක (Flutter extension එක install කරගන්න).

---

## 📂 පියවර 02: නව Project එකක් සෑදීම

Terminal එකේ හෝ Command Prompt එකේ පහත විධානය (Command) ලබා දෙන්න:

```bash
flutter create wastage_tracker
cd wastage_tracker

```

ඉන්පසු `pubspec.yaml` file එකට ගොස් අවශ්‍ය Packages ඇතුළත් කරගන්න:

```yaml
dependencies:
  flutter:
    sdk: flutter
  sqflite: ^2.3.0        # Phone එකේ දත්ත සේව් කිරීමට
  path: ^1.8.3
  pdf: ^3.10.8           # PDF එක නිර්මාණය කිරීමට
  printing: ^5.11.1      # PDF එක Print/Save කිරීමට
  intl: ^0.19.0          # දිනය Format කිරීමට

```

*ඉන්පසු `flutter pub get` විධානය ලබා දී මේවා install කරගන්න.*

---

## 🏗️ පියවර 03: Code Structure (පද්ධතියේ සැකැස්ම)

හොඳ App එකක් හදද්දී code එක කොටස් වලට බෙදා ගැනීම වැදගත්:

1. **`models/wastage_item.dart`**: දත්ත ගබඩා කරන ආකෘතිය (Item Name, Weight, Date).
2. **`services/database_helper.dart`**: SQLite database එක හැසිරවීම.
3. **`services/pdf_service.dart`**: PDF එක නිර්මාණය කරන logic එක.
4. **`screens/home_screen.dart`**: දත්ත ඇතුළත් කරන Form එක සහ බොත්තම්.

---

## 👨‍💻 පියවර 04: ප්‍රධාන Code එක (Example Logic)

ඔයාගේ `lib/main.dart` එකේ හෝ අදාළ file එකේ තිබිය යුතු ප්‍රධාන කොටස් මෙන්න:

### A. දත්ත ඇතුළත් කිරීමේ UI එක (User Friendly Form)

පරිශීලකයාට පහසුවෙන් දත්ත ඇතුළත් කිරීමට Dropdown එකක් සහ Text field එකක් භාවිතා කරන්න.

```dart
// සරල උදාහරණයක් - බර ඇතුළත් කරන කොටස
TextField(
  keyboardType: TextInputType.number,
  decoration: InputDecoration(
    labelText: "බර (Weight in kg)",
    border: OutlineInputBorder(),
    prefixIcon: Icon(Icons.scale),
  ),
  onChanged: (val) => weight = double.parse(val),
),

```

### B. PDF එක නිර්මාණය කිරීමේ Logic එක

මෙය තමයි ඔයා ඉල්ලපු ප්‍රධානම කොටස.

```dart
import 'package:pdf/widgets.dart' as pw;
import 'package:printing/printing.dart';

class PdfService {
  static Future<void> generateDailyReport(List<Map<String, dynamic>> data) async {
    final pdf = pw.Document();

    pdf.addPage(
      pw.Page(
        build: (context) => pw.Column(
          children: [
            pw.Text("Daily Wastage Summary Report", style: pw.TextStyle(fontSize: 24, fontWeight: pw.FontWeight.bold)),
            pw.Divider(),
            pw.TableHelper.fromTextArray(
              headers: ['Item Name', 'Category', 'Weight (kg)'],
              data: data.map((item) => [item['name'], item['category'], item['weight']]).toList(),
            ),
            pw.SizedBox(height: 20),
            pw.Text("Total Items: ${data.length}"),
          ],
        ),
      ),
    );

    await Printing.layoutPdf(onLayout: (format) => pdf.save());
  }
}

```

## ✅ පද්ධතියේ ඇති වාසි (Key Features)

* **Offline Support:** ඉන්ටර්නෙට් නැතිව වැඩ කරයි (SQLite නිසා).
* **Simple UI:** ඕනෑම සේවකයෙකුට පහසුවෙන් තේරුම් ගත හැක.
* **Professional PDF:** ආයතනයේ අවශ්‍යතා වලට ගැලපෙන ලෙස PDF වාර්තාව සකස් කළ හැක.
* **No Cost:** සම්පූර්ණයෙන්ම නොමිලේ සාදාගත හැක.

---
