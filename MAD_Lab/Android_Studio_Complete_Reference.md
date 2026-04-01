# ANDROID STUDIO — Complete Exam Reference Guide
### Java | Empty Views Activity | Groovy DSL

---

# SECTION 1: PROJECT SETUP (Do this FIRST every time)

## 1.1 Creating a New Project
File → New → New Project → Empty Views Activity → Next
- Name: YourAppName
- Package: com.example.yourappname
- Language: Java
- Minimum SDK: API 24
- Build configuration language: Groovy DSL

---

## 1.2 MASTER build.gradle (Module :app) — Paste this EVERY time
Open: app/build.gradle (the one INSIDE the 'app' folder, NOT the project-level one)

```groovy
plugins {
    id 'com.android.application'
}

android {
    namespace 'com.example.yourappname'   // match your package
    compileSdk 34

    defaultConfig {
        applicationId 'com.example.yourappname'
        minSdk 24
        targetSdk 34
        versionCode 1
        versionName '1.0'
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.11.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
    implementation 'androidx.recyclerview:recyclerview:1.3.2'
    implementation 'androidx.cardview:cardview:1.0.0'
    implementation 'androidx.viewpager2:viewpager2:1.0.0'
}
```

⚠ After editing build.gradle ALWAYS click 'Sync Now' in the yellow bar at the top!

---

## 1.3 MASTER AndroidManifest.xml — Add lines as needed
Location: app/src/main/AndroidManifest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">

    <!-- ═══ PERMISSIONS (add ABOVE <application>) ═══ -->
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.BLUETOOTH" />
    <uses-permission android:name="android.permission.BLUETOOTH_ADMIN" />
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.YourAppName">

        <!-- MAIN ACTIVITY (already here by default) -->
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <!-- EVERY additional Activity MUST be listed here -->
        <activity android:name=".SecondActivity" android:exported="false" />
        <activity android:name=".ResultActivity" android:exported="false" />

        <!-- BroadcastReceiver -->
        <receiver android:name=".MyReceiver" android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
            </intent-filter>
        </receiver>

        <!-- Service -->
        <service android:name=".MyService" android:exported="false" />

    </application>
</manifest>
```

⚠ MOST COMMON EXAM ERROR: Forgetting to register a new Activity in Manifest → app crashes!

---

# SECTION 2: XML LAYOUTS — Complete Reference

All layout files go in: app/src/main/res/layout/

## 2.1 LinearLayout (stack views vertically or horizontally)

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:id="@+id/tvTitle"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Hello World"
        android:textSize="18sp"
        android:textStyle="bold"
        android:textColor="#000000"
        android:gravity="center" />

    <EditText
        android:id="@+id/etInput"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter text here"
        android:inputType="text"
        android:layout_marginTop="8dp" />

    <!-- inputType options: text | textPassword | number | phone | textEmailAddress -->

    <Button
        android:id="@+id/btnSubmit"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Submit"
        android:layout_gravity="center" />

</LinearLayout>
```

## 2.2 RelativeLayout

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:id="@+id/tvLabel"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Name:"
        android:layout_alignParentTop="true"
        android:layout_alignParentStart="true" />

    <EditText
        android:id="@+id/etName"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/tvLabel" />

    <Button
        android:id="@+id/btnOk"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="OK"
        android:layout_below="@id/etName"
        android:layout_alignParentEnd="true" />

    <Button
        android:id="@+id/btnCancel"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Cancel"
        android:layout_below="@id/etName"
        android:layout_toStartOf="@id/btnOk" />

</RelativeLayout>
```

## 2.3 All UI Widget XML Snippets

### CheckBox
```xml
<CheckBox
    android:id="@+id/cbItem"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Option 1" />
```

### RadioGroup + RadioButton
```xml
<RadioGroup
    android:id="@+id/rgOptions"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content">

    <RadioButton android:id="@+id/rbMale"   android:text="Male"   android:layout_width="wrap_content" android:layout_height="wrap_content"/>
    <RadioButton android:id="@+id/rbFemale" android:text="Female" android:layout_width="wrap_content" android:layout_height="wrap_content"/>
</RadioGroup>
```

### ToggleButton
```xml
<ToggleButton
    android:id="@+id/tbMode"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textOn="ON"
    android:textOff="OFF" />
```

### Switch
```xml
<Switch
    android:id="@+id/swToggle"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Wi-Fi" />
```

### SeekBar
```xml
<SeekBar
    android:id="@+id/seekBar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:max="100"
    android:progress="50" />
```

### Spinner
```xml
<Spinner
    android:id="@+id/spinner"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

### ListView
```xml
<ListView
    android:id="@+id/listView"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```

### ImageView
```xml
<ImageView
    android:id="@+id/ivImage"
    android:layout_width="100dp"
    android:layout_height="100dp"
    android:src="@drawable/ic_launcher_background"
    android:scaleType="centerCrop" />
```

### RatingBar
```xml
<RatingBar
    android:id="@+id/ratingBar"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:numStars="5"
    android:stepSize="1" />
```

