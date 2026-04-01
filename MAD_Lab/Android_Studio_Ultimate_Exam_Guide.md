# 🚀 ANDROID STUDIO — ULTIMATE EXAM GUIDE
### "If you read this once, you can solve ANY question" — ICT 3268 MAD Lab

---

## 📌 HOW TO USE THIS DOCUMENT
- Each section = one type of feature/concept
- Every section has: **What it is → Setup steps → Full code → Common exam variations**
- Search (Ctrl+F) the feature name to jump directly
- Everything is self-contained — you don't need to remember anything else

---

# ═══════════════════════════════════════
# PART 0: ANDROID PROJECT SETUP (DO THIS FIRST ALWAYS)
# ═══════════════════════════════════════

## Step-by-step: Creating a New Project

1. Open Android Studio → **New Project**
2. Choose **Empty Views Activity** (NOT Compose)
3. Name: whatever the question says | Language: **Java** | Min SDK: API 21
4. Click Finish

## Files you will ALWAYS touch:
| File | Where | What it does |
|------|--------|--------------|
| `activity_main.xml` | `res/layout/` | The screen design (UI) |
| `MainActivity.java` | `java/com.yourpackage/` | The logic/code |
| `AndroidManifest.xml` | `manifests/` | Declares activities, permissions |
| `strings.xml` | `res/values/` | Text constants (optional) |
| `build.gradle (Module)` | Gradle Scripts | Add dependencies here |

## Rule: Every new Activity you create needs:
1. A new `.java` file
2. A new `.xml` layout file
3. Declared in `AndroidManifest.xml`

```xml
<!-- AndroidManifest.xml — add this inside <application> tag -->
<activity android:name=".SecondActivity"></activity>
```

---

# ═══════════════════════════════════════
# PART 1: DISPLAYING TEXT & TOAST MESSAGES
# ═══════════════════════════════════════

## 1A. Show a Toast (popup message)

```java
// Short toast (2 seconds)
Toast.makeText(this, "Hello World!", Toast.LENGTH_SHORT).show();

// Long toast (3.5 seconds)
Toast.makeText(this, "Hello World!", Toast.LENGTH_LONG).show();

// Toast with variable
String name = "Sam";
Toast.makeText(this, "Hello " + name + "!", Toast.LENGTH_SHORT).show();
```

## 1B. Show what user typed in a Toast

```xml
<!-- activity_main.xml -->
<EditText
    android:id="@+id/etName"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:hint="Enter your name"/>

<Button
    android:id="@+id/btnShow"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Show"/>
```

```java
// MainActivity.java — inside onCreate()
EditText etName = findViewById(R.id.etName);
Button btnShow = findViewById(R.id.btnShow);

btnShow.setOnClickListener(view -> {
    String name = etName.getText().toString().trim();
    if (name.isEmpty()) {
        Toast.makeText(this, "Please enter a name!", Toast.LENGTH_SHORT).show();
    } else {
        Toast.makeText(this, "You entered: " + name, Toast.LENGTH_SHORT).show();
    }
});
```

## 1C. Update a TextView dynamically

```java
TextView tvResult = findViewById(R.id.tvResult);
tvResult.setText("New text here");

// Change color
tvResult.setTextColor(Color.RED);

// Change size
tvResult.setTextSize(24);
```

---

# ═══════════════════════════════════════
# PART 2: PASSING DATA BETWEEN ACTIVITIES (INTENTS)
# ═══════════════════════════════════════

## THE GOLDEN RULE: Intent = Messenger between screens

## 2A. Go to next screen AND send data

### Activity 1 (Sender):
```java
// Step 1: Create intent pointing to SecondActivity
Intent intent = new Intent(MainActivity.this, SecondActivity.class);

// Step 2: Pack data into intent (key → value)
intent.putExtra("NAME_KEY", "Sam");
intent.putExtra("AGE_KEY", 20);
intent.putExtra("EMAIL_KEY", etEmail.getText().toString());

// Step 3: Launch it
startActivity(intent);
```

### Activity 2 (Receiver):
```java
// Inside SecondActivity.java onCreate()

// Step 1: Get the intent that launched this activity
Intent intent = getIntent();

// Step 2: Unpack data using same keys
String name = intent.getStringExtra("NAME_KEY");
int age = intent.getIntExtra("AGE_KEY", 0); // 0 = default
String email = intent.getStringExtra("EMAIL_KEY");

// Step 3: Display it
TextView tvName = findViewById(R.id.tvName);
tvName.setText("Name: " + name);

// Or show in toast
Toast.makeText(this, "Welcome " + name, Toast.LENGTH_SHORT).show();
```

## 2B. Go back to previous activity (Back Button)
```java
Button btnBack = findViewById(R.id.btnBack);
btnBack.setOnClickListener(view -> {
    finish(); // This closes current activity and goes back
});
```

## 2C. Common exam pattern — Form in Activity 1, Display in Activity 2

### Activity 1 XML:
```xml
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <EditText android:id="@+id/etName"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Name"/>

    <EditText android:id="@+id/etAge"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Age"
        android:inputType="number"/>

    <Button android:id="@+id/btnSubmit"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Submit"/>
</LinearLayout>
```

### Activity 1 Java:
```java
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        EditText etName = findViewById(R.id.etName);
        EditText etAge = findViewById(R.id.etAge);
        Button btnSubmit = findViewById(R.id.btnSubmit);

        btnSubmit.setOnClickListener(view -> {
            String name = etName.getText().toString().trim();
            String age = etAge.getText().toString().trim();

            // ALWAYS validate!
            if (name.isEmpty() || age.isEmpty()) {
                Toast.makeText(this, "Fill all fields!", Toast.LENGTH_SHORT).show();
                return;
            }

            Intent intent = new Intent(MainActivity.this, SecondActivity.class);
            intent.putExtra("NAME", name);
            intent.putExtra("AGE", age);
            startActivity(intent);
        });
    }
}
```

