


Great! Below are **two complete, beginner-friendly, step-by-step project guides** you can fully build inside Azure:

1. **A To-Do Website + Azure Function API + Logic App Email Workflow**
    
2. **A Serverless API Backend (Azure Functions + Azure Storage + APIM)**
    

Each project includes:

✔ Architecture diagram (text-based)  
✔ What services you will create  
✔ Exact step-by-step instructions  
✔ Example code you can copy/paste  
✔ Deployment steps  
✔ Testing steps

---

# ✅ **PROJECT 1 — To-Do Website + Function API + Logic App Email Workflow**

This is perfect for beginners—simple, but touches Web Apps, Functions, Storage, and Logic Apps.

---

# 📐 **Architecture Overview**

```
User ↔ Azure Web App (HTML/JS Frontend)
             ↓ calls
      HTTP Azure Function API
             ↓ saves
       Azure Table Storage
             ↓ triggers
         Logic App → sends email confirmation
```

---

# 🧩 **You Will Create**

1. **Azure Table Storage** → Stores To-Do items
    
2. **Azure Function App** → API endpoints:
    
    - POST /todo → add a task
        
    - GET /todo → list all tasks
        
3. **Azure Web App** → simple HTML/JS UI that calls the Function API
    
4. **Logic App** → Sends an email when a new task is created
    

---

# 🛠 **Step-by-Step Implementation**

---

# **STEP 1 — Create an Azure Storage Account**

1. Portal → **Create Resource**
    
2. Search **Storage Account**
    
3. Create with:
    
    - Resource group: `rg-todo`
        
    - Name: `todostorage123`
        
4. After creation → open **Tables**
    
5. Create Table: **TodoItems**
    

---

# **STEP 2 — Create Your Function App**

1. Create Resource → search **Function App**
    
2. Configure:
    
    - Name: `todo-function-app-123`
        
    - Runtime: **Node.js** (easiest for beginners)
        
    - Hosting: Consumption plan
        
3. Create it.
    

---

# **STEP 3 — Create Two Functions (GET + POST)**

### **Create POST /todo (Add Item)**

1. Open Function App
    
2. Functions → **Add Function**
    
3. Choose **HTTP trigger**
    
4. Name: `AddTodo`
    
5. Authorization: **Function**
    

### Paste this code (**index.js**)

```javascript
const { TableClient, AzureSASCredential } = require("@azure/data-tables");

module.exports = async function (context, req) {
    const connectionString = process.env.STORAGE_CONN;
    const tableClient = TableClient.fromConnectionString(connectionString, "TodoItems");

    const item = {
        partitionKey: "todo",
        rowKey: new Date().getTime().toString(),
        title: req.body.title || "Untitled task"
    };

    await tableClient.createEntity(item);

    context.res = {
        status: 201,
        body: item
    };
};
```

### Add Application Setting

1. Function App → Configuration → Application Settings
    
2. Add:
    
    - Name: **STORAGE_CONN**
        
    - Value: Storage Account → Access Keys → Connection String
        
3. Save + restart.
    

---

### **Create GET /todo (List Items)**

Add another HTTP function:

```javascript
const { TableClient } = require("@azure/data-tables");

module.exports = async function (context, req) {
    const connectionString = process.env.STORAGE_CONN;
    const tableClient = TableClient.fromConnectionString(connectionString, "TodoItems");

    let items = [];
    for await (const item of tableClient.listEntities()) {
        items.push(item);
    }

    context.res = { body: items };
};
```

---

# **STEP 4 — Build the To-Do Website (HTML + JavaScript)**

Create a folder with:

### **index.html**

```html
<!DOCTYPE html>
<html>
<head>
  <title>Azure To-Do App</title>
</head>
<body>
  <h2>Add To-Do</h2>
  <input id="task" type="text" placeholder="Task name">
  <button onclick="addTodo()">Add</button>

  <h2>To-Do List</h2>
  <ul id="list"></ul>

<script>
const apiBase = "<YOUR FUNCTION APP URL>";

async function addTodo() {
  const title = document.getElementById("task").value;
  await fetch(apiBase + "/AddTodo", {
    method: "POST",
    headers: {"Content-Type": "application/json"},
    body: JSON.stringify({title})
  });
  loadTodos();
}

async function loadTodos() {
  const res = await fetch(apiBase + "/GetTodos");
  const items = await res.json();
  const list = document.getElementById("list");
  list.innerHTML = "";
  items.forEach(i => {
    list.innerHTML += `<li>${i.title}</li>`;
  })
}

loadTodos();
</script>
</body>
</html>
```

