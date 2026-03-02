---
layout: post
title: הנדסה לאחור של אפליקצייה זדונית
---

מטבע הדברים, אני מצרף בפוסט קבצים זדוניים. שימו לב והזהרו!






אתמול, ישראלים רבים קיבלו את ההודעה הזו:
כניסה לקישור תוריד קובץ APK בשם RedAlert.apk שנראה כמו האפליקציה הרגילה של "צבע אדום".
כמובן שזה חשוד (עדכון אמור להגיע פשוט דרך גוגל פליי).
הקובץ זמין כאן.

[aiv-dev.com/files/RedAlert_Malmware.apk](aiv-dev.com/files/RedAlert_Malmware.apk)

<img src="https://aiv-dev.com/he-IL/assets/images/RedAlertVirus1.png" width="50%" />

*ירוט טיל איראני במבצע "עם כלביא" על רקע הזריחה*
בדיקה ראשונית שלו תראה ששם החבילה הוא
com.red.alertx
ולא
com.red.alert - שם החבילה של האפליקציה האמיתית.

כמו כן, כשנפתח את האפליקציה כשאפליקציית "ה-Activity הנוכחי" פועלת, יוצג לנו גם שם האקטיביטי הראשי.


<img src="https://aiv-dev.com/he-IL/assets/images/RedAlertVirus2.png" width="50%" />
 
זה לא ActivityMain או משהו, אלא משהו מסורבל... בתוך תת נתיב בתוך האפליקציה בשמות שלא מעידים על זה. כבר קצת חשוד.
פתיחה עם JADX גילתה שהאפליקצייה כתובה בקוטלין - בעוד "צבע אדום" המקורית כתובה בJAVA.

