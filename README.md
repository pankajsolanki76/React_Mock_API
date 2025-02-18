
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
npx create-react-app mockapi-crud
cd mockapi-crud
npm start
```

---

## 🔹 **Step 3: Create API Service**
Inside `src/`, create a file: **`api.js`** to manage API requests.

```js
const API_URL = "https://64a8c53000c00b772f32fbd.mockapi.io/users";

// GET all users
export const getUsers = async () => {
  const response = await fetch(API_URL);
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

## 🔹 **Step 4: Fetch and Display Users**
Create **`UsersList.js`** to display user data.

```js
import React, { useEffect, useState } from "react";
import { getUsers, deleteUser } from "./api";

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
    fetchUsers(); // Refresh list after deletion
  };

  return (
    <div>
      <h2>User List</h2>
      <ul>
        {users.map((user) => (
          <li key={user.id}>
            <img src={user.avatar} alt={user.name} width="50" />
            {user.name} - {user.email}
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

## 🔹 **Step 5: Add New User Form**
Create **`AddUser.js`** to add users.

```js
import React, { useState } from "react";
import { addUser } from "./api";

const AddUser = ({ onUserAdded }) => {
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");
  const [avatar, setAvatar] = useState("");

  const handleSubmit = async (e) => {
    e.preventDefault();
    const newUser = { name, email, avatar };
    await addUser(newUser);
    onUserAdded();
    setName("");
    setEmail("");
    setAvatar("");
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

## 🔹 **Step 6: Edit User**
Create **`EditUser.js`** for updating users.

```js
import React, { useState } from "react";
import { updateUser } from "./api";

const EditUser = ({ user, onUserUpdated }) => {
  const [name, setName] = useState(user.name);
  const [email, setEmail] = useState(user.email);
  const [avatar, setAvatar] = useState(user.avatar);

  const handleSubmit = async (e) => {
    e.preventDefault();
    await updateUser(user.id, { name, email, avatar });
    onUserUpdated();
  };

  return (
    <form onSubmit={handleSubmit}>
      <h2>Edit User</h2>
      <input value={name} onChange={(e) => setName(e.target.value)} required />
      <input value={email} onChange={(e) => setEmail(e.target.value)} required />
      <input value={avatar} onChange={(e) => setAvatar(e.target.value)} required />
      <button type="submit">Update User</button>
    </form>
  );
};

export default EditUser;
```

---

## 🔹 **Step 7: Connect Components in `App.js`**
Finally, modify **`App.js`** to include everything.

```js
import React, { useState } from "react";
import UsersList from "./UsersList";
import AddUser from "./AddUser";

const App = () => {
  const [reload, setReload] = useState(false);

  return (
    <div>
      <h1>CRUD with MockAPI</h1>
      <AddUser onUserAdded={() => setReload(!reload)} />
      <UsersList key={reload} />
    </div>
  );
};

export default App;
```

---

## 🎯 **Final Steps**
1. Start your React app:
   ```sh
   npm start
   ```
2. You can now:
   - **Add** new users.
   - **View** users.
   - **Delete** users.
   - **Edit** users.

---

## 🎉 **Congratulations!**
You’ve successfully built a **React CRUD App** using **MockAPI** with `fetch`. 🚀 Let me know if you need any modifications! 😊
