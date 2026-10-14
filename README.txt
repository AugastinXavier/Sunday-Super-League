SSL — SUNDAY SUPER LEAGUE, SEASON 2
Website package and shared score setup

CONTENTS
- index.html: Season 2 league site, Week 1 fixtures, rules, editable shared scores and automatic table.
- README.txt: setup guide.

FIXTURES (WEEK 1)
1. GOAT FC vs 18+ UNITED FC
2. CR7 UNITED FC vs STAW HAT FC
3. GOAT FC vs STAW HAT FC
4. CR7 UNITED FC vs 18+ UNITED FC
5. 18+ UNITED FC vs STAW HAT FC
6. GOAT FC vs CR7 UNITED FC

RULES SHOWN
- Each team plays 8 league-stage matches.
- Top 2 teams qualify directly for the finals.
- Each match lasts 15 minutes: 7 minutes play, 1 minute break, 7 minutes play.
- Side box-outs and corner rules will be followed.
- Top assist and top goal scorer awards will be calculated.

IMPORTANT ABOUT THE 8 MATCHES
The supplied poster gives six Week 1 fixtures, while each team plays 8 matches in the full league stage. This site currently includes exactly the six fixtures shown in the Week 1 poster. Add the remaining season fixtures in a later update to calculate full-season standings correctly.

SET UP SHARED LIVE SCORES
The site uses Firebase Realtime Database + Firebase Authentication. The HTML is ready for the connection, but it will not sync across devices until you configure a Firebase project.

1. Go to https://console.firebase.google.com/ and create a project.
2. Add a Web app in Project Overview and copy its config.
3. Go to Build -> Realtime Database -> Create Database. Choose a nearby region and start in locked mode. Copy the database URL.
4. Go to Build -> Authentication -> Get started -> Sign-in method. Enable Email/Password.
5. Under Authentication -> Users, create one admin user for the scorekeeper. Copy that user's UID.
6. In Realtime Database -> Rules, publish the rules below after replacing REPLACE_WITH_ADMIN_UID with the exact admin UID:

{
  "rules": {
    "league": {
      ".read": true,
      ".write": "auth != null && auth.uid === 'ny8hc7DVDoZwqbuLQiSEa1RENiH2'"
    }
  }
}

These rules allow anyone to read public league scores, but only the selected signed-in admin to write scores. Do not publish open write rules.

7. Open index.html in a text editor. Find the `firebaseConfig` object near the start of the script and replace all placeholder values with the exact values from Firebase:
   - apiKey
   - authDomain
   - databaseURL (use the exact URL shown in Realtime Database)
   - projectId
   - appId

8. Upload the configured index.html to the root of your GitHub repository.
9. In GitHub, go to Settings -> Pages -> Deploy from a branch -> main -> /(root) -> Save.
10. Open the GitHub Pages URL. Anyone can view live results; only the admin can sign in and edit scores.

HOW TO USE
- Admin signs in at the bottom of the site.
- Score boxes become editable.
- When a score is entered, the result and table sync for all visitors.
- Sign out when finished.
- Keep the admin credentials private.

SECURITY
The Firebase web API key is intended to be included in a client app. Security depends on Firebase Authentication and database rules, not on hiding the API key. Do not share admin passwords or use public write rules.