### Activity 2 Java:
```java
public class SecondActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_second);

        String name = getIntent().getStringExtra("NAME");
        String age = getIntent().getStringExtra("AGE");

        TextView tvDisplay = findViewById(R.id.tvDisplay);
        tvDisplay.setText("Name: " + name + "\nAge: " + age);

        Button btnBack = findViewById(R.id.btnBack);
        btnBack.setOnClickListener(v -> finish());
    }
}
```

---

# ═══════════════════════════════════════
# PART 3: ALL INPUT CONTROLS (BUTTONS, CHECKBOX, RADIO, TOGGLE, SEEKBAR, SWITCH)
# ═══════════════════════════════════════

## 3A. Button
```xml
<Button
    android:id="@+id/btn"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Click Me"/>
```
```java
Button btn = findViewById(R.id.btn);
btn.setOnClickListener(view -> {
    // your action here
    Toast.makeText(this, "Clicked!", Toast.LENGTH_SHORT).show();
});
```

## 3B. CheckBox (Multiple Selections)
```xml
<CheckBox android:id="@+id/cbPizza"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Pizza - ₹200"/>

<CheckBox android:id="@+id/cbBurger"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Burger - ₹150"/>
```
```java
CheckBox cbPizza = findViewById(R.id.cbPizza);
CheckBox cbBurger = findViewById(R.id.cbBurger);

btnSubmit.setOnClickListener(view -> {
    String order = "";
    int total = 0;
    if (cbPizza.isChecked()) { order += "Pizza "; total += 200; }
    if (cbBurger.isChecked()) { order += "Burger "; total += 150; }
    Toast.makeText(this, "Order: " + order + "\nTotal: ₹" + total, Toast.LENGTH_LONG).show();
});

// Disable checkboxes after submit (exam trick!)
btnSubmit.setOnClickListener(view -> {
    cbPizza.setEnabled(false);
    cbBurger.setEnabled(false);
});
```

## 3C. RadioButton (Only ONE selection allowed)
```xml
<RadioGroup android:id="@+id/rg"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content">

    <RadioButton android:id="@+id/rbMale"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Male"/>

    <RadioButton android:id="@+id/rbFemale"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Female"/>
</RadioGroup>
```
```java
RadioGroup rg = findViewById(R.id.rg);

btnSubmit.setOnClickListener(view -> {
    int selectedId = rg.getCheckedRadioButtonId();
    if (selectedId == -1) {
        Toast.makeText(this, "Select an option!", Toast.LENGTH_SHORT).show();
        return;
    }
    RadioButton selected = findViewById(selectedId);
    String gender = selected.getText().toString();
    Toast.makeText(this, "Gender: " + gender, Toast.LENGTH_SHORT).show();
});
```

## 3D. ToggleButton
```xml
<ToggleButton android:id="@+id/tb"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textOn="WiFi ON"
    android:textOff="WiFi OFF"/>
```
```java
ToggleButton tb = findViewById(R.id.tb);
tb.setOnCheckedChangeListener((buttonView, isChecked) -> {
    if (isChecked) {
        Toast.makeText(this, "WiFi Turned ON", Toast.LENGTH_SHORT).show();
        // show wifi image: imageView.setImageResource(R.drawable.wifi_on);
    } else {
        Toast.makeText(this, "WiFi Turned OFF", Toast.LENGTH_SHORT).show();
    }
});
```

## 3E. Switch
```xml
<Switch android:id="@+id/sw"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Dark Mode"/>
```
```java
Switch sw = findViewById(R.id.sw);
sw.setOnCheckedChangeListener((buttonView, isChecked) -> {
    if (isChecked) {
        Toast.makeText(this, "Dark Mode ON", Toast.LENGTH_SHORT).show();
    } else {
        Toast.makeText(this, "Dark Mode OFF", Toast.LENGTH_SHORT).show();
    }
});
```

## 3F. SeekBar (Slider)
```xml
<SeekBar android:id="@+id/sb"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:max="100"/>

<TextView android:id="@+id/tvProgress"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="0"/>
```
```java
SeekBar sb = findViewById(R.id.sb);
TextView tvProgress = findViewById(R.id.tvProgress);
sb.setOnSeekBarChangeListener(new SeekBar.OnSeekBarChangeListener() {
    @Override
    public void onProgressChanged(SeekBar seekBar, int progress, boolean fromUser) {
        tvProgress.setText("Value: " + progress);
    }
    @Override public void onStartTrackingTouch(SeekBar seekBar) {}
    @Override public void onStopTrackingTouch(SeekBar seekBar) {}
});
```

---

# ═══════════════════════════════════════
# PART 4: SPINNER (DROPDOWN MENU)
# ═══════════════════════════════════════

## 4A. Basic Spinner from Array

```xml
<Spinner android:id="@+id/spinner"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"/>
```

```java
Spinner spinner = findViewById(R.id.spinner);

// Option 1: From string array
String[] items = {"Car", "Bike", "Bus", "Auto"};
ArrayAdapter<String> adapter = new ArrayAdapter<>(this,
    android.R.layout.simple_spinner_item, items);
adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);
spinner.setAdapter(adapter);

// Get selected item
spinner.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
    @Override
    public void onItemSelected(AdapterView<?> parent, View view, int position, long id) {
        String selected = parent.getItemAtPosition(position).toString();
        Toast.makeText(MainActivity.this, "Selected: " + selected, Toast.LENGTH_SHORT).show();
    }
    @Override
    public void onNothingSelected(AdapterView<?> parent) {}
});

// Get selected item on button click
btnSubmit.setOnClickListener(view -> {
    String selected = spinner.getSelectedItem().toString();
    Toast.makeText(this, "You chose: " + selected, Toast.LENGTH_SHORT).show();
});
```

## 4B. Spinner from strings.xml (cleaner approach)

```xml
<!-- res/values/strings.xml -->
<string-array name="vehicle_types">
    <item>Car</item>
    <item>Bike</item>
    <item>Truck</item>
</string-array>
```
```xml
<!-- In layout XML -->
<Spinner android:id="@+id/spinner"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:entries="@array/vehicle_types"/>
```

