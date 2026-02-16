# Lesson 10: Product Database

## Before You Start: Sync Your Fork

Because you are working from a fork, make sure your fork (and your Codespace) are up to date.

1. In the GitHub website, open **your fork** of this repo.
2. Click **Sync fork** (or **Fetch upstream**) and complete the prompts.
3. In your Codespace terminal (inside VS Code), pull the latest changes:

```bash
git pull
```

---

## Setup (2 Terminals)

1. Open **two terminals** in VS Code.

2. In **Terminal 1 (Client)**, run:

```bash
cd /workspaces/c2c-full-stack-102/lesson-10/app/client
```

If needed, create `client/.env`:

```bash
cat > .env <<'EOF'
REACT_APP_API_BASE_URL=http://localhost:3001
EOF
```

Start the client:

```bash
npm install
npm start
```

This launches the React app on **http://localhost:3000**.

3. In **Terminal 2 (Server)**, run:

```bash
cd /workspaces/c2c-full-stack-102/lesson-10/app/server
```

If needed, create `server/.env` (update `DB_PASSWORD` and `DB_NAME` to match your MySQL setup):

```bash
cat > .env <<'EOF'
# Environment configuration for the Express server
DB_HOST=127.0.0.1
DB_USER=root
DB_PASSWORD=password
DB_NAME=ecommerce
PORT=3001
EOF
```

Start the server:

```bash
npm install
npm run dev
```

This runs the Express server with `nodemon` on **http://localhost:3001**.

---

## Overview

In this lesson, you will restructure your app to use a product database instead of hardcoded product data. You will:

- Create and connect a MySQL product database
- Set up an API endpoint to retrieve product data
- Display products dynamically on your shopping page

---

## Step 1: Review Your Product Structure

1. Open your ecommerce project in the IDE.
2. Locate the code for your shopping page (e.g., `Shopping.js`).
3. Consider:
   - How are products currently displayed?
   - Where is the product information coming from?
   - How could a database improve product management?

---

## Step 2: Create the Product Database Table

**Using the cweijan database-client extension:**

1. Open the database client in VS Code.
2. Connect to your MySQL server.
3. Run the following SQL to create a `products` table:

```sql
CREATE TABLE products (
	id INT AUTO_INCREMENT PRIMARY KEY,
	name VARCHAR(255),
	description VARCHAR(255),
	price DECIMAL(10,2),
	image_url VARCHAR(255)
);
```

4. Insert a few example products:

```sql
INSERT INTO products (name, description, price, image_url) VALUES
('Product 1', 'Description for product 1', 19.99, 'https://via.placeholder.com/150'),
('Product 2', 'Description for product 2', 29.99, 'https://via.placeholder.com/150');
```

---

## Step 3: Set Up the API Endpoint

1. In your server code (e.g., `server/index.js`), add the following endpoint:

```js
app.get("/api/ecommerce/products", (req, res) => {
  const sql = "SELECT * FROM products";
  db.query(sql, (err, result) => {
    if (err) {
      console.log(err);
      return;
    }
    res.setHeader("Content-Type", "application/json");
    res.json(result);
  });
});
```

2. Restart your server if needed.
3. Test the endpoint (e.g., visit `http://localhost:3001/api/ecommerce/products` in your browser or use a REST client).

---

## Step 4: Fetch and Display Products in the Client

1. In your shopping page component (e.g., `Shopping.js`):
   - Import `useState`, `useEffect` from React and `axios`.
2. Set up state and fetch logic:

```js
const [products, setProducts] = useState([]);

useEffect(() => {
  axios
    .get("http://localhost:3001/api/ecommerce/products")
    .then((res) => {
      setProducts(res.data);
    })
    .catch((err) => console.log(err));
}, []);
```

3. Render the products:

```js
<div id="shopping">
  {products.map((product) => (
    <div key={product.id} id="product">
      <img src={product.image_url} alt="" />
      <h2>{product.name}</h2>
      <h3>{product.description}</h3>
      <h3>{product.price}</h3>
      <button onClick={() => addToCart(product)}>Add to Cart</button>
    </div>
  ))}
</div>
```

---

## Step 5: Style and Add More Products

1. Use your CSS file (e.g., `shopping.css`) to organize product cards into a grid or another layout you prefer.
2. Add more products to your database using the database client and SQL `INSERT` statements.

---

## Exit Ticket (Review)

1. What is the primary purpose of creating a product database in this lesson?
   - To replace hardcoded product data with a more dynamic solution
2. Which SQL statement is used to retrieve all product data from the product database?
   - `SELECT * FROM products`
3. When setting up an API endpoint to respond to GET requests, what should the response header be set to?
   - `Content-Type: application/json`
4. What is the purpose of the `useEffect` hook in this lesson?
   - To fetch product data from the server using Axios

---

## Vocabulary

- **Map method**: A function that creates a new array by using a function on every object in the array
- **Axios**: A JavaScript library used to make HTTP requests from node.js
- **Hard-coded**: Data coded directly into the source code, rather than pulling it from a separate source
- **API Endpoint**: A digital point where an API receives requests
