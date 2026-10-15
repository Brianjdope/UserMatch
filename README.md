# UserMatch

A travel-buddy matching app: users create a profile with their interests, travel
spots, hobbies, and location, and the API suggests nearby people with similar
interests and age.

Built as a team project with Albert and Caren Chung.

> **Status: backend prototype.** The Express/MongoDB API is implemented; the
> React frontend in `usermatch/src` is still the Create React App starter page.

## How matching works

`GET /possiblematches` returns other users who:

- share **at least one interest**
- are within **±5 years** of age
- have the **same working-out preference**
- are within **100 km** (MongoDB `$near` geospatial query)

## API

Server runs on `http://localhost:3000` and uses the MongoDB database `travelplans`.

| Method | Endpoint | Purpose |
|---|---|---|
| `POST` | `/login` | Check username + password |
| `POST` | `/userprofile` | Create or update a profile (name, age, interests, travel_spots, hobbies, working_out, location) |
| `POST` | `/user/:id/location` | Update a user's latitude/longitude |
| `GET` | `/interests?username=` | Users sharing at least one interest |
| `GET` | `/possiblematches?username=&latitude=&longitude=` | Full match query (above) |

## Tech stack

- Node.js, Express, Mongoose, MongoDB
- React (Create React App) — scaffold only

## Run locally

Requires Node.js and a local MongoDB running on the default port.

```bash
cd usermatch/backend
npm install
npm start          # API on http://localhost:3000

# in another terminal (frontend scaffold)
cd usermatch
npm install
npm start
```

## Known issues

- Passwords are stored and compared in **plain text** — fine for a class demo, must be hashed (e.g. bcrypt) before real use.
- The `2dsphere` index on `location` is commented out in `backend.js`, which the `$near` query in `/possiblematches` needs.
- `backend.js` passes the whole `require('./userSchema')` export object to `mongoose.model` instead of its `userSchema` property.
- Frontend not built yet.

## Project structure

```text
usermatch/
  backend/
    backend.js      Express server and endpoints
    userSchema.js   Mongoose user schema (profile + GeoJSON location)
  src/              React starter app
```