---

# ═══════════════════════════════════════
# PART 5: DATE PICKER & TIME PICKER
# ═══════════════════════════════════════

## 5A. DatePicker Dialog (shows calendar popup)

```xml
<Button android:id="@+id/btnDate"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Pick Date"/>

<TextView android:id="@+id/tvDate"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="No date selected"/>
```

```java
Button btnDate = findViewById(R.id.btnDate);
TextView tvDate = findViewById(R.id.tvDate);

btnDate.setOnClickListener(view -> {
    // Get today's date as default
    final Calendar cal = Calendar.getInstance();
    int year = cal.get(Calendar.YEAR);
    int month = cal.get(Calendar.MONTH);
    int day = cal.get(Calendar.DAY_OF_MONTH);

    DatePickerDialog dialog = new DatePickerDialog(this,
        (datePicker, y, m, d) -> {
            String date = d + "/" + (m + 1) + "/" + y;  // m+1 because months start at 0
            tvDate.setText("Date: " + date);
        }, year, month, day);
    dialog.show();
});
```

## 5B. TimePicker Dialog

```java
Button btnTime = findViewById(R.id.btnTime);
TextView tvTime = findViewById(R.id.tvTime);

btnTime.setOnClickListener(view -> {
    final Calendar cal = Calendar.getInstance();
    int hour = cal.get(Calendar.HOUR_OF_DAY);
    int minute = cal.get(Calendar.MINUTE);

    TimePickerDialog dialog = new TimePickerDialog(this,
        (timePicker, h, min) -> {
            String time = h + ":" + String.format("%02d", min);  // formats minutes like 09 not 9
            tvTime.setText("Time: " + time);
        }, hour, minute, true); // true = 24-hour format
    dialog.show();
});
```

## 5C. Both Date + Time (exam combo)
```java
// Store date and time in variables
String[] selectedDate = {""};
String[] selectedTime = {""};

btnDate.setOnClickListener(v -> {
    Calendar cal = Calendar.getInstance();
    new DatePickerDialog(this, (dp, y, m, d) -> {
        selectedDate[0] = d + "/" + (m+1) + "/" + y;
        tvDate.setText(selectedDate[0]);
    }, cal.get(Calendar.YEAR), cal.get(Calendar.MONTH), cal.get(Calendar.DAY_OF_MONTH)).show();
});

btnSubmit.setOnClickListener(v -> {
    Toast.makeText(this, "Date: " + selectedDate[0] + " Time: " + selectedTime[0], Toast.LENGTH_LONG).show();
});
```

---

# ═══════════════════════════════════════
# PART 6: LISTVIEW
# ═══════════════════════════════════════

## 6A. Basic ListView with click handling

```xml
<ListView android:id="@+id/listView"
    android:layout_width="match_parent"
    android:layout_height="match_parent"/>
```

```java
ListView listView = findViewById(R.id.listView);
String[] sports = {"Cricket", "Football", "Basketball", "Tennis", "Hockey"};

ArrayAdapter<String> adapter = new ArrayAdapter<>(this,
    android.R.layout.simple_list_item_1, sports);
listView.setAdapter(adapter);

// Click on item
listView.setOnItemClickListener((parent, view, position, id) -> {
    String selected = sports[position];
    Toast.makeText(this, "You selected: " + selected, Toast.LENGTH_SHORT).show();
});

// Long click on item
listView.setOnItemLongClickListener((parent, view, position, id) -> {
    Toast.makeText(this, "Long clicked: " + sports[position], Toast.LENGTH_SHORT).show();
    return true; // must return true
});
```

## 6B. ListView with dynamic data (ArrayList)

```java
ArrayList<String> taskList = new ArrayList<>();
taskList.add("Buy groceries");
taskList.add("Complete assignment");

ArrayAdapter<String> adapter = new ArrayAdapter<>(this,
    android.R.layout.simple_list_item_1, taskList);
listView.setAdapter(adapter);

// Add item dynamically
btnAdd.setOnClickListener(v -> {
    String newTask = etTask.getText().toString().trim();
    taskList.add(newTask);
    adapter.notifyDataSetChanged(); // IMPORTANT: refresh the list
    etTask.setText("");
});
```

---

# ═══════════════════════════════════════
# PART 7: GRIDVIEW
# ═══════════════════════════════════════

```xml
<GridView android:id="@+id/gridView"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:numColumns="3"
    android:columnWidth="100dp"
    android:horizontalSpacing="10dp"
    android:verticalSpacing="10dp"/>
```

```java
GridView gridView = findViewById(R.id.gridView);
String[] items = {"A", "B", "C", "D", "E", "F", "G", "H", "I"};

ArrayAdapter<String> adapter = new ArrayAdapter<>(this,
    android.R.layout.simple_list_item_1, items);
gridView.setAdapter(adapter);

gridView.setOnItemClickListener((parent, view, position, id) -> {
    Toast.makeText(this, "Clicked: " + items[position], Toast.LENGTH_SHORT).show();
});
```

---

# ═══════════════════════════════════════
# PART 8: TABLAYOUT WITH VIEWPAGER (TAB NAVIGATION)
# ═══════════════════════════════════════

## Step 1: Add dependency in build.gradle (Module)
```gradle
dependencies {
    implementation 'com.google.android.material:material:1.9.0'
}
```

## Step 2: activity_main.xml
```xml
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <com.google.android.material.tabs.TabLayout
        android:id="@+id/tabLayout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:tabMode="fixed"
        app:tabGravity="fill"/>

    <androidx.viewpager.widget.ViewPager
        android:id="@+id/viewPager"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
</LinearLayout>
```

## Step 3: Create a Fragment (e.g., TabFragment.java)
```java
public class TabFragment extends Fragment {
    private String content;

    public TabFragment(String content) {
        this.content = content;
    }

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment_tab, container, false);
        TextView tv = view.findViewById(R.id.tvContent);
        tv.setText(content);
        return view;
    }
}
```

