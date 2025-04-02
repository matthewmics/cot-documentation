# Prerequisites

## Prerequisites for Local Server Setup

Before setting up our codebase locally, ensure that you meet the following requirements:

### 1. Clone the Repository

To get started, clone the project from GitLab using HTTPS:

```
git clone https://gitlab.connextglobal.com/connextdev/recruitment-system.git
cd recruitment-system
```

### 2. Install Dependencies

In the root directory, install dependencies, including Husky, which is used for managing Git hooks:

```
npm install
```

Husky helps enforce code quality by running pre-commit scripts before allowing commits.

### 3. Configure Pre-commit Hook

We have a pre-commit hook configured in Husky to automatically format code using Prettier before committing.

#### Pre-commit Script

```
cd api && npm run prettier:fix && cd ..
cd client && npm run prettier:fix && cd ..
```

#### Explanation:

    - `cd api && npm run prettier:fix && cd ..` → This moves into the api folder, runs Prettier to format the code, and returns to the root directory.

    - `cd client && npm run prettier:fix && cd ..` → Similarly, this moves into the client folder, formats the code using Prettier, and returns to the root.

    - This ensures that all files in both the backend (api) and frontend (client) are properly formatted before a commit is made.

### 4. Install NVM (Node Version Manager) on Windows

Since our project requires specific Node.js versions, we use NVM to manage them.

#### Installation Steps for Windows:

1. Download NVM for Windows from the official GitHub repository:
https://github.com/coreybutler/nvm-windows/releases

2. Run the installer and follow the setup instructions.

3. After installation, restart your terminal and verify the installation:

```
nvm version
```

4. Install and use the required Node.js version (e.g., 18):

```
nvm install 18
nvm use 18
```

### 5. Install MySQL Server on Windows

Our application requires MySQL as the database.

#### Installation Steps for MySQL on Windows:

1. Download MySQL from the official website:
https://dev.mysql.com/downloads/installer/

2. Run the MySQL Installer and select MySQL Server.

3. Choose the Developer Default setup.

4. Configure MySQL with a root password (e.g., P@sswurd).

5. Complete the installation and verify by running:

```
mysql -u root -p
```

Now, you are ready to proceed with setting up the project! 🚀

## Conclusion

By following the steps outlined in this guide, you have successfully prepared your local environment for development. You have:

✅ Cloned the repository from GitLab

✅ Installed dependencies, including Husky for enforcing code quality

✅ Set up a pre-commit hook to automatically format code using Prettier

✅ Installed NVM to manage Node.js versions efficiently

✅ Installed MySQL Server to handle the database requirements

With everything in place, you can now run the application, contribute to development, and ensure smooth collaboration with the team. If you encounter any issues, refer to the documentation or reach out for support.

Happy coding! 🚀