👉 Replace  
`<YOUR FUNCTION APP URL>`  
with your Function App’s base URL.

---

# **STEP 5 — Deploy To-Do Website to Azure Web App**

1. Create Resource → **Web App**
    
2. Runtime: **Static** or Node (doesn’t matter for pure HTML)
    
3. After creation:
    
    - Go to **Deployment Center → Upload ZIP**
        
    - ZIP your index.html
        
4. Browse the URL to see your To-Do app.
    

---

# **STEP 6 — Create Logic App to Send Email When a Task Is Added**

### 1. Create Logic App (Consumption)

2. Open **Logic App Designer**
    
3. Choose Trigger:  
    **“When an HTTP request is received”**  
    → Copy URL
    

---

### 4. Add an Action → “Send email”

Use Outlook or Gmail connector.  
Email body:

```
A new task was created:
@{triggerBody()?['title']}
```

Save the Logic App.

---

# **STEP 7 — Modify Function to Call Logic App**

Add this line inside the **AddTodo** function after saving the entity:

```javascript
await fetch(process.env.LOGICAPP_URL, {
  method: "POST",
  headers: {"Content-Type": "application/json"},
  body: JSON.stringify(item)
});
```

Add application setting:

- Name: **LOGICAPP_URL**
    

---

# 🎉 PROJECT 1 COMPLETE

You now have:

✔ A working To-Do Website  
✔ An Azure Function API  
✔ Storage-backed data  
✔ Logic App sending emails

---

# 🚀 **PROJECT 2 — Serverless API Backend**

A more advanced, scalable backend using:

- Azure Functions
    
- Azure API Management (APIM)
    
- Azure Storage
    
- Optional: Cosmos DB
    

---

# 📐 Architecture

```
Client → API Management → Azure Functions → Storage/Cosmos DB
```

---

# 🧩 You Will Build

1. **3 API endpoints**
    
    - GET /products
        
    - POST /products
        
    - DELETE /products/{id}
        
2. **Database**: Azure Table Storage or Cosmos DB
    
3. **Secure API using APIM key**
    
4. **Deploy and test**
    

---

# 🛠 STEP 1 — Create Storage or Cosmos DB

(To keep it simple, choose **Storage Tables**):

- Storage Account
    
- Create table: **Products**
    

---

# 🛠 STEP 2 — Create Function App

Same as before:

- Name: `product-api`
    
- Runtime: Node.js
    
- Hosting: Consumption
    

---

# 🧱 STEP 3 — API Endpoints

### **POST /products**

```javascript
const { TableClient } = require("@azure/data-tables");

module.exports = async (context, req) => {
  const client = TableClient.fromConnectionString(process.env.CONN, "Products");
  const entity = {
    partitionKey: "products",
    rowKey: Date.now().toString(),
    name: req.body.name,
    price: req.body.price
  };
  await client.createEntity(entity);
  context.res = { status: 201, body: entity };
};
```

---

### **GET /products**

```javascript
const { TableClient } = require("@azure/data-tables");

module.exports = async (context, req) => {
  const client = TableClient.fromConnectionString(process.env.CONN, "Products");
  let items = [];
  for await (const p of client.listEntities()) items.push(p);
  context.res = { body: items };
};
```

---

### **DELETE /products/{id}**

```javascript
const { TableClient } = require("@azure/data-tables");

module.exports = async (context, req) => {
  const client = TableClient.fromConnectionString(process.env.CONN, "Products");
  const id = context.bindingData.id;
  await client.deleteEntity("products", id);
  context.res = { status: 204 };
};
```

---

# 🛠 STEP 4 — Add API Management (APIM)

1. Create **API Management** instance
    
2. Go to **APIs → Add API → Function App**
    
3. Select your Function App
    
4. Import all 3 functions
    
5. APIM generates clean endpoints:
    

```
https://<apim>.azure-api.net/products
```

6. Add subscription requirement
    
7. Get subscription key (shared key)
    

---

# 🛠 STEP 5 — Test Secure API

Use:

```
Ocp-Apim-Subscription-Key: <your-key>
```

Test using Postman or browser.

---

# 🎉 PROJECT 2 COMPLETE

You now have:

✔ A professional serverless API backend  
✔ Secure API gateway  
✔ Scalable database  
✔ Microservice-ready architecture

---

# 👏 You're Ready for More

If you want, I can also build with you:

- A full authentication system (Azure AD + Web App + Function)
    
- A CI/CD pipeline (GitHub → Azure)
    
- A production-grade serverless system
    
- A database-backed SaaS template
    
- A multi-environment (dev/test/prod) setup
    

Just tell me what you'd like to build next!