## Step 4: fragment_tab.xml
```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center">

    <TextView android:id="@+id/tvContent"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="24sp"/>
</LinearLayout>
```

## Step 5: MainActivity.java
```java
TabLayout tabLayout = findViewById(R.id.tabLayout);
ViewPager viewPager = findViewById(R.id.viewPager);

viewPager.setAdapter(new FragmentPagerAdapter(getSupportFragmentManager(),
        FragmentPagerAdapter.BEHAVIOR_RESUME_ONLY_CURRENT_FRAGMENT) {
    @Override
    public Fragment getItem(int position) {
        switch (position) {
            case 0: return new TabFragment("Top Stories content here");
            case 1: return new TabFragment("Sports content here");
            default: return new TabFragment("Entertainment content here");
        }
    }
    @Override
    public int getCount() { return 3; }
    @Override
    public CharSequence getPageTitle(int position) {
        switch (position) {
            case 0: return "Top Stories";
            case 1: return "Sports";
            default: return "Entertainment";
        }
    }
});

tabLayout.setupWithViewPager(viewPager);
```

---

# ═══════════════════════════════════════
# PART 9: OPTIONS MENU
# ═══════════════════════════════════════

## Step 1: Create res/menu/menu_main.xml
(Right click res → New → Android Resource Directory → type: menu → then New Menu Resource File)

```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <item
        android:id="@+id/menuSearch"
        android:title="Search"
        android:icon="@android:drawable/ic_menu_search"
        app:showAsAction="ifRoom"/>

    <item
        android:id="@+id/menuSettings"
        android:title="Settings"
        app:showAsAction="never"/>

    <item
        android:id="@+id/menuAbout"
        android:title="About Us"
        app:showAsAction="never"/>
</menu>
```

## Step 2: MainActivity.java
```java
// Add these two methods to your activity (outside onCreate)

@Override
public boolean onCreateOptionsMenu(Menu menu) {
    getMenuInflater().inflate(R.menu.menu_main, menu);
    return true;
}

@Override
public boolean onOptionsItemSelected(MenuItem item) {
    switch (item.getItemId()) {
        case R.id.menuSearch:
            Toast.makeText(this, "Search clicked", Toast.LENGTH_SHORT).show();
            return true;
        case R.id.menuSettings:
            // Open settings activity
            startActivity(new Intent(this, SettingsActivity.class));
            return true;
        case R.id.menuAbout:
            Toast.makeText(this, "About Us page", Toast.LENGTH_SHORT).show();
            return true;
        default:
            return super.onOptionsItemSelected(item);
    }
}
```

---

# ═══════════════════════════════════════
# PART 10: CONTEXT MENU (Long Press Menu)
# ═══════════════════════════════════════

## Step 1: Create res/menu/context_menu.xml
```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/ctxEdit" android:title="Edit"/>
    <item android:id="@+id/ctxDelete" android:title="Delete"/>
    <item android:id="@+id/ctxShare" android:title="Share"/>
</menu>
```

## Step 2: MainActivity.java
```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    TextView tvItem = findViewById(R.id.tvItem);
    registerForContextMenu(tvItem); // Register view for long-press menu
}

@Override
public void onCreateContextMenu(ContextMenu menu, View v, ContextMenu.ContextMenuInfo menuInfo) {
    super.onCreateContextMenu(menu, v, menuInfo);
    getMenuInflater().inflate(R.menu.context_menu, menu);
    menu.setHeaderTitle("Choose Action");
}

@Override
public boolean onContextItemSelected(MenuItem item) {
    switch (item.getItemId()) {
        case R.id.ctxEdit:
            Toast.makeText(this, "Edit selected", Toast.LENGTH_SHORT).show();
            return true;
        case R.id.ctxDelete:
            Toast.makeText(this, "Delete selected", Toast.LENGTH_SHORT).show();
            return true;
        default:
            return super.onContextItemSelected(item);
    }
}
```

---

# ═══════════════════════════════════════
# PART 11: POPUP MENU (Button triggers floating menu)
# ═══════════════════════════════════════

## Step 1: Create res/menu/popup_menu.xml
```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/popOption1" android:title="Option 1"/>
    <item android:id="@+id/popOption2" android:title="Option 2"/>
    <item android:id="@+id/popOption3" android:title="Option 3"/>
</menu>
```

## Step 2: In your Activity
```java
Button btnMenu = findViewById(R.id.btnMenu);
btnMenu.setOnClickListener(view -> {
    PopupMenu popup = new PopupMenu(MainActivity.this, view);
    popup.getMenuInflater().inflate(R.menu.popup_menu, popup.getMenu());

    popup.setOnMenuItemClickListener(item -> {
        switch (item.getItemId()) {
            case R.id.popOption1:
                Toast.makeText(this, "Option 1 selected", Toast.LENGTH_SHORT).show();
                return true;
            case R.id.popOption2:
                Toast.makeText(this, "Option 2 selected", Toast.LENGTH_SHORT).show();
                return true;
            default:
                return false;
        }
    });
    popup.show();
});
```

---

# ═══════════════════════════════════════
# PART 12: SQLITE DATABASE (CRUD Operations)
# ═══════════════════════════════════════

## THE PATTERN: 3 files needed
1. `DatabaseHelper.java` — manages the database
2. XML layout — form for input
3. `MainActivity.java` — connects UI to database

## Step 1: DatabaseHelper.java (Copy-paste and modify table name/columns)

