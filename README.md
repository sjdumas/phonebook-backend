# Phonebook Backend

This is the phonebook backend exercise for [Part 3](https://fullstackopen.com/en/part3) of the University of Helsinki [Full Stack Open](https://fullstackopen.com/en/) course.

It is a REST API for phonebook entries, built with Node.js, Express, and MongoDB (with Mongoose). The server also serves a production build of the phonebook frontend from `dist/`.

The live link of the project is here: [Phonebook Backend](https://phonebook-backend-r69q.onrender.com/)

A later version of this app, with the frontend source, tests, and a CI/CD pipeline, is in [fso-phonebook](https://github.com/sjdumas/fso-phonebook).

## Project Structure

```text
├── dist/                 Production build of the phonebook frontend, served by the backend
├── models/person.js      Mongoose model, validation, and database connection
├── requests/             Example requests for the VS Code REST Client extension
├── index.js              Express app, API routes, and error handling
├── mongo.js              Command-line script from an early exercise (see below)
└── package.json          Scripts and dependencies
```

## Get Started

You need Node.js 20.19 or newer and a MongoDB database, for example a free [MongoDB Atlas](https://www.mongodb.com/atlas) cluster.

1. Clone the repository:

   ```bash
   git clone https://github.com/sjdumas/phonebook-backend.git
   cd phonebook-backend
   ```

2. Install dependencies:

   ```bash
   npm install
   ```

3. Create a `.env` file in the project root:

   ```env
   MONGODB_URI=mongodb+srv://<username>:<password>@<cluster>/<database>
   PORT=3001
   ```

   Never commit `.env`. It is listed in `.gitignore`.

4. Start the development server:

   ```bash
   npm run dev
   ```

The server runs at <http://localhost:3001>, or the port set in `PORT`. Opening that address shows the phonebook frontend.

`models/person.js` makes Node use Google's DNS servers (`8.8.8.8` and `8.8.4.4`) to look up `mongodb+srv://` addresses, as a workaround for a local network problem. If your network blocks outside DNS servers, remove those lines.

## Scripts

- `npm start` to start the server
- `npm run dev` to start the server with automatic restarts on file changes
- `npm run lint` to run ESLint

## API

| Method   | Endpoint           | Purpose                               |
| -------- | ------------------ | ------------------------------------- |
| `GET`    | `/api/persons`     | List phonebook entries                |
| `GET`    | `/api/persons/:id` | Get one entry                         |
| `POST`   | `/api/persons`     | Create an entry                       |
| `PUT`    | `/api/persons/:id` | Update an entry                       |
| `DELETE` | `/api/persons/:id` | Delete an entry                       |
| `GET`    | `/info`            | Show the entry count and current time |

Create and update requests use JSON with a `name` and `number`, for example:

```json
{
  "name": "Ada Lovelace",
  "number": "040-1234567"
}
```

Names are required and must be at least three characters. Phone numbers are required and must contain a two- or three-digit prefix, a hyphen, and digits; the complete number must be at least eight characters long. For example, `040-1234567` is valid, but `040-123-4567` is not, because it has a second hyphen.

### Responses and Errors

| Status | When                                                                                  |
| ------ | ------------------------------------------------------------------------------------- |
| `200`  | Successful `GET`, `POST`, or `PUT`. `POST` returns the new entry with its `id`        |
| `204`  | Successful `DELETE`. Deleting an id that does not exist also returns `204`            |
| `400`  | Missing `name` or `number` (`content missing`), a validation error, or a malformed id |
| `404`  | No entry with that id, or an unknown endpoint                                         |

## mongo.js

`mongo.js` is a script from an early exercise that reads and adds entries directly in the database:

```bash
node mongo.js <password>                              # list all entries
node mongo.js <password> "Ada Lovelace" 040-1234567   # add an entry
```

It does not read `.env`. The connection string, including the cluster and database user, is written in the file, so it only works with that cluster.