---

# SECTION 3: JAVA CODE — Activity Basics

## 3.1 Boilerplate MainActivity.java

```java
package com.example.yourappname;

import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.*;
import android.content.Intent;

public class MainActivity extends AppCompatActivity {

    // Step 1: Declare all views as fields
    TextView tvResult;
    EditText etInput;
    Button btnSubmit;
    CheckBox cbOption;
    RadioGroup rgOptions;
    ToggleButton tbMode;
    Spinner spinner;
    ListView listView;
    Switch swToggle;
    SeekBar seekBar;
    ImageView ivImage;
    RatingBar ratingBar;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);  // link to your XML

        // Step 2: Find views by ID
        tvResult  = findViewById(R.id.tvResult);
        etInput   = findViewById(R.id.etInput);
        btnSubmit = findViewById(R.id.btnSubmit);
        cbOption  = findViewById(R.id.cbOption);
        rgOptions = findViewById(R.id.rgOptions);
        tbMode    = findViewById(R.id.tbMode);
        spinner   = findViewById(R.id.spinner);
        listView  = findViewById(R.id.listView);
        swToggle  = findViewById(R.id.swToggle);
        seekBar   = findViewById(R.id.seekBar);
        ivImage   = findViewById(R.id.ivImage);
        ratingBar = findViewById(R.id.ratingBar);

        // Step 3: Set listeners
        btnSubmit.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                handleSubmit();
            }
        });

        // Lambda shorthand (same thing):
        // btnSubmit.setOnClickListener(v -> handleSubmit());
    }

    private void handleSubmit() {
        String input = etInput.getText().toString().trim();
        if (input.isEmpty()) {
            Toast.makeText(this, "Please enter something", Toast.LENGTH_SHORT).show();
            return;
        }
        tvResult.setText("You entered: " + input);
    }
}
```

## 3.2 Activity Lifecycle (for Lifecycle Lab exercise)

```java
@Override protected void onCreate(Bundle s)  { super.onCreate(s);  Toast.makeText(this,"onCreate",Toast.LENGTH_SHORT).show(); }
@Override protected void onStart()           { super.onStart();    Toast.makeText(this,"onStart",Toast.LENGTH_SHORT).show(); }
@Override protected void onResume()          { super.onResume();   Toast.makeText(this,"onResume",Toast.LENGTH_SHORT).show(); }
@Override protected void onPause()           { super.onPause();    Toast.makeText(this,"onPause",Toast.LENGTH_SHORT).show(); }
@Override protected void onStop()            { super.onStop();     Toast.makeText(this,"onStop",Toast.LENGTH_SHORT).show(); }
@Override protected void onDestroy()         { super.onDestroy();  Toast.makeText(this,"onDestroy",Toast.LENGTH_SHORT).show(); }

// OR use Log for Logcat:
// Log.d("LIFECYCLE", "onCreate called");
```

---

# SECTION 4: INTENTS & MULTI-ACTIVITY NAVIGATION

## 4.1 Explicit Intent (go to another activity in YOUR app)

```java
// In FirstActivity.java — SENDING data
Intent intent = new Intent(MainActivity.this, SecondActivity.class);
intent.putExtra("KEY_NAME",  "John");       // String
intent.putExtra("KEY_AGE",   25);           // int
intent.putExtra("KEY_SCORE", 98.5);         // double
intent.putExtra("KEY_FLAG",  true);         // boolean
startActivity(intent);

// ---------------------------------------------------
// In SecondActivity.java — RECEIVING data
String  name  = getIntent().getStringExtra("KEY_NAME");
int     age   = getIntent().getIntExtra("KEY_AGE", 0);       // 0 = default
double  score = getIntent().getDoubleExtra("KEY_SCORE", 0.0);
boolean flag  = getIntent().getBooleanExtra("KEY_FLAG", false);

// Display received data
tvName.setText("Name: " + name);
```

## 4.2 Implicit Intent (open browser, email, phone, map)

```java
// Open URL in browser
Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse("https://www.google.com"));
startActivity(intent);

// Make a phone call
Intent intent = new Intent(Intent.ACTION_DIAL, Uri.parse("tel:1234567890"));
startActivity(intent);

// Send an email
Intent intent = new Intent(Intent.ACTION_SENDTO, Uri.parse("mailto:someone@example.com"));
intent.putExtra(Intent.EXTRA_SUBJECT, "Subject here");
intent.putExtra(Intent.EXTRA_TEXT, "Body here");
startActivity(intent);

// Share text
Intent intent = new Intent(Intent.ACTION_SEND);
intent.setType("text/plain");
intent.putExtra(Intent.EXTRA_TEXT, "Text to share");
startActivity(Intent.createChooser(intent, "Share via"));
```

## 4.3 Back button in SecondActivity

