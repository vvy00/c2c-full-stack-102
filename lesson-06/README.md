# Lesson 6 — Back End Setup

This lesson has two parts:

- **API**: set up an Express server (port **3001**) + Nodemon.
- **MySQL**: use VS Code Database Client to create a schema + table.

We are **not** connecting the Express server to MySQL yet (that comes after).

### Copy last class ui code to lesson 6 directory

```bash
cd /workspaces/c2c-full-stack-102/lesson-05/app
cp -r /workspaces/c2c-full-stack-102/lesson-05/app/client .
```

## API

### 1) Start the client (so we avoid port conflicts)

In one terminal, make sure your React client is running (it should use port 3000):

```bash
cd /workspaces/c2c-full-stack-102/lesson-06/app/client
npm install
npm start
```

Leave this running.

### 2) Initialize the server folder

Open a **new** terminal and run:

```bash
cd /workspaces/c2c-full-stack-102/lesson-06/app/server
npm init -y
```

This creates `package.json`.

### 3) Create `index.js`

Inside `/workspaces/c2c-full-stack-102/lesson-06/app/server/`, create a new file named `index.js`:

```bash
cd /workspaces/c2c-full-stack-102/lesson-06/app/server
touch index.js
```

### 4) Install dependencies

Still inside `/workspaces/c2c-full-stack-102/lesson-06/app/server/`:

```bash
npm install express body-parser mysql nodemon
```

### 5) Express server — step 1 (create the server)

In `index.js`, add this first chunk:

```js
const express = require("express");
const app = express();

app.listen(3001, () => {
  console.log("running on port 3001");
});
```

Run it:

```bash
node index.js
```

You should see: `running on port 3001`.

Now visit `http://localhost:3001`.

You will likely see: `Cannot GET /`.

### 6) Express server — step 2 (add a GET route)

In `index.js`, add this route **above** the `app.listen(...)`:

```js
app.get("/", (req, res) => {
  res.send("hello world");
});
```

Stop your server (`Ctrl+C`) and run it again:

```bash
node index.js
```

Refresh `http://localhost:3001` and you should see: `hello world`.

### 7) Nodemon setup (edit `package.json` scripts)

In `/workspaces/c2c-full-stack-102/lesson-06/app/server/package.json`, update/add the `scripts` section to include:

```json
"scripts": {
	"start": "node index.js",
	"devStart": "nodemon index.js"
}
```

### 8) Run the server with Nodemon (stop here)

Run:

```bash
npm run devStart
```

Confirm it works by changing your `res.send("hello world")` message, saving the file, and refreshing the browser.

Stop here for the API portion.

## MySQL

This dev container already runs a MySQL server for you (see `.devcontainer/docker-compose.yml`). You will create a schema and table using the **VS Code Database Client** extension.

Connection info (dev container defaults):

- Host: `127.0.0.1`
- Port: `3306`
- User: `root`
- Password: `password`

### 1) Create a MySQL connection in VS Code

1. Open the **Database Client** view in VS Code.
2. Click **Add Connection**.
3. Choose **MySQL**.
4. Enter the connection info above.
5. Test/Save the connection and connect.

### 2) Create a schema

In Database Client, open a new query window for your MySQL connection and run:

```sql
CREATE DATABASE IF NOT EXISTS example;
```

### 3) Create a table

Run the following (same query window is fine):

```sql
USE example;

CREATE TABLE IF NOT EXISTS pkm_info (
	id INT NOT NULL AUTO_INCREMENT,
	name VARCHAR(255) NOT NULL,
	type VARCHAR(255) NOT NULL,
	PRIMARY KEY (id)
);
```

### 4) Quick verification

Run:

```sql
USE lesson_06;
SHOW TABLES;
DESCRIBE pkm_info;
```

Stop here — we’ll connect the Express API to MySQL next.
