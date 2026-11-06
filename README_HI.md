# APEX SANKALP COMPETITIVE CLASSES — Online Quiz Portal

यह मोबाइल-फ्रेंडली शुरुआती पोर्टल है। इसमें Student Entry (नाम + मोबाइल, OTP नहीं), अलग-अलग परीक्षा श्रेणियाँ, प्रश्न-टाइमर, +1/−0.25 अंक, 10 प्रश्न के बाद प्रोग्रेस स्कोर, फाइनल रिजल्ट, उत्तर समीक्षा और लीडरबोर्ड शामिल हैं। Admin पेज से Firebase पर नए टेस्ट अपलोड किए जा सकते हैं।

## 1) अभी क्या काम करता है?
- Firebase कॉन्फ़िगरेशन के बिना Demo Mode चलता है। विद्यार्थी के डिवाइस पर नाम और टेस्ट रिजल्ट लोकल सेव होते हैं।
- साझा ऑनलाइन टेस्ट, Admin से टेस्ट प्रकाशित करना, और सभी विद्यार्थियों का साझा Leaderboard/Result देखने के लिए Firebase सेटअप जरूरी है।
- Demo प्रश्न केवल फीचर जाँचने के लिए हैं; इन्हें परीक्षा-प्रमाणित प्रश्नपत्र न मानें।

## 2) Firebase सेटअप (एक बार)
1. https://console.firebase.google.com/ खोलकर नया प्रोजेक्ट बनाएँ: `apex-sankalp-portal` (या उपलब्ध कोई नाम)।
2. Project Overview में Web App (`</>`) जोड़ें। Firebase जो `firebaseConfig` देगा, उसे `firebase-config.js` में placeholders की जगह डालें।
3. Authentication → Sign-in method में **Anonymous** और **Email/Password** चालू करें। Anonymous विद्यार्थियों के लिए है; Email/Password Admin के लिए।
4. Firestore Database बनाएँ। शुरुआत में Production mode चुनें।
5. Authentication → Users → Add user से अपना Admin ईमेल/पासवर्ड बनाएँ। उस Admin user का UID कॉपी करें।
6. Firestore → Rules में `firestore.rules` की सामग्री पेस्ट करें और `REPLACE_WITH_ADMIN_UID` की जगह अपना वास्तविक Admin UID लिखकर Publish करें।
7. Firebase Authentication → Settings → Authorized domains में अपनी GitHub Pages domain जोड़ें, जैसे `YOUR_GITHUB_USERNAME.github.io` (यदि पहले से न हो)।

**सुरक्षा:** Firebase web config वेबसाइट में दिखती है; यह सामान्य है। असली सुरक्षा Firestore Rules से आती है। Admin UID को सही भरना बहुत जरूरी है। Admin पासवर्ड या निजी credentials कभी HTML/JS फाइल में न लिखें। नाम + मोबाइल लॉगिन OTP से सत्यापित नहीं है; विद्यार्थी द्वारा दिया गया नंबर अप्रमाणित रहेगा।

## 3) GitHub Pages पर प्रकाशित करना
1. GitHub में नया **Public Repository** बनाएँ, उदाहरण: `apex-sankalp-quiz`।
2. इस ZIP की सभी फाइलें repository की root directory में अपलोड करें (ZIP को ही root में रखने से वेबसाइट नहीं चलेगी; ZIP को extract करके फाइलें अपलोड करें)।
3. Repository → Settings → Pages → Build and deployment में `Deploy from a branch` चुनें। Branch `main`, folder `/(root)` चुनकर Save करें।
4. प्रकाशित पता सामान्यतः होगा: `https://YOUR_GITHUB_USERNAME.github.io/apex-sankalp-quiz/`

## 4) Admin से प्रश्न कैसे डालें?
1. प्रकाशित वेबसाइट के साथ `admin.html` खोलें।
2. Firebase में बनाए गए Admin ईमेल/पासवर्ड से लॉगिन करें।
3. टेस्ट का नाम, श्रेणी, समय और प्रश्न JSON में भरें।
4. JSON का उदाहरण:

```json
[
  {
    "q": "भारत की राजधानी क्या है?",
    "o": ["मुंबई", "नई दिल्ली", "कोलकाता", "चेन्नई"],
    "a": 1,
    "e": "नई दिल्ली भारत की राजधानी है।"
  }
]
```

`a` में सही विकल्प का क्रमांक दें: A=0, B=1, C=2, D=3। प्रकाशित (Published) चेक रहने पर विद्यार्थी टेस्ट सूची में देख पाएँगे।

## 5) जरूरी सीमाएँ और अगले सुधार
- इस शुरुआती build में मोबाइल नंबर OTP से verify नहीं होता। Anonymous Firebase पहचान ब्राउज़र/डिवाइस पर आधारित है; browser data साफ होने पर पहचान बदल सकती है।
- विद्यार्थी अपनी लोकल प्रोफाइल बदल सकते हैं। उच्च-नियंत्रण वाली परीक्षा के लिए verified login, attempt limits, server-side scoring, question randomization और anti-cheating नियंत्रण अलग से जोड़ने होंगे।
- Leaderboard में केवल display name और score दिखाएँ; मोबाइल नंबर सार्वजनिक न करें।
- प्रश्नपत्र प्रकाशित करने से पहले उत्तर, विकल्प, भाषा और व्याख्या की शिक्षक द्वारा जाँच अनिवार्य है।

## Files
- `index.html`, `styles.css`, `app.js` — विद्यार्थी पोर्टल
- `admin.html`, `admin.css`, `admin.js` — Admin Dashboard
- `firebase-config.js` — Firebase web app settings (आपको भरना है)
- `firestore.rules` — Database security rules (Admin UID भरना है)