```java
// Option 1: ActionBar back button
getSupportActionBar().setDisplayHomeAsUpEnabled(true);

@Override
public boolean onOptionsItemSelected(MenuItem item) {
    if (item.getItemId() == android.R.id.home) {
        finish();
        return true;
    }
    return super.onOptionsItemSelected(item);
}

// Option 2: A "Back" button in layout
btnBack.setOnClickListener(v -> finish());
```

---

# SECTION 5: INPUT CONTROLS — Java Code

## 5.1 Button + Toast

```java
btnSubmit.setOnClickListener(v -> {
    Toast.makeText(MainActivity.this, "Button Clicked!", Toast.LENGTH_SHORT).show();
});
// Toast.LENGTH_SHORT = ~2sec,  Toast.LENGTH_LONG = ~3.5sec
```

## 5.2 CheckBox — Reading state

```java
// Single checkbox check
if (cbOption.isChecked()) {
    // option is selected
}

// onChange listener
cbOption.setOnCheckedChangeListener((buttonView, isChecked) -> {
    if (isChecked) tvResult.setText("Checked");
    else           tvResult.setText("Unchecked");
});

// Multiple checkboxes — collect selected items
StringBuilder selected = new StringBuilder();
int total = 0;
if (cbPizza.isChecked())  { selected.append("Pizza - Rs.200\n");  total += 200; }
if (cbBurger.isChecked()) { selected.append("Burger - Rs.150\n"); total += 150; }
if (cbPasta.isChecked())  { selected.append("Pasta - Rs.180\n");  total += 180; }
selected.append("Total: Rs." + total);
tvResult.setText(selected.toString());
```

## 5.3 RadioGroup + RadioButton

```java
rgOptions.setOnCheckedChangeListener((group, checkedId) -> {
    if (checkedId == R.id.rbMale)        tvResult.setText("Male selected");
    else if (checkedId == R.id.rbFemale) tvResult.setText("Female selected");
});

// Read selected RadioButton text
int selectedId = rgOptions.getCheckedRadioButtonId();
RadioButton rb = findViewById(selectedId);
String selectedText = rb.getText().toString();
```

## 5.4 ToggleButton

```java
tbMode.setOnCheckedChangeListener((buttonView, isChecked) -> {
    if (isChecked) {
        tvResult.setText("Mode: Wi-Fi");
        ivIcon.setImageResource(R.drawable.ic_wifi);
    } else {
        tvResult.setText("Mode: Mobile Data");
        ivIcon.setImageResource(R.drawable.ic_data);
    }
});
```

## 5.5 SeekBar

```java
seekBar.setOnSeekBarChangeListener(new SeekBar.OnSeekBarChangeListener() {
    @Override
    public void onProgressChanged(SeekBar seekBar, int progress, boolean fromUser) {
        tvResult.setText("Value: " + progress);
    }
    @Override public void onStartTrackingTouch(SeekBar seekBar) {}
    @Override public void onStopTrackingTouch(SeekBar seekBar) {}
});
```

## 5.6 Spinner — Populate + Handle

```java
// Simple string array spinner
String[] items = {"Car", "Bike", "Bus", "Auto"};
ArrayAdapter<String> adapter = new ArrayAdapter<>(this,
        android.R.layout.simple_spinner_item, items);
adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);
spinner.setAdapter(adapter);

spinner.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
    @Override
    public void onItemSelected(AdapterView<?> parent, View view, int position, long id) {
        String selected = items[position];
        tvResult.setText("Selected: " + selected);
    }
    @Override public void onNothingSelected(AdapterView<?> parent) {}
});

// ArrayList spinner
ArrayList<String> list = new ArrayList<>();
list.add("Item 1"); list.add("Item 2");
ArrayAdapter<String> adapter2 = new ArrayAdapter<>(this,
        android.R.layout.simple_spinner_item, list);
spinner.setAdapter(adapter2);
```

## 5.7 ListView

```java
String[] sports = {"Cricket", "Football", "Basketball", "Tennis"};
ArrayAdapter<String> adapter = new ArrayAdapter<>(this,
        android.R.layout.simple_list_item_1, sports);
listView.setAdapter(adapter);

// Click listener
listView.setOnItemClickListener((parent, view, position, id) -> {
    String selected = sports[position];
    Toast.makeText(MainActivity.this, "Selected: " + selected, Toast.LENGTH_SHORT).show();
});

// Long press listener
listView.setOnItemLongClickListener((parent, view, position, id) -> {
    Toast.makeText(MainActivity.this, "Long pressed: " + sports[position], Toast.LENGTH_SHORT).show();
    return true; // must return true to consume event
});
```

---

# SECTION 6: DATE PICKER & TIME PICKER

## 6.1 DatePickerDialog — Complete working code

