
---

## 🔹 **Step 1: Setup MockAPI for Fake Data**
MockAPI provides a free online REST API for testing.

1. Go to **[MockAPI.io](https://mockapi.io/)**
2. Sign up or log in.
3. Click **"Create a new project"** → Give it a name (e.g., "Users API").
4. Click **"Create a new resource"** (like a database table) → Name it **"users"**.
5. Add fields:
   - `name` (String)
   - `email` (String)
   - `avatar` (Image URL)
6. Click **"Save"** and note down the **API Endpoint** (e.g., `https://64a8c53000c00b772f32fbd.mockapi.io/users`).

---

## 🔹 **Step 2: Create a React App**
If you don’t have a React app, create one using:

```sh
npm create vite@latest mockapi-crud
cd mockapi-crud
npm run dev
```

---





## 🔹 **Step 3: Install Dependencies**
Ensure you have **React Router** installed:

```sh
npm install react-router-dom
```

---

## 🔹 **Step 4: Setup API Services (`api.js`)**  
Inside `src/`, create `api.js` to handle API requests.

```js
const API_URL = "https://64a8c53000c00b772f32fbd.mockapi.io/users";

// GET all users
export const getUsers = async () => {
  const response = await fetch(API_URL);
  return response.json();
};

// GET single user
export const getUserById = async (id) => {
  const response = await fetch(`${API_URL}/${id}`);
  return response.json();
};

// POST a new user
export const addUser = async (user) => {
  const response = await fetch(API_URL, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(user),
  });
  return response.json();
};

// PUT (Update) a user
export const updateUser = async (id, user) => {
  const response = await fetch(`${API_URL}/${id}`, {
    method: "PUT",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(user),
  });
  return response.json();
};

// DELETE a user
export const deleteUser = async (id) => {
  await fetch(`${API_URL}/${id}`, { method: "DELETE" });
};
```

---


---

## 🔹 **Step 6: Create User List (`UsersList.js`)**
Shows users with **Add** & **Edit** buttons.

```js
import React, { useEffect, useState } from "react";
import { getUsers, deleteUser } from "./api";
import { Link } from "react-router-dom";

const UsersList = () => {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    fetchUsers();
  }, []);

  const fetchUsers = async () => {
    const data = await getUsers();
    setUsers(data);
  };

  const handleDelete = async (id) => {
    await deleteUser(id);
    fetchUsers(); // Refresh list
  };

  return (
    <div>
      <h2>User List</h2>
      <Link to="/add">
        <button>Add User</button>
      </Link>
      <ul>
        {users.map((user) => (
          <li key={user.id}>
            <img src={user.avatar} alt={user.name} width="50" />
            {user.name} - {user.email}
            <Link to={`/edit/${user.id}`}>
              <button>Edit</button>
            </Link>
            <button onClick={() => handleDelete(user.id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default UsersList;
```

---

## 🔹 **Step 7: Add New User (`AddUser.js`)**  
Handles form submission to add a new user.

```js
import React, { useState } from "react";
import { addUser } from "./api";
import { useNavigate } from "react-router-dom";

const AddUser = () => {
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");
  const [avatar, setAvatar] = useState("");
  const navigate = useNavigate();

  const handleSubmit = async (e) => {
    e.preventDefault();
    const newUser = { name, email, avatar };
    await addUser(newUser);
    navigate("/"); // Redirect to user list
  };

  return (
    <form onSubmit={handleSubmit}>
      <h2>Add User</h2>
      <input value={name} onChange={(e) => setName(e.target.value)} placeholder="Name" required />
      <input value={email} onChange={(e) => setEmail(e.target.value)} placeholder="Email" required />
      <input value={avatar} onChange={(e) => setAvatar(e.target.value)} placeholder="Avatar URL" required />
      <button type="submit">Add User</button>
    </form>
  );
};

export default AddUser;
```

---

## 🔹 **Step 8: Edit User (`EditUser.js`)**  
Fetches the selected user and updates the details.

```js
import React, { useEffect, useState } from "react";
import { getUserById, updateUser } from "./api";
import { useNavigate, useParams } from "react-router-dom";

const EditUser = () => {
  const { id } = useParams();
  const navigate = useNavigate();
  const [user, setUser] = useState({ name: "", email: "", avatar: "" });

  useEffect(() => {
    fetchUser();
  }, []);

  const fetchUser = async () => {
    const data = await getUserById(id);
    setUser(data);
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    await updateUser(id, user);
    navigate("/");
  };

  return (
    <form onSubmit={handleSubmit}>
      <h2>Edit User</h2>
      <input value={user.name} onChange={(e) => setUser({ ...user, name: e.target.value })} required />
      <input value={user.email} onChange={(e) => setUser({ ...user, email: e.target.value })} required />
      <input value={user.avatar} onChange={(e) => setUser({ ...user, avatar: e.target.value })} required />
      <button type="submit">Update User</button>
    </form>
  );
};

export default EditUser;
```

---

## 🔹 **Final Steps**
### ✅ Run Your App:
```sh
npm run dev
```





## 🎯 **Project Folder Structure**
```
mockapi-crud/
│── src/
│   ├── api.js
│   ├── App.js
│   ├── UsersList.js
│   ├── AddUser.js
│   ├── EditUser.js
│   ├── index.css
│   └── main.jsx
└── public/
```




