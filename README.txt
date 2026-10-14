SSL — SUNDAY SUPER LEAGUE, SEASON 2
Website package and Firebase shared-score setup

FILES
- index.html: Season 2 league website with Week 1 fixtures, admin sign-in, shared scores and automatic league table.
- README.txt: this setup guide.

WEEK 1 FIXTURES
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

IMPORTANT ABOUT THE FIXTURES
The current website includes the six Week 1 fixtures supplied. Since the full league format says each team plays 8 matches, add the remaining fixtures in a future update for full-season standings.

FIREBASE CONFIGURATION
The supplied Firebase project settings are already inserted in index.html:
- Project ID: sunday-super-league
- Authentication domain: sunday-super-league.firebaseapp.com
- Realtime Database: https://sunday-super-league-default-rtdb.asia-southeast1.firebasedatabase.app

You do not need to replace the firebaseConfig object unless the Firebase Console shows different values. The website now checks that the required config fields are present; it no longer incorrectly treats these real settings as unconfigured.

SET UP AUTHENTICATION AND DATABASE
1. Open https://console.firebase.google.com/ and select the sunday-super-league project.
2. Go to Build → Authentication → Get started → Sign-in method. Enable Email/Password.
3. Under Authentication → Users, create the scorekeeper account. Use its email and password in the website sign-in form. Keep the password private.
4. Go to Build → Realtime Database and confirm that the database exists at the URL listed above. If it does not exist, create it in the appropriate region.
5. In Authentication → Users, open the scorekeeper account and copy its exact UID.
6. In Realtime Database → Rules, use the rules below, replacing REPLACE_WITH_ADMIN_UID with the scorekeeper's actual UID:

{
  "rules": {
    "league": {
      ".read": true,
      ".write": "auth != null && auth.uid === 'ny8hc7DVDoZwqbuLQiSEa1RENiH2'"
    }
  }
}

7. Click Publish to save the database rules. Do not use public write rules. Anyone can read the public league scores, but only the selected authenticated UID can write to the league path.
8. Upload index.html to the root of your GitHub repository. Keep README.txt for reference; it does not need to be deployed for the site to work.
9. In GitHub, open Settings → Pages → Build and deployment → Deploy from a branch. Choose branch main and folder /(root), then save.
10. Open the GitHub Pages URL. Wait for the deployment to finish before testing.

HOW TO USE
- Open the Admin sign in section at the bottom of the website.
- Sign in using the account created in Firebase Authentication.
- When signed in, score boxes become editable.
- Enter both scores for a match and move focus away from the field; the result is saved to Realtime Database.
- Visitors should see the updated scores and league table when the database connection is working.
- Select Sign Out when finished.

TROUBLESHOOTING
- “Firebase not connected yet”: ensure you uploaded the updated index.html, not an older copy, and reload the page.
- “auth/invalid-credential”: check the email/password and confirm Email/Password sign-in is enabled.
- “Permission denied” or database connection errors: check that the database URL is correct and that the published rules contain the exact scorekeeper UID.
- If the website is opened directly as a local file and behaves inconsistently, test it through GitHub Pages or a local web server.
- Check the browser developer console for detailed Firebase errors.

SECURITY
Firebase web config values, including the API key, are normally visible in browser code. Database security must be enforced by Firebase Authentication and Realtime Database Rules. Never share the admin password and never publish open write rules.