```java
import android.app.DatePickerDialog;
import java.util.Calendar;

// Get today's date for defaults
Calendar cal = Calendar.getInstance();
int year  = cal.get(Calendar.YEAR);
int month = cal.get(Calendar.MONTH);
int day   = cal.get(Calendar.DAY_OF_MONTH);

// Show DatePicker when button clicked
btnPickDate.setOnClickListener(v -> {
    DatePickerDialog dpd = new DatePickerDialog(MainActivity.this,
        (view2, selectedYear, selectedMonth, selectedDay) -> {
            String date = selectedDay + "/" + (selectedMonth + 1) + "/" + selectedYear;
            tvDate.setText("Date: " + date);
        }, year, month, day);
    dpd.show();
});
```

## 6.2 TimePickerDialog — Complete working code

```java
import android.app.TimePickerDialog;

btnPickTime.setOnClickListener(v -> {
    TimePickerDialog tpd = new TimePickerDialog(MainActivity.this,
        (view2, hourOfDay, minute) -> {
            String time = String.format("%02d:%02d", hourOfDay, minute);
            tvTime.setText("Time: " + time);
        }, cal.get(Calendar.HOUR_OF_DAY), cal.get(Calendar.MINUTE), true); // true = 24hr format
    tpd.show();
});
```

---

# SECTION 7: MENUS — Options, Context, Popup

## 7.1 Options Menu (3-dot menu in AppBar)

**Step 1 — Create menu XML:** `app/src/main/res/menu/menu_main.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <item android:id="@+id/action_search"
          android:title="Search"
          android:icon="@android:drawable/ic_menu_search"
          app:showAsAction="ifRoom" />

    <item android:id="@+id/action_settings"
          android:title="Settings"
          app:showAsAction="never" />

    <item android:id="@+id/action_about"
          android:title="About"
          app:showAsAction="never" />
</menu>
```

**Step 2 — Java code in Activity:**

```java
// Inflate the menu
@Override
public boolean onCreateOptionsMenu(Menu menu) {
    getMenuInflater().inflate(R.menu.menu_main, menu);
    return true;
}

// Handle clicks
@Override
public boolean onOptionsItemSelected(MenuItem item) {
    int id = item.getItemId();
    if (id == R.id.action_search) {
        Toast.makeText(this, "Search clicked", Toast.LENGTH_SHORT).show();
        return true;
    } else if (id == R.id.action_settings) {
        startActivity(new Intent(this, SettingsActivity.class));
        return true;
    } else if (id == R.id.action_about) {
        tvContent.setText("About Us: XYZ App v1.0");
        return true;
    }
    return super.onOptionsItemSelected(item);
}
```

---

## 7.2 Context Menu (long-press floating menu)

**Step 1 — Create** `res/menu/context_menu.xml`

```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/ctx_edit"   android:title="Edit" />
    <item android:id="@+id/ctx_delete" android:title="Delete" />
    <item android:id="@+id/ctx_share"  android:title="Share" />
</menu>
```

**Step 2 — Java code:**

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    registerForContextMenu(listView);  // register ANY view for long-press context menu
}

@Override
public void onCreateContextMenu(ContextMenu menu, View v, ContextMenu.ContextMenuInfo info) {
    super.onCreateContextMenu(menu, v, info);
    menu.setHeaderTitle("Choose Action");
    getMenuInflater().inflate(R.menu.context_menu, menu);
}

@Override
public boolean onContextItemSelected(MenuItem item) {
    int id = item.getItemId();
    if (id == R.id.ctx_edit)   { Toast.makeText(this, "Edit",   Toast.LENGTH_SHORT).show(); return true; }
    if (id == R.id.ctx_delete) { Toast.makeText(this, "Delete", Toast.LENGTH_SHORT).show(); return true; }
    if (id == R.id.ctx_share)  { Toast.makeText(this, "Share",  Toast.LENGTH_SHORT).show(); return true; }
    return super.onContextItemSelected(item);
}
```

---

## 7.3 Popup Menu (anchored to a button)

**Step 1 — Create** `res/menu/popup_menu.xml`

```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/popup_option1" android:title="Option 1" />
    <item android:id="@+id/popup_option2" android:title="Option 2" />
    <item android:id="@+id/popup_option3" android:title="Option 3" />
</menu>
```

**Step 2 — Java code:**

```java
import android.widget.PopupMenu;

btnShowPopup.setOnClickListener(v -> {
    PopupMenu popup = new PopupMenu(MainActivity.this, v); // v = anchor view (the button)
    popup.getMenuInflater().inflate(R.menu.popup_menu, popup.getMenu());
    popup.setOnMenuItemClickListener(item -> {
        int id = item.getItemId();
        if (id == R.id.popup_option1) { tvResult.setText("Option 1 chosen"); return true; }
        if (id == R.id.popup_option2) { tvResult.setText("Option 2 chosen"); return true; }
        if (id == R.id.popup_option3) { tvResult.setText("Option 3 chosen"); return true; }
        return false;
    });
    popup.show();
});
```

---

# SECTION 8: SQLITE DATABASE — Full Working Template

## 8.1 DatabaseHelper.java — Copy-paste template

```java
package com.example.yourappname;

import android.content.ContentValues;
import android.content.Context;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;

public class DatabaseHelper extends SQLiteOpenHelper {

