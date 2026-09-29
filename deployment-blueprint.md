# Deployment Blueprint: Node.js/Express + MongoDB Atlas on Render

This guide provides step-by-step instructions for deploying a Node.js/Express backend API connected to a managed MongoDB Atlas database using Render.

---

## 1. Provision the Remote Database (MongoDB Atlas)

1. Log in or create an account at [MongoDB Atlas](https://www.mongodb.com/cloud/atlas).
2. Create a new project and build a **Free M0 Shared Cluster**.
3. **Database Security Setup**:
   * **Database User**: Go to **Database Access**, create a user with a strong password, and assign the `Read and write to any database` role.
   * **Network Access**: Go to **Network Access**, click **Add IP Address**, and select `Allow Access from Anywhere` (`0.0.0.0/0`) so Render instances can connect.
4. **Get Connection String**:
   * Go to **Database Clusters** -> click **Connect** -> choose **Drivers**.
   * Copy the connection URI string. It will follow this format:
     `mongodb+srv://<username>:<password>@cluster0.mongodb.net/<db_name>?retryWrites=true&w=majority`
   * Replace `<username>`, `<password>`, and `<db_name>` with your actual credentials.

---

## 2. Set Up Environment Variables Securely

Never commit secret keys or URI strings directly to GitHub.

1. Ensure `.env` is listed inside your local `.gitignore` file.
2. Store environment variables locally in a `.env` file for local testing:
   ```env
   PORT=5000
   DATABASE_URL=mongodb+srv://<username>:<password>@cluster0.mongodb.net/my_database
   JWT_SECRET=your_secret_key_here

---

 ##  3. Prepare Code & Package Scripts

    Build Command: The command the host runs to download and set up all dependencies before launching (e.g., npm install).

    Start Command: The command the host runs to actually execute the production app (e.g., npm start or node server.js). 


---

## 4. Deploy Application to Render via GitHub

1. Push your latest code to your public GitHub repository.

2. Log in to Render and click New + -> Web Service.

3. Connect your GitHub account and select your backend repository.

4. Fill in the service configuration:

    Name: my-backend-api

    Region: Select the region closest to your users.

    Branch: main (or master)

    Runtime: Node

    Build Command: npm install

    Start Command: npm start

    Instance Type: Free

5. Scroll down to Environment Variables and add your secrets:

    Key: DATABASE_URL | Value: <Your MongoDB Atlas URI>

    Key: NODE_ENV | Value: production

6. Click Create Web Service.

---

## Test and Verify the Live Deployment

1. Once the build finish logs show Your service is live 🎉, copy your public Render URL (e.g., https://my-backend-api.onrender.com).

2. Verify Database Read/Write:

    . Use Postman, Bruno, or curl to send a POST request to an endpoint (e.g., POST https://my-backend-api.onrender.com/api/users).

    .  Verify that a 201 Created status code is returned.

    . Send a GET request to retrieve data and ensure records created online are successfully fetched from MongoDB Atlas.