```java
import android.content.ContentValues;
import android.content.Context;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;

public class DatabaseHelper extends SQLiteOpenHelper {

    private static final String DATABASE_NAME = "AppDB.db";
    private static final int DATABASE_VERSION = 1;
    private static final String TABLE_NAME = "tasks";  // Change this

    public DatabaseHelper(Context context) {
        super(context, DATABASE_NAME, null, DATABASE_VERSION);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        // CREATE your table here
        String createTable = "CREATE TABLE " + TABLE_NAME + " (" +
                "id INTEGER PRIMARY KEY AUTOINCREMENT, " +
                "name TEXT, " +
                "priority TEXT, " +
                "duedate TEXT)";
        db.execSQL(createTable);
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        db.execSQL("DROP TABLE IF EXISTS " + TABLE_NAME);
        onCreate(db);
    }

    // ─── INSERT ───
    public boolean insertData(String name, String priority, String duedate) {
        SQLiteDatabase db = this.getWritableDatabase();
        ContentValues values = new ContentValues();
        values.put("name", name);
        values.put("priority", priority);
        values.put("duedate", duedate);
        long result = db.insert(TABLE_NAME, null, values);
        return result != -1; // returns true if success
    }

    // ─── READ ALL ───
    public Cursor getAllData() {
        SQLiteDatabase db = this.getReadableDatabase();
        return db.rawQuery("SELECT * FROM " + TABLE_NAME, null);
    }

    // ─── UPDATE ───
    public boolean updateData(String id, String name, String priority) {
        SQLiteDatabase db = this.getWritableDatabase();
        ContentValues values = new ContentValues();
        values.put("name", name);
        values.put("priority", priority);
        int result = db.update(TABLE_NAME, values, "id=?", new String[]{id});
        return result > 0;
    }

    // ─── DELETE ───
    public boolean deleteData(String id) {
        SQLiteDatabase db = this.getWritableDatabase();
        int result = db.delete(TABLE_NAME, "id=?", new String[]{id});
        return result > 0;
    }
}
```

## Step 2: Using DatabaseHelper in MainActivity

```java
DatabaseHelper db = new DatabaseHelper(this);

// INSERT
btnSave.setOnClickListener(view -> {
    String name = etName.getText().toString().trim();
    String priority = spinner.getSelectedItem().toString();
    String date = tvDate.getText().toString();

    if (name.isEmpty()) {
        Toast.makeText(this, "Enter task name!", Toast.LENGTH_SHORT).show();
        return;
    }

    boolean success = db.insertData(name, priority, date);
    if (success) {
        Toast.makeText(this, "Task saved!", Toast.LENGTH_SHORT).show();
    } else {
        Toast.makeText(this, "Error saving!", Toast.LENGTH_SHORT).show();
    }
});

// READ and display in ListView
btnView.setOnClickListener(view -> {
    Cursor cursor = db.getAllData();
    ArrayList<String> list = new ArrayList<>();
    
    if (cursor.getCount() == 0) {
        Toast.makeText(this, "No data found!", Toast.LENGTH_SHORT).show();
        return;
    }
    
    while (cursor.moveToNext()) {
        String row = "Name: " + cursor.getString(1) +
                     " | Priority: " + cursor.getString(2) +
                     " | Date: " + cursor.getString(3);
        list.add(row);
    }
    
    ArrayAdapter<String> adapter = new ArrayAdapter<>(this,
        android.R.layout.simple_list_item_1, list);
    listView.setAdapter(adapter);
});
```

---

# ═══════════════════════════════════════
# PART 13: SHARED PREFERENCES (Save small data permanently)
# ═══════════════════════════════════════

## Use when: save login state, user name, settings, theme, last opened state

```java
// SAVE data
SharedPreferences prefs = getSharedPreferences("MyApp", MODE_PRIVATE);
SharedPreferences.Editor editor = prefs.edit();
editor.putString("username", "John");
editor.putBoolean("isLoggedIn", true);
editor.putInt("age", 25);
editor.apply(); // ALWAYS call apply() to save!

// READ data
SharedPreferences prefs = getSharedPreferences("MyApp", MODE_PRIVATE);
String username = prefs.getString("username", "Guest"); // "Guest" = default
boolean loggedIn = prefs.getBoolean("isLoggedIn", false);
int age = prefs.getInt("age", 0);

// DELETE one key
editor.remove("username");
editor.apply();

// DELETE everything
editor.clear();
editor.apply();
```

## Exam Pattern: Remember login state

```java
// In onCreate — check if already logged in
SharedPreferences prefs = getSharedPreferences("MyApp", MODE_PRIVATE);
boolean isLoggedIn = prefs.getBoolean("isLoggedIn", false);
if (isLoggedIn) {
    startActivity(new Intent(this, HomeActivity.class));
    finish(); // go directly to home, skip login
}

// On login button click
btnLogin.setOnClickListener(view -> {
    String user = etUser.getText().toString();
    SharedPreferences.Editor editor = prefs.edit();
    editor.putString("username", user);
    editor.putBoolean("isLoggedIn", true);
    editor.apply();
    startActivity(new Intent(this, HomeActivity.class));
    finish();
});

// On logout button
btnLogout.setOnClickListener(view -> {
    prefs.edit().clear().apply();
    startActivity(new Intent(this, MainActivity.class));
    finish();
});
```

---

# ═══════════════════════════════════════
# PART 14: ACTIVITY LIFECYCLE (Understanding & Logging)
# ═══════════════════════════════════════

```java
public class MainActivity extends AppCompatActivity {
    private static final String TAG = "LifecycleDemo";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Log.d(TAG, "onCreate called");
        Toast.makeText(this, "onCreate", Toast.LENGTH_SHORT).show();
    }

    @Override
    protected void onStart() {
        super.onStart();
        Log.d(TAG, "onStart called");
        Toast.makeText(this, "onStart", Toast.LENGTH_SHORT).show();
    }

    @Override
    protected void onResume() {
        super.onResume();
        Log.d(TAG, "onResume called");
    }

    @Override
    protected void onPause() {
        super.onPause();
        Log.d(TAG, "onPause called");
    }

    @Override
    protected void onStop() {
        super.onStop();
        Log.d(TAG, "onStop called");
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        Log.d(TAG, "onDestroy called");
    }
}
```

### Lifecycle order:
```
App opens:      onCreate → onStart → onResume
User presses Home: onPause → onStop
User comes back:  onRestart → onStart → onResume  
User presses Back: onPause → onStop → onDestroy
```