    private static final String DB_NAME    = "MyApp.db";
    private static final int    DB_VERSION = 1;

    // Table name and columns — change these to match your app
    public static final String TABLE        = "tasks";
    public static final String COL_ID       = "id";
    public static final String COL_NAME     = "name";
    public static final String COL_DATE     = "due_date";
    public static final String COL_PRIORITY = "priority";

    public DatabaseHelper(Context context) {
        super(context, DB_NAME, null, DB_VERSION);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        String createTable = "CREATE TABLE " + TABLE + " (" +
                COL_ID       + " INTEGER PRIMARY KEY AUTOINCREMENT, " +
                COL_NAME     + " TEXT NOT NULL, " +
                COL_DATE     + " TEXT, " +
                COL_PRIORITY + " TEXT)";
        db.execSQL(createTable);
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        db.execSQL("DROP TABLE IF EXISTS " + TABLE);
        onCreate(db);
    }

    // ─── INSERT ───
    public long insert(String name, String date, String priority) {
        SQLiteDatabase db = getWritableDatabase();
        ContentValues cv = new ContentValues();
        cv.put(COL_NAME,     name);
        cv.put(COL_DATE,     date);
        cv.put(COL_PRIORITY, priority);
        return db.insert(TABLE, null, cv);
    }

    // ─── READ ALL ───
    public Cursor getAll() {
        return getReadableDatabase().rawQuery("SELECT * FROM " + TABLE, null);
    }

    // ─── READ ONE by ID ───
    public Cursor getById(int id) {
        return getReadableDatabase().rawQuery(
            "SELECT * FROM " + TABLE + " WHERE " + COL_ID + "=?",
            new String[]{String.valueOf(id)});
    }

    // ─── UPDATE ───
    public int update(int id, String newName, String newDate, String newPriority) {
        SQLiteDatabase db = getWritableDatabase();
        ContentValues cv = new ContentValues();
        cv.put(COL_NAME,     newName);
        cv.put(COL_DATE,     newDate);
        cv.put(COL_PRIORITY, newPriority);
        return db.update(TABLE, cv, COL_ID + "=?", new String[]{String.valueOf(id)});
    }

    // ─── DELETE ───
    public int delete(int id) {
        return getWritableDatabase().delete(TABLE, COL_ID + "=?", new String[]{String.valueOf(id)});
    }
}
```

## 8.2 Using DatabaseHelper in an Activity

```java
DatabaseHelper dbHelper = new DatabaseHelper(this);

// INSERT
long rowId = dbHelper.insert("Buy groceries", "2025-06-01", "High");
if (rowId > 0) Toast.makeText(this, "Saved!", Toast.LENGTH_SHORT).show();
else           Toast.makeText(this, "Error saving", Toast.LENGTH_SHORT).show();

// READ and display in ListView
Cursor cursor = dbHelper.getAll();
ArrayList<String> taskList = new ArrayList<>();
if (cursor.moveToFirst()) {
    do {
        String name     = cursor.getString(cursor.getColumnIndexOrThrow(DatabaseHelper.COL_NAME));
        String priority = cursor.getString(cursor.getColumnIndexOrThrow(DatabaseHelper.COL_PRIORITY));
        taskList.add(name + " [" + priority + "]");
    } while (cursor.moveToNext());
}
cursor.close();

ArrayAdapter<String> adapter = new ArrayAdapter<>(this,
        android.R.layout.simple_list_item_1, taskList);
listView.setAdapter(adapter);
```

---

## 8.3 Shared Preferences — Complete Reference

```java
// Initialize
SharedPreferences prefs = getSharedPreferences("MyPrefs", Context.MODE_PRIVATE);
SharedPreferences.Editor editor = prefs.edit();

// ── SAVE ──
editor.putString("username",   "JohnDoe");
editor.putInt("age",            25);
editor.putBoolean("isLoggedIn", true);
editor.putFloat("score",        98.5f);
editor.apply();  // ALWAYS call apply() to save!

// ── READ ──
String  name     = prefs.getString("username",   "default");
int     age      = prefs.getInt("age",            0);
boolean loggedIn = prefs.getBoolean("isLoggedIn", false);

// ── DELETE ONE KEY ──
editor.remove("username"); editor.apply();

// ── CLEAR ALL ──
editor.clear(); editor.apply();

// ── SAVE on app close (onPause), RESTORE on open (onCreate) ──
@Override
protected void onPause() {
    super.onPause();
    SharedPreferences.Editor e = getSharedPreferences("MyPrefs", MODE_PRIVATE).edit();
    e.putString("saved_text", etInput.getText().toString());
    e.apply();
}

// In onCreate, after setContentView:
String savedText = getSharedPreferences("MyPrefs", MODE_PRIVATE).getString("saved_text", "");
etInput.setText(savedText);
```

---

# SECTION 9: TABLAYOUT + VIEWPAGER2 + FRAGMENTS

## 9.1 TabLayout — Complete Step-by-Step (5 files)

**Step 1 — activity_main.xml:**

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <com.google.android.material.tabs.TabLayout
        android:id="@+id/tabLayout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:tabMode="fixed"
        app:tabGravity="fill" />

    <androidx.viewpager2.widget.ViewPager2
        android:id="@+id/viewPager"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</LinearLayout>
```

