# Autorisierung mit der Drinks API

Du brauchst Node.js 26, MongoDB und Postman.

1. Führe in `auth-server` und `api` jeweils `npm ci` aus.
2. Kopiere in beiden Ordnern `.env.example` nach `.env`. Trage unter `MONGO_URI` deine Verbindung zu MongoDB ein.
3. Erzeuge einen Schlüssel und trage denselben Wert in beiden `.env`-Dateien unter `ACCESS_TOKEN_SECRET` ein.

   ```bash
   node -p "require('node:crypto').randomBytes(32).toString('hex')"
   ```

4. Starte MongoDB, falls du eine lokale Instanz nutzt.
5. Führe in zwei Terminals jeweils `npm run dev` aus, einmal in `auth-server` und einmal in `api`.

| Dienst      | Adresse                 |
| ----------- | ----------------------- |
| Auth-Server | `http://127.0.0.1:3001` |
| Drinks-API  | `http://127.0.0.1:3000` |

Melde dich über `POST /auth/login` an oder registriere ein Konto über `POST /auth/register`. Beide Requests erwarten `email` und `password` als JSON. Verwende den `accessToken` aus der Antwort in Postman unter **Authorization → Bearer Token**.

| Request an die Drinks-API                      | Wer darf ihn ausführen? |
| ---------------------------------------------- | ----------------------- |
| `GET /drinks`, `GET /drinks/:id`               | Alle                    |
| `POST /drinks`                                 | Angemeldete Konten      |
| `PATCH /drinks/:id/menu`, `DELETE /drinks/:id` | Besitzer oder Admin     |

Lege zum Ausprobieren einen Drink mit `{ "name": "Demo-Drink" }` an. Verwende dessen `_id` für die weiteren Requests. Für PATCH sende `{ "onMenu": true }` oder `{ "onMenu": false }`. Zum Löschen muss der Drink von der Karte genommen sein. Ältere Drinks ohne Besitzer ID können nur Admins ändern oder löschen.