---

# ═══════════════════════════════════════
# PART 15: DISPLAYING IMAGES (ImageView)
# ═══════════════════════════════════════

```xml
<ImageView
    android:id="@+id/imgView"
    android:layout_width="200dp"
    android:layout_height="200dp"
    android:scaleType="centerCrop"
    android:src="@drawable/my_image"/>
```

```java
ImageView imgView = findViewById(R.id.imgView);

// Change image from code
imgView.setImageResource(R.drawable.another_image);

// Show/Hide image
imgView.setVisibility(View.VISIBLE);
imgView.setVisibility(View.GONE);

// Change image based on toggle
ToggleButton tb = findViewById(R.id.tb);
tb.setOnCheckedChangeListener((btn, isChecked) -> {
    if (isChecked) {
        imgView.setImageResource(R.drawable.wifi_on);
    } else {
        imgView.setImageResource(R.drawable.wifi_off);
    }
});
```

> **NOTE:** Put image files in `res/drawable/` folder. Name must be lowercase, no spaces. Example: `wifi_on.png`

---

# ═══════════════════════════════════════
# PART 16: CALCULATOR APP PATTERN
# ═══════════════════════════════════════

```xml
<!-- activity_main.xml -->
<LinearLayout ... android:orientation="vertical" android:padding="16dp">
    <EditText android:id="@+id/etNum1" android:hint="Enter number 1" android:inputType="numberDecimal"/>
    <EditText android:id="@+id/etNum2" android:hint="Enter number 2" android:inputType="numberDecimal"/>
    <LinearLayout android:orientation="horizontal">
        <Button android:id="@+id/btnAdd" android:text="+" android:layout_weight="1" android:layout_width="0dp" android:layout_height="wrap_content"/>
        <Button android:id="@+id/btnSub" android:text="-" android:layout_weight="1" android:layout_width="0dp" android:layout_height="wrap_content"/>
        <Button android:id="@+id/btnMul" android:text="*" android:layout_weight="1" android:layout_width="0dp" android:layout_height="wrap_content"/>
        <Button android:id="@+id/btnDiv" android:text="/" android:layout_weight="1" android:layout_width="0dp" android:layout_height="wrap_content"/>
    </LinearLayout>
</LinearLayout>
```

```java
EditText etNum1 = findViewById(R.id.etNum1);
EditText etNum2 = findViewById(R.id.etNum2);

View.OnClickListener calcListener = view -> {
    if (etNum1.getText().toString().isEmpty() || etNum2.getText().toString().isEmpty()) {
        Toast.makeText(this, "Enter both numbers!", Toast.LENGTH_SHORT).show();
        return;
    }
    double n1 = Double.parseDouble(etNum1.getText().toString());
    double n2 = Double.parseDouble(etNum2.getText().toString());
    double result = 0;
    String operator = "";

    if (view.getId() == R.id.btnAdd) { result = n1 + n2; operator = "+"; }
    else if (view.getId() == R.id.btnSub) { result = n1 - n2; operator = "-"; }
    else if (view.getId() == R.id.btnMul) { result = n1 * n2; operator = "*"; }
    else if (view.getId() == R.id.btnDiv) {
        if (n2 == 0) { Toast.makeText(this, "Cannot divide by zero!", Toast.LENGTH_SHORT).show(); return; }
        result = n1 / n2; operator = "/";
    }

    // Pass result to next activity
    Intent intent = new Intent(this, ResultActivity.class);
    intent.putExtra("EXPRESSION", n1 + " " + operator + " " + n2 + " = " + result);
    startActivity(intent);
};

findViewById(R.id.btnAdd).setOnClickListener(calcListener);
findViewById(R.id.btnSub).setOnClickListener(calcListener);
findViewById(R.id.btnMul).setOnClickListener(calcListener);
findViewById(R.id.btnDiv).setOnClickListener(calcListener);
```

---

# ═══════════════════════════════════════
# PART 17: IMPORTANT IMPORTS (Add at top of every Java file)
# ═══════════════════════════════════════

```java
// Most common — add ALL of these to any activity
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.*;
import android.widget.AdapterView;
import android.view.Menu;
import android.view.MenuInflater;
import android.view.MenuItem;
import android.app.DatePickerDialog;
import android.app.TimePickerDialog;
import android.content.ContentValues;
import android.content.SharedPreferences;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;
import android.util.Log;
import android.graphics.Color;
import androidx.appcompat.app.AppCompatActivity;
import androidx.fragment.app.Fragment;
import androidx.fragment.app.FragmentPagerAdapter;
import com.google.android.material.tabs.TabLayout;
import java.util.ArrayList;
import java.util.Calendar;
```

> **TIP:** Android Studio auto-suggests imports. Press `Alt+Enter` on red-underlined code to import.

---

# ═══════════════════════════════════════
# PART 18: COMMON XML LAYOUT ATTRIBUTES CHEATSHEET
# ═══════════════════════════════════════

```
android:layout_width="match_parent"   → fills full screen width
android:layout_width="wrap_content"   → only as wide as content
android:layout_height="match_parent"  → fills full screen height
android:layout_height="wrap_content"  → only as tall as content
android:orientation="vertical"         → LinearLayout stacks DOWN
android:orientation="horizontal"       → LinearLayout stacks SIDEWAYS
android:padding="16dp"                → space INSIDE the view
android:margin="8dp"                  → space OUTSIDE the view
android:gravity="center"              → centers content INSIDE the view
android:layout_gravity="center"       → centers the view in its parent
android:textSize="18sp"               → text size (always use sp)
android:textStyle="bold"              → makes text bold
android:hint="Enter name..."          → placeholder text in EditText
android:inputType="number"            → number keyboard on EditText
android:inputType="textPassword"      → password field (hidden text)
android:inputType="textEmailAddress"  → email keyboard
android:visibility="visible"          → show view
android:visibility="gone"             → hide view (removes space too)
android:visibility="invisible"        → hide view (keeps space)
android:background="#FF0000"          → set background color (red)
android:textColor="#FFFFFF"           → white text
android:layout_weight="1"            → equal distribution in LinearLayout
```