**Step 2 — fragment_tab.xml:**

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center">

    <TextView
        android:id="@+id/tvTabContent"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="24sp" />
</LinearLayout>
```

**Step 3 — TabFragment.java:**

```java
package com.example.yourappname;
import android.os.Bundle;
import android.view.*;
import android.widget.TextView;
import androidx.fragment.app.Fragment;

public class TabFragment extends Fragment {

    public static TabFragment newInstance(String text) {
        TabFragment f = new TabFragment();
        Bundle args = new Bundle();
        args.putString("content", text);
        f.setArguments(args);
        return f;
    }

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        View v = inflater.inflate(R.layout.fragment_tab, container, false);
        String content = getArguments() != null ? getArguments().getString("content") : "Tab";
        v.<TextView>findViewById(R.id.tvTabContent).setText(content);
        return v;
    }
}
```

**Step 4 — ViewPagerAdapter.java:**

```java
package com.example.yourappname;
import androidx.annotation.NonNull;
import androidx.fragment.app.*;
import androidx.viewpager2.adapter.FragmentStateAdapter;
import java.util.*;

public class ViewPagerAdapter extends FragmentStateAdapter {
    private List<Fragment> fragments = new ArrayList<>();
    private List<String>   titles    = new ArrayList<>();

    public ViewPagerAdapter(FragmentActivity fa) { super(fa); }

    public void addFragment(Fragment f, String title) {
        fragments.add(f); titles.add(title);
    }

    @NonNull @Override
    public Fragment createFragment(int position) { return fragments.get(position); }

    @Override
    public int getItemCount() { return fragments.size(); }

    public String getTitle(int position) { return titles.get(position); }
}
```

**Step 5 — MainActivity.java:**

```java
import com.google.android.material.tabs.TabLayout;
import com.google.android.material.tabs.TabLayoutMediator;
import androidx.viewpager2.widget.ViewPager2;

TabLayout  tabLayout  = findViewById(R.id.tabLayout);
ViewPager2 viewPager  = findViewById(R.id.viewPager);

ViewPagerAdapter adapter = new ViewPagerAdapter(this);
adapter.addFragment(TabFragment.newInstance("Top Stories content"), "Top Stories");
adapter.addFragment(TabFragment.newInstance("Sports content"),       "Sports");
adapter.addFragment(TabFragment.newInstance("Entertainment here"),   "Entertainment");
viewPager.setAdapter(adapter);

new TabLayoutMediator(tabLayout, viewPager,
    (tab, position) -> tab.setText(adapter.getTitle(position))
).attach();
```

---

# SECTION 10: COMMON EXAM PATTERNS — Ready Solutions

## 10.1 Calculator App

```java
// Layout: 2 EditTexts, 4 Buttons (+, -, *, /), 1 TextView for result

btnAdd.setOnClickListener(v -> calculate("+"));
btnSub.setOnClickListener(v -> calculate("-"));
btnMul.setOnClickListener(v -> calculate("*"));
btnDiv.setOnClickListener(v -> calculate("/"));

void calculate(String op) {
    String s1 = etNum1.getText().toString().trim();
    String s2 = etNum2.getText().toString().trim();
    if (s1.isEmpty() || s2.isEmpty()) {
        Toast.makeText(this, "Enter both numbers", Toast.LENGTH_SHORT).show();
        return;
    }
    double n1 = Double.parseDouble(s1);
    double n2 = Double.parseDouble(s2);
    double result = 0;
    switch (op) {
        case "+": result = n1 + n2; break;
        case "-": result = n1 - n2; break;
        case "*": result = n1 * n2; break;
        case "/":
            if (n2 == 0) { tvResult.setText("Cannot divide by zero"); return; }
            result = n1 / n2;
            break;
    }
    // Display as: Num1 op Num2 = Result
    tvResult.setText(n1 + " " + op + " " + n2 + " = " + result);
}
```

## 10.2 Food Ordering App — Multi-checkbox with total

```java
btnSubmit.setOnClickListener(v -> {
    StringBuilder order = new StringBuilder("Your Order:\n");
    int total = 0;
    if (cbPizza.isChecked())  { order.append("Pizza  - Rs.200\n"); total += 200; cbPizza.setEnabled(false); }
    if (cbBurger.isChecked()) { order.append("Burger - Rs.150\n"); total += 150; cbBurger.setEnabled(false); }
    if (cbPasta.isChecked())  { order.append("Pasta  - Rs.180\n"); total += 180; cbPasta.setEnabled(false); }
    order.append("Total: Rs." + total);

    Intent intent = new Intent(this, ResultActivity.class);
    intent.putExtra("ORDER_DETAILS", order.toString());
    startActivity(intent);
    btnSubmit.setEnabled(false); // prevent re-ordering
});
```

## 10.3 Ticket Booking — Spinner + DatePicker + ToggleButton + Submit

```java
btnSubmit.setOnClickListener(v -> {
    String source  = spinnerFrom.getSelectedItem().toString();
    String dest    = spinnerTo.getSelectedItem().toString();
    String date    = tvDate.getText().toString();
    String type    = tbTicket.isChecked() ? "Round Trip" : "One Way";

    if (source.equals(dest)) {
        Toast.makeText(this, "Source and destination cannot be same", Toast.LENGTH_SHORT).show();
        return;
    }

    Intent intent = new Intent(this, ConfirmActivity.class);
    intent.putExtra("SOURCE", source);
    intent.putExtra("DEST",   dest);
    intent.putExtra("DATE",   date);
    intent.putExtra("TYPE",   type);
    startActivity(intent);
});

