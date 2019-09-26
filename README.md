# AndroidQ-DarkTheme

안드로이드 Q의 새로운 피처인 다크테마 적용 방법에 대하여 알아보겠습니다.

시스템에서 다크테마 적용은 설정 - 디스플레이 메뉴에서 적용이 가능합니다.

앱에서는,

1. style - theme의 parent를 Theme.AppCompat.DayNight 로 수정

    <style name="AppTheme" parent="Theme.AppCompat.DayNight">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
    </style>


2. theme의 parent를 DayNight 로 설정하지 못하는 경우에는 android:forceDarkAllowed 속성 추가

    <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
        <item name="android:forceDarkAllowed">true</item>
    </style>


3. 코드 레벨에서 강제로 ON/OFF 

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        findViewById<View>(R.id.change_mode_btn).setOnClickListener { toggleNightMode() }
        renderModeState()
    }

    fun renderModeState() {
        var output: TextView = findViewById(R.id.output_tv)
        val currentNightMode = resources.configuration.uiMode and Configuration.UI_MODE_NIGHT_MASK
        if (currentNightMode == Configuration.UI_MODE_NIGHT_NO) {
            output.text = "NIGHT_MODE_OFF"
        } else if (currentNightMode == Configuration.UI_MODE_NIGHT_YES) {
            output.text = "NIGHT_MODE_ON"
        }
    }

    fun toggleNightMode() {
        val currentNightMode = resources.configuration.uiMode and Configuration.UI_MODE_NIGHT_MASK
        if (currentNightMode == Configuration.UI_MODE_NIGHT_NO) {
            AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_YES)
        } else if (currentNightMode == Configuration.UI_MODE_NIGHT_YES) {
            AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_NO)
        }
    }
}

configuration에서 uiMode 값의 마스킹을 통해서 현재 앱이 night mode가 적용되어있는지 아닌지를 읽을 수 있습니다.
public static final int UI_MODE_NIGHT_NO = 16;
public static final int UI_MODE_NIGHT_YES = 32;

이 값에 따라서 나이트 모드를 적용할 것인지는 AppCompatDelegate.setDefaultNightMode api를 사용하면 적용이 가능합니다.
