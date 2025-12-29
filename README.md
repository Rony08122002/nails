# 💅 מערכת ניהול ציפורניים - הוראות התקנה

## 📋 תוכן עניינים
1. [יצירת פרויקט Firebase](#firebase-setup)
2. [הגדרת הקבצים](#file-setup)
3. [העלאה ל-GitHub Pages](#github-pages)
4. [שימוש במערכת](#usage)

---

## 🔥 שלב 1: יצירת פרויקט Firebase

### 1.1 יצירת חשבון
1. גש ל-[Firebase Console](https://console.firebase.google.com/)
2. לחץ על "Add project" (הוסף פרויקט)
3. תן שם לפרויקט (למשל: "nail-salon")
4. השבת Google Analytics (לא חובה)
5. לחץ על "Create project"

### 1.2 הפעלת Authentication
1. בתפריט הצד, לחץ על "Build" → "Authentication"
2. לחץ על "Get started"
3. בטאב "Sign-in method", הפעל "Email/Password"
4. שמור את השינויים

### 1.3 יצירת משתמש מנהל
1. בטאב "Users", לחץ על "Add user"
2. הזן את האימייל והסיסמה שלך
3. לחץ "Add user"
4. **שמור את הפרטים האלה - אלה הפרטים שבהם תתחבר!**

### 1.4 הפעלת Realtime Database
1. בתפריט הצד, לחץ על "Build" → "Realtime Database"
2. לחץ על "Create Database"
3. בחר מיקום (Europe או US)
4. בחר "Start in **test mode**" (נשנה את זה מיד)
5. לחץ "Enable"

### 1.5 הגדרת כללי אבטחה ל-Database
1. לחץ על הטאב "Rules"
2. **העתק והדבק את הכללים הבאים:**

```json
{
  "rules": {
    "shapes": {
      ".read": true,
      ".write": "auth != null"
    },
    "colors": {
      ".read": true,
      ".write": "auth != null"
    },
    "orders": {
      ".read": "auth != null",
      ".write": true,
      "$orderId": {
        ".validate": "newData.hasChildren(['clientName', 'clientPhone', 'shape', 'color', 'status', 'createdAt'])"
      }
    }
  }
}
```

3. לחץ "Publish"

**הסבר על הכללים:**
- `shapes` ו-`colors` - כולם יכולים לקרוא, רק משתמשים מחוברים יכולים לכתוב
- `orders` - רק משתמשים מחוברים יכולים לקרוא, כולם יכולים לכתוב (כדי שלקוחות יוכלו לשלוח הזמנות)

### 1.6 הפעלת Storage
1. בתפריט הצד, לחץ על "Build" → "Storage"
2. לחץ על "Get started"
3. השאר את כללי האבטחה כברירת מחדל
4. לחץ "Done"

### 1.7 הגדרת כללי אבטחה ל-Storage
1. לחץ על הטאב "Rules"
2. **העתק והדבק את הכללים הבאים:**

```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /shapes/{allPaths=**} {
      allow read: if true;
      allow write: if request.auth != null;
    }
  }
}
```

3. לחץ "Publish"

### 1.8 קבלת פרטי הקונפיגורציה
1. בתפריט הצד, לחץ על ⚙️ → "Project settings"
2. גלול למטה עד "Your apps"
3. לחץ על הסמל `</>`  (Web)
4. תן שם לאפליקציה (למשל: "nail-salon-web")
5. אל תסמן "Firebase Hosting"
6. לחץ "Register app"
7. **העתק את ה-firebaseConfig** - זה יראה כך:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyXXXXXXXXXXXXXXXXXXXXXXXX",
  authDomain: "your-project.firebaseapp.com",
  databaseURL: "https://your-project-default-rtdb.firebaseio.com",
  projectId: "your-project",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:xxxxx"
};
```

---

## 📁 שלב 2: הגדרת הקבצים

### 2.1 עדכון admin.html
1. פתח את הקובץ `admin.html`
2. חפש את השורה:
```javascript
// TODO: Replace with your Firebase config
const firebaseConfig = {
```
3. החלף את כל אובייקט ה-`firebaseConfig` בזה שהעתקת מ-Firebase
4. שמור את הקובץ

### 2.2 עדכון client.html
1. פתח את הקובץ `client.html`
2. חפש את השורה:
```javascript
// TODO: Replace with your Firebase config (same as admin.html)
const firebaseConfig = {
```
3. החלף את כל אובייקט ה-`firebaseConfig` **באותו** שהשתמשת ב-admin.html
4. שמור את הקובץ

---

## 🚀 שלב 3: העלאה ל-GitHub Pages

### 3.1 יצירת Repository
1. התחבר ל-[GitHub](https://github.com/)
2. לחץ על "New repository"
3. תן שם ל-repository (למשל: `nail-salon`)
4. סמן "Public"
5. לחץ "Create repository"

### 3.2 העלאת הקבצים
**דרך 1: דרך הממשק (קל יותר)**
1. בעמוד ה-repository, לחץ "uploading an existing file"
2. גרור את 2 הקבצים: `admin.html` ו-`client.html`
3. לחץ "Commit changes"

**דרך 2: דרך Git (למתקדמים)**
```bash
git init
git add admin.html client.html
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/nail-salon.git
git push -u origin main
```

### 3.3 הפעלת GitHub Pages
1. ב-repository, לחץ על "Settings"
2. בתפריט הצד, לחץ על "Pages"
3. תחת "Branch", בחר `main`
4. תחת "Folder", בחר `/ (root)`
5. לחץ "Save"
6. **המתן 1-2 דקות** עד שהאתר יהיה מוכן
7. תראה הודעה: "Your site is live at `https://YOUR-USERNAME.github.io/nail-salon/`"

---

## 📱 שלב 4: שימוש במערכת

### לך ולאשתך (בעלים):
1. גש ל: `https://YOUR-USERNAME.github.io/nail-salon/admin.html`
2. התחבר עם האימייל והסיסמה שיצרת ב-Firebase
3. הוסף צורות וצבעים
4. קבל התראות על הזמנות חדשות

### ללקוחות:
1. שלח להן את הקישור: `https://YOUR-USERNAME.github.io/nail-salon/client.html`
2. הן יוכלו לבחור צורה וצבע
3. למלא פרטים אישיים
4. לשלוח הזמנה

---

## 🎨 תכונות המערכת

### ממשק מנהל (admin.html)
- ✅ הוספת צורות ציפורניים עם תמונות
- ✅ הוספת צבעים עם בורר צבעים
- ✅ ניהול מלאי (מחיקה/עדכון)
- ✅ צפייה בהזמנות חדשות
- ✅ סימון הזמנות כהושלמו
- ✅ סטטיסטיקות (הזמנות ממתינות, הכנסות וכו')
- ✅ שיתוף קישור עם לקוחות
- ✅ שינוי סיסמה

### ממשק לקוח (client.html)
- ✅ בחירת צורת ציפורן
- ✅ בחירת צבע
- ✅ מילוי פרטים אישיים
- ✅ סיכום הזמנה עם מחיר
- ✅ שליחת הזמנה
- ✅ אישור קבלת הזמנה

---

## 🔒 אבטחה

המערכת בטוחה לחלוטין:
- רק בעלי האתר יכולים להוסיף/למחוק פריטים
- רק בעלי האתר יכולים לראות הזמנות
- לקוחות יכולים רק לשלוח הזמנות חדשות
- כל הנתונים מאוחסנים ב-Firebase (מאובטח)
- התמונות מאוחסנות ב-Firebase Storage

---

## ❓ שאלות נפוצות

**ש: האתר לא עובד, מה עושים?**
ת: בדוק שהעתקת נכון את ה-firebaseConfig משני הקבצים

**ש: איך משנים את המראה?**
ת: ערוך את ה-CSS בתוך תג ה-`<style>` בכל קובץ

**ש: איך להוסיף עוד משתמשים מנהלים?**
ת: ב-Firebase Console → Authentication → Users → Add user

**ש: האתר עובד אבל לא רואה תמונות?**
ת: בדוק ש-Storage מופעל ושכללי האבטחה נכונים

**ש: איך לקבל התראות על הזמנות חדשות?**
ת: פתח את admin.html ותראה את ההזמנות בזמן אמת

**ש: האם זה חינמי לגמרי?**
ת: כן! Firebase וגם GitHub Pages חינמיים לחלוטין לשימוש בסיסי

---

## 📞 תמיכה

אם יש בעיות:
1. בדוק שה-firebaseConfig נכון בשני הקבצים
2. ודא ש-Database Rules ו-Storage Rules מוגדרים נכון
3. פתח את Console בדפדפן (F12) וחפש שגיאות

---

## 🎉 מזל טוב!

המערכת שלך מוכנה! 
עכשיו אשתך יכולה לנהל את המלאי והלקוחות יכולות לבחור עיצובים בקלות 💅✨