// Reset button
btnReset.setOnClickListener(v -> {
    spinnerFrom.setSelection(0);
    spinnerTo.setSelection(0);
    tvDate.setText("Select Date");
    tbTicket.setChecked(false);
});
```

## 10.4 URL Launcher App

```java
btnOpen.setOnClickListener(v -> {
    String url = etUrl.getText().toString().trim();
    if (!url.startsWith("http://") && !url.startsWith("https://"))
        url = "https://" + url;
    Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse(url));
    startActivity(intent);
});
```

## 10.5 Parking Registration — Spinner + Fields + Serial Number

```java
btnSubmit.setOnClickListener(v -> {
    String vehicleType = spinner.getSelectedItem().toString();
    String vehicleNo   = etVehicleNo.getText().toString().trim();
    String rcNo        = etRcNo.getText().toString().trim();

    if (vehicleNo.isEmpty() || rcNo.isEmpty()) {
        Toast.makeText(this, "Fill all fields", Toast.LENGTH_SHORT).show();
        return;
    }

    int serial = (int)(Math.random() * 9000) + 1000; // random 4-digit serial

    Intent intent = new Intent(this, ConfirmActivity.class);
    intent.putExtra("TYPE",    vehicleType);
    intent.putExtra("VEHICLE", vehicleNo);
    intent.putExtra("RC",      rcNo);
    intent.putExtra("SERIAL",  serial);
    startActivity(intent);
});
```

## 10.6 Sports List — ListView + Toast on click

```java
String[] sports = {"Cricket", "Football", "Basketball", "Tennis", "Badminton", "Hockey"};
ArrayAdapter<String> adapter = new ArrayAdapter<>(this,
        android.R.layout.simple_list_item_1, sports);
listView.setAdapter(adapter);

listView.setOnItemClickListener((parent, view, position, id) -> {
    Toast.makeText(this, "Selected: " + sports[position], Toast.LENGTH_SHORT).show();
});
```

---

# SECTION 11: BROADCAST RECEIVER & CAMERA

## 11.1 BroadcastReceiver

```java
// MyReceiver.java
import android.content.BroadcastReceiver;
import android.content.Context;
import android.content.Intent;
import android.widget.Toast;

public class MyReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
        Toast.makeText(context, "Broadcast received: " + intent.getAction(),
                Toast.LENGTH_LONG).show();
    }
}

// ─ Register programmatically in Activity ─
IntentFilter filter = new IntentFilter("com.example.MY_ACTION");
MyReceiver receiver = new MyReceiver();
registerReceiver(receiver, filter);

// ─ Send broadcast ─
Intent intent = new Intent("com.example.MY_ACTION");
sendBroadcast(intent);

// ─ Unregister in onDestroy ─
@Override
protected void onDestroy() {
    super.onDestroy();
    unregisterReceiver(receiver);
}
```

## 11.2 Camera Intent (take photo)

```java
// Add to Manifest: <uses-permission android:name="android.permission.CAMERA" />

private static final int REQUEST_IMAGE_CAPTURE = 1;

btnCamera.setOnClickListener(v -> {
    Intent intent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
    if (intent.resolveActivity(getPackageManager()) != null)
        startActivityForResult(intent, REQUEST_IMAGE_CAPTURE);
});

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    if (requestCode == REQUEST_IMAGE_CAPTURE && resultCode == RESULT_OK) {
        Bitmap thumbnail = (Bitmap) data.getExtras().get("data");
        imageView.setImageBitmap(thumbnail);
    }
}
```

---

# SECTION 12: TROUBLESHOOTING & QUICK REFERENCE

## 12.1 Common Errors & Fixes

| Error | Fix |
|-------|-----|
| App crashes on launch | Every Activity used must be declared in Manifest `<application>` block |
| NullPointerException on findViewById | ID in Java must EXACTLY match ID in XML |
| Cannot resolve symbol R | Check XML for errors → Clean Project → Rebuild Project |
| Gradle sync failed | File → Sync Project with Gradle Files. Check build.gradle for typos |
| No resource found @id/... | You're referencing an ID that doesn't exist in the current layout |
| PopupMenu import missing | `import android.widget.PopupMenu;` |
| TabLayout not found | Add material dependency in build.gradle and sync |
| ViewPager2 not found | Add `implementation 'androidx.viewpager2:viewpager2:1.0.0'` |
| SQLite table not created | Uninstall app from emulator then reinstall (onCreate only runs once!) |
| Spinner shows blank | Call setAdapter() AFTER creating the adapter AND setting the layout |
| Toast not showing | You forgot to call `.show()` at the end |
| Intent data not received | Key string must be IDENTICAL in putExtra() and getExtra() |
| DatePicker not showing | Call `.show()` on the DatePickerDialog object |
| Context menu not appearing | Call `registerForContextMenu(view)` in onCreate |

---

## 12.2 MUST-ADD IMPORTS — Copy to top of every Java file

```java
import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import android.content.Intent;
import android.content.Context;
import android.content.SharedPreferences;
import android.net.Uri;
import android.view.Menu;
import android.view.MenuItem;
import android.view.View;
import android.view.MenuInflater;
import android.view.ContextMenu;
import android.widget.*;
import android.app.DatePickerDialog;
import android.app.TimePickerDialog;
import android.database.Cursor;
import android.database.sqlite.*;
import android.content.ContentValues;
import android.graphics.Bitmap;
import android.provider.MediaStore;
import android.widget.PopupMenu;
import android.content.IntentFilter;
import android.content.BroadcastReceiver;
import java.util.*;
```

---

## 12.3 XML Dimensions Quick Reference

| Unit | Use For | Example |
|------|---------|---------|
| `match_parent` | Fill the parent completely | `android:layout_width="match_parent"` |
| `wrap_content` | Only as big as content needs | `android:layout_height="wrap_content"` |
| `dp` | Width, height, margin, padding | `android:padding="16dp"` |
| `sp` | TEXT sizes ONLY | `android:textSize="18sp"` |
| `android:padding` | Space INSIDE the view border | `android:padding="8dp"` |
| `android:layout_margin` | Space OUTSIDE the view | `android:layout_margin="8dp"` |
| `android:gravity` | Aligns CONTENT inside view | `android:gravity="center"` |
| `android:layout_gravity` | Aligns VIEW inside parent | `android:layout_gravity="center_horizontal"` |

---

## 12.4 How to Add a New Activity — Step by Step

1. Right-click on your package (in java folder) → New → Activity → Empty Views Activity
2. Give it a name (e.g. `ResultActivity`) — Android Studio auto-adds it to Manifest
3. Edit the new `activity_result.xml` layout as needed
4. In `ResultActivity.java`, use `getIntent().getXXXExtra("KEY")` to receive data
5. To go back: `btnBack.setOnClickListener(v -> finish());`
6. To go back AND send data back: use `startActivityForResult()` instead of `startActivity()`

## 12.5 How to Add a Drawable/Image

1. Copy your image file (PNG/JPG) to `app/src/main/res/drawable/`
2. Use in XML: `android:src="@drawable/your_image_name"` (no extension in XML)
3. Use in Java: `imageView.setImageResource(R.drawable.your_image_name);`

⚠ Image filename must be lowercase letters, numbers, underscores ONLY. No spaces, no caps!

## 12.6 How to Add String Resources (best practice)

In `res/values/strings.xml`:
```xml
<resources>
    <string name="app_name">MyApp</string>
    <string name="btn_submit">Submit</string>
</resources>
```
Use in XML: `android:text="@string/btn_submit"`

---

## 12.7 TableLayout — Quick XML

```xml
<TableLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:stretchColumns="1">

    <TableRow>
        <TextView android:text="Name"  android:padding="8dp" android:textStyle="bold"/>
        <TextView android:text="John"  android:padding="8dp"/>
    </TableRow>
    <TableRow>
        <TextView android:text="Age"   android:padding="8dp" android:textStyle="bold"/>
        <TextView android:text="25"    android:padding="8dp"/>
    </TableRow>
    <TableRow>
        <TextView android:text="Email" android:padding="8dp" android:textStyle="bold"/>
        <TextView android:text="john@mail.com" android:padding="8dp"/>
    </TableRow>
</TableLayout>
```

## 12.8 GridView — Quick Setup

```xml
<!-- In XML -->
<GridView
    android:id="@+id/gridView"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:numColumns="3"
    android:horizontalSpacing="4dp"
    android:verticalSpacing="4dp" />
```

```java
// In Java
String[] items = {"A","B","C","D","E","F","G","H","I"};
ArrayAdapter<String> adapter = new ArrayAdapter<>(this,
        android.R.layout.simple_list_item_1, items);
gridView.setAdapter(adapter);

gridView.setOnItemClickListener((parent, view, position, id) ->
    Toast.makeText(this, "Clicked: " + items[position], Toast.LENGTH_SHORT).show()
);
```

---

# GOOD LUCK IN YOUR EXAM! You've got this 💪