[https://github.com/eladnava/redalert-android](https://github.com/eladnava/redalert-android)

(ואגב, שם האקטיביטי הראשי הוא אקטיביטי עם שם רגיל).

בפתיחה עם MT Manager נראה המון הבדלים, אבל הלכתי ישר לתיקיית הassets. הקובץ הזדוני מכיל שם קובץ שלא קיים במקור, בשם umgdn. בדיקה קצרה (אם כי ספציפית ראיתי שמישהו כתב את זה, אבל בכל מקרה הייתי בודק את הקובץ הזה) מראה שזהו קובץ APK. ובכן, הנה הוא.

[aiv-dev.com/files/umgdn.apk](aiv-dev.com/files/umgdn.apk)
 
קובץ הumgdn הוא עם אותו שם חבילה של הקובץ הזדוני המקורי (com.red.alertx) אך משום מה חתום עם מפתח אחר (וזה מוזר, הרי התוקף פיתח את שני הקבצים).

אז חיפשתי בSMALI את המילה umgdn - ומצאתי אותה במקום אחד, עם שם מוזר - בהחלט מריח כמו קוד שעבר אובספיקציה.

<img src="https://aiv-dev.com/he-IL/assets/images/RedAlertVirus3.png" width="50%" />

אז פתחתי את זה עם JADX במחשב.
וכאן כבר מתחיל להיות מעניין...
הקוד, באופן די מפתיע, לא מתקין את הumgdn. הוא ערמומי בהרבה! הוא מכניס אותו לתיקיית הנתונים של האפליקציה, ומעכשיו טוען קוד ממנו ולא מהAPK המותקן. ככה שאפילו לא מותקן APK אחר. מתוחכם מאוד.
ו... גורם לכך שהאפליקציה תזוהה כמותקנת מגוגל פליי.

חוץ מכל מה שקורה עד לכאן, האפליקציה הזו מבקשת הרשאות לSMS וכדומה - מה שהאפליקציה המקורית לא מבקשת.

אז בואו נפתח את הAndroidManifest.xml של שתי האפליקציות להשוואת בקשת הרשאות:

בקובץ הזדוני:

```
    <!-- Control vibration -->
    <uses-permission android:name="android.permission.VIBRATE" />
    <!-- Have full network access -->
    <uses-permission android:name="android.permission.INTERNET" />
    <!-- Prevent phone from sleeping -->
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <!-- View network connections -->
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <!-- Run at startup -->
    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
    <!-- Allows applications to access coarse location information usually obtained through the network -->
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <!-- Allows applications to access fine location information obtained through GPS -->
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <!-- Show notifications -->
    <uses-permission android:name="android.permission.POST_NOTIFICATIONS" />
    <!-- This app can appear on top of other apps -->
    <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW" />
    <!-- Schedule precisely timed actions -->
    <uses-permission android:name="android.permission.SCHEDULE_EXACT_ALARM" />
    <!-- Run foreground service -->
    <uses-permission android:name="android.permission.FOREGROUND_SERVICE" />
    <!-- Run foreground service with the type "location" -->
    <uses-permission android:name="android.permission.FOREGROUND_SERVICE_LOCATION" />
    <!-- Run foreground service with the type "remoteMessaging" -->
    <uses-permission android:name="android.permission.FOREGROUND_SERVICE_REMOTE_MESSAGING" />
    <!-- Find accounts on the device -->
    <uses-permission android:name="android.permission.GET_ACCOUNTS" />
    <!-- Read your text messages (SMS or MMS) -->
    <uses-permission android:name="android.permission.READ_SMS" />
    <!-- Read your contacts -->
    <uses-permission android:name="android.permission.READ_CONTACTS" />
    <!-- Query all packages -->
    <uses-permission android:name="android.permission.QUERY_ALL_PACKAGES" />
    <uses-feature
        android:name="android.hardware.location"
        android:required="false" />
    <uses-feature
        android:name="android.hardware.location.gps"
        android:required="false" />
    <uses-feature
        android:name="android.hardware.location.network"
        android:required="false" />
    <uses-feature
        android:glEsVersion="0x20000"
        android:required="true" />
    <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
```

בקובץ המקורי

```
    <!-- Control vibration -->
    <uses-permission android:name="android.permission.VIBRATE" />
    <!-- Have full network access -->
    <uses-permission android:name="android.permission.INTERNET" />
    <!-- Prevent phone from sleeping -->
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <!-- View network connections -->
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <!-- Run at startup -->
    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
    <!-- Allows applications to access coarse location information usually obtained through the network -->
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <!-- Allows applications to access fine location information obtained through GPS -->
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <!-- Show notifications -->
    <uses-permission android:name="android.permission.POST_NOTIFICATIONS" />
    <!-- This app can appear on top of other apps -->
    <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW" />
    <!-- Schedule precisely timed actions -->
    <uses-permission android:name="android.permission.SCHEDULE_EXACT_ALARM" />
    <!-- Run foreground service -->
    <uses-permission android:name="android.permission.FOREGROUND_SERVICE" />
    <!-- Run foreground service with the type "location" -->
    <uses-permission android:name="android.permission.FOREGROUND_SERVICE_LOCATION" />
    <!-- Run foreground service with the type "remoteMessaging" -->
    <uses-permission android:name="android.permission.FOREGROUND_SERVICE_REMOTE_MESSAGING" />
    <uses-feature
        android:name="android.hardware.location"
        android:required="false" />
    <uses-feature
        android:name="android.hardware.location.gps"
        android:required="false" />
    <uses-feature
        android:name="android.hardware.location.network"
        android:required="false" />
    <uses-feature
        android:glEsVersion="0x20000"
        android:required="true" />
    <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" /
```

ובכן, 4 הרשאות נוספו בקוד הזדוני. סמס ואנש"ק - שראינו לבד שנוסף, כי אנדרואיד שואל אם לתת - ועוד שתי הרשאות "שקטות".

```
android.permission.GET_ACCOUNTS
```

מאפשר גישה לרשימת החשבונות במכשיר (Google, WhatsApp וכו’).

ו

```
android.permission.QUERY_ALL_PACKAGES
```

שמאפשר לדעת אילו אפליקציות מותקנות במכשיר.


יש לציין, ששתי האפליקציות הזדוניות (זו שיורדת מהלינק והumgdn) נראות מבחינת ממשק וכו' חיקוי מושלם ל"צבע אדום" המקורית, ובמבט מלמעלה גם די זהות אחת לשניה. למעשה, לא ממש הבנתי את הצורך במהלך המתוחכם מאוד שנעשה פה עם הAPK הנוסף - אחרי שגרמת למשתמש להתקין אפליקציה, שנראית לגיטימית לחלוטין, הוא כבר בידיים שלך, ואת זיופי החתימות ומקור ההתקנה הוא גם ככה עושה...

צריך עוד הרבה מחקר...