---

# ═══════════════════════════════════════
# PART 19: HARDER EXAM QUESTIONS — SOLVED APPROACHES
# ═══════════════════════════════════════

## Q: "Food Ordering App — checkboxes, calculate total, send to new screen, disable after submit"

### Approach:
1. Create checkboxes with food items
2. On submit: loop through checkboxes, calculate total
3. Send total + selected items via Intent to ResultActivity
4. Disable all checkboxes after submit

```java
String[] foods = {"Pizza-200", "Burger-150", "Pasta-120"};
int[] prices = {200, 150, 120};
CheckBox[] checkBoxes = new CheckBox[3];

// In onCreate, create checkboxes dynamically
LinearLayout layout = findViewById(R.id.llFoods);
for (int i = 0; i < foods.length; i++) {
    CheckBox cb = new CheckBox(this);
    cb.setText(foods[i]);
    cb.setTag(prices[i]);
    checkBoxes[i] = cb;
    layout.addView(cb);
}

btnOrder.setOnClickListener(view -> {
    StringBuilder selected = new StringBuilder();
    int total = 0;
    for (CheckBox cb : checkBoxes) {
        if (cb.isChecked()) {
            selected.append(cb.getText()).append("\n");
            total += (int) cb.getTag();
            cb.setEnabled(false); // disable after submit
        }
    }
    if (selected.length() == 0) {
        Toast.makeText(this, "Select at least one item!", Toast.LENGTH_SHORT).show();
        return;
    }
    btnOrder.setEnabled(false); // also disable button
    Intent intent = new Intent(this, OrderSummaryActivity.class);
    intent.putExtra("ITEMS", selected.toString());
    intent.putExtra("TOTAL", total);
    startActivity(intent);
});
```

---

## Q: "Vehicle Parking Registration — Spinner + TextFields + submit shows details + confirm/edit option"

### Approach:
1. Spinner for vehicle type
2. EditTexts for vehicle number and RC number
3. Submit goes to new activity
4. New activity shows details with Confirm and Edit buttons
5. Confirm shows toast with serial number (use random number)

```java
// In ResultActivity.java
btnConfirm.setOnClickListener(view -> {
    int serialNo = (int)(Math.random() * 90000) + 10000; // 5-digit random number
    Toast.makeText(this, "Parking confirmed! Serial No: " + serialNo, Toast.LENGTH_LONG).show();
});

btnEdit.setOnClickListener(view -> {
    finish(); // go back to form
});
```

---

## Q: "Movie Ticket Booking — Spinner + DatePicker + TimePicker + ToggleButton + Validation"

### Approach:
1. Spinners for movie and theatre
2. Date picker for show date
3. Time picker for show time
4. ToggleButton for Standard/Premium
5. If Premium: check if time > 12:00 PM to enable Book Now button

```java
ToggleButton tbTicket = findViewById(R.id.tbTicket);
Button btnBook = findViewById(R.id.btnBook);

tbTicket.setOnCheckedChangeListener((btn, isChecked) -> {
    if (isChecked) { // Premium selected
        // Check current selected time
        btnBook.setEnabled(selectedHour >= 12);
        if (selectedHour < 12) {
            Toast.makeText(this, "Premium only after 12:00 PM!", Toast.LENGTH_SHORT).show();
        }
    } else {
        btnBook.setEnabled(true); // Standard always enabled
    }
});
```

---

## Q: "Task Manager — SQLite with ListView, edit and delete"

### Approach for edit/delete from ListView:
```java
// Long press on list item → show dialog with Edit/Delete options
listView.setOnItemLongClickListener((parent, view, position, id) -> {
    Cursor cursor = db.getAllData();
    cursor.moveToPosition(position);
    String taskId = cursor.getString(0); // id column
    String taskName = cursor.getString(1);

    // Show dialog
    AlertDialog.Builder builder = new AlertDialog.Builder(this);
    builder.setTitle("Choose action for: " + taskName);
    builder.setPositiveButton("Delete", (dialog, which) -> {
        db.deleteData(taskId);
        Toast.makeText(this, "Deleted!", Toast.LENGTH_SHORT).show();
        loadListView(); // refresh list
    });
    builder.setNegativeButton("Edit", (dialog, which) -> {
        Intent intent = new Intent(this, EditActivity.class);
        intent.putExtra("ID", taskId);
        intent.putExtra("NAME", taskName);
        startActivity(intent);
    });
    builder.show();
    return true;
});
```

---

## Q: "Travel Ticket Booking — Source + Destination Spinners, Date, Toggle One-Way/Round Trip"

```java
// Get data from all controls
String source = spinnerSource.getSelectedItem().toString();
String destination = spinnerDest.getSelectedItem().toString();
String date = tvDate.getText().toString();
String tripType = tbTrip.isChecked() ? "Round Trip" : "One Way";

// Validate
if (source.equals(destination)) {
    Toast.makeText(this, "Source and destination cannot be same!", Toast.LENGTH_SHORT).show();
    return;
}

// Send to next screen
Intent intent = new Intent(this, TicketActivity.class);
intent.putExtra("SOURCE", source);
intent.putExtra("DESTINATION", destination);
intent.putExtra("DATE", date);
intent.putExtra("TYPE", tripType);
startActivity(intent);

// Reset button
btnReset.setOnClickListener(view -> {
    spinnerSource.setSelection(0);
    spinnerDest.setSelection(0);
    tvDate.setText("Select date");
    tbTrip.setChecked(false);
});
```

---

# ═══════════════════════════════════════
# PART 20: ALERTDIALOG (Popup confirmation box)
# ═══════════════════════════════════════

```java
AlertDialog.Builder builder = new AlertDialog.Builder(this);
builder.setTitle("Confirm");
builder.setMessage("Are you sure you want to delete?");
builder.setPositiveButton("Yes", (dialog, which) -> {
    // Do the action
    Toast.makeText(this, "Deleted!", Toast.LENGTH_SHORT).show();
});
builder.setNegativeButton("No", (dialog, which) -> {
    dialog.dismiss(); // just close
});
builder.show();
```

---

# ═══════════════════════════════════════
# PART 21: OPENING URL / EMAIL / CALL (Implicit Intents)
# ═══════════════════════════════════════

```java
// Open a URL in browser
String url = etUrl.getText().toString().trim();
if (!url.startsWith("http://") && !url.startsWith("https://")) {
    url = "https://" + url;
}
Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse(url));
startActivity(intent);

// Make a phone call
Intent callIntent = new Intent(Intent.ACTION_DIAL, Uri.parse("tel:9876543210"));
startActivity(callIntent);

// Send email
Intent emailIntent = new Intent(Intent.ACTION_SEND);
emailIntent.setType("message/rfc822");
emailIntent.putExtra(Intent.EXTRA_EMAIL, new String[]{"test@example.com"});
emailIntent.putExtra(Intent.EXTRA_SUBJECT, "Subject here");
emailIntent.putExtra(Intent.EXTRA_TEXT, "Body here");
startActivity(Intent.createChooser(emailIntent, "Choose email app"));

// Share text
Intent shareIntent = new Intent(Intent.ACTION_SEND);
shareIntent.setType("text/plain");
shareIntent.putExtra(Intent.EXTRA_TEXT, "Check this out!");
startActivity(Intent.createChooser(shareIntent, "Share via"));
```

---

# ═══════════════════════════════════════
# PART 22: TOOLBAR / APP BAR SETUP
# ═══════════════════════════════════════

## Step 1: In styles.xml (res/values/themes.xml), change theme to NoActionBar:
```xml
<style name="Theme.YourApp" parent="Theme.MaterialComponents.DayNight.NoActionBar">
```

## Step 2: Add Toolbar to XML
```xml
<androidx.appcompat.widget.Toolbar
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    android:elevation="4dp"
    app:title="My App"
    app:titleTextColor="@android:color/white"/>
```

## Step 3: Set toolbar in Java
```java
Toolbar toolbar = findViewById(R.id.toolbar);
setSupportActionBar(toolbar);
// Optional: show back button
getSupportActionBar().setDisplayHomeAsUpEnabled(true);
```

---

# ═══════════════════════════════════════
# PART 23: MAKING SECOND ACTIVITY (Step by Step)
# ═══════════════════════════════════════

1. In Android Studio: **File → New → Activity → Empty Views Activity**
   - Name: `SecondActivity`
   - Layout: `activity_second` (auto-created)
   - It auto-adds to AndroidManifest.xml ✅

2. Design `activity_second.xml` with your display views (TextViews, etc.)

3. In `SecondActivity.java`:
```java
public class SecondActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_second);

        // Get data from previous activity
        Intent intent = getIntent();
        String data = intent.getStringExtra("KEY");

        // Display it
        TextView tv = findViewById(R.id.tvData);
        tv.setText(data);

        // Back button
        Button btnBack = findViewById(R.id.btnBack);
        btnBack.setOnClickListener(v -> finish());
    }
}
```

---

# ═══════════════════════════════════════
# PART 24: QUICK REFERENCE — DATA TYPES IN INTENTS
# ═══════════════════════════════════════

```java
// SENDING:
intent.putExtra("key", "hello");       // String
intent.putExtra("key", 42);           // int
intent.putExtra("key", 3.14);         // double
intent.putExtra("key", true);         // boolean
intent.putExtra("key", new String[]{"a","b"}); // String array

// RECEIVING:
String s = intent.getStringExtra("key");
int i = intent.getIntExtra("key", 0);         // 0 = default
double d = intent.getDoubleExtra("key", 0.0);
boolean b = intent.getBooleanExtra("key", false);
String[] arr = intent.getStringArrayExtra("key");
```

---

# ═══════════════════════════════════════
# PART 25: EXAM DAY CHECKLIST ✅
# ═══════════════════════════════════════

Before submitting your app, check:

- [ ] All activities declared in AndroidManifest.xml?
- [ ] All button click listeners set?
- [ ] Input validation done (empty check)?
- [ ] Toast messages showing?
- [ ] Back button working (finish())?
- [ ] Data passing correctly between activities?
- [ ] Layout looks correct (not overlapping)?
- [ ] Database table created properly?
- [ ] adapter.notifyDataSetChanged() called after list update?
- [ ] editor.apply() called after SharedPreferences edit?
- [ ] All imports added? (Alt+Enter to auto-import)
- [ ] No crash when field is empty (null check)?

---

# ═══════════════════════════════════════
# BONUS: COMMON ERRORS & FIXES
# ═══════════════════════════════════════

| Error | Cause | Fix |
|-------|-------|-----|
| `NullPointerException` | findViewById with wrong ID | Check XML id matches Java code |
| App crashes on button click | Missing null check on EditText | Always `.trim()` and check `.isEmpty()` |
| List not updating | Forgot to notify adapter | Call `adapter.notifyDataSetChanged()` |
| Second activity not found | Not declared in Manifest | Add `<activity android:name=".SecondActivity"/>` |
| Cannot resolve symbol 'R' | Build error | Clean project: Build → Clean Project |
| Database not saving | Forgot `editor.apply()` | Always call `.apply()` after SharedPreferences |
| Month shows 0 instead of 1 | Calendar.MONTH starts at 0 | Always use `month + 1` when displaying |
| Time shows 9 instead of 09 | No formatting | Use `String.format("%02d", minute)` |
| `ClassCastException` on spinner | Wrong cast | Use `.toString()` on getSelectedItem() |

---

*This guide covers ICT 3268 — Mobile Application Development Lab (MAD) — MIT Manipal 2025*
*All code is in Java for Android Studio. Verified against the official lab manual.*
