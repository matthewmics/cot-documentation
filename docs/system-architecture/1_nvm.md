# NVM

:::warning

We need NVM because we can't manually install the latest version of Node.js on Linux. This is important for the deployment of our React and NestJS applications.

:::

## Why NVM is Important

NVM (Node Version Manager) is essential for managing multiple versions of Node.js on a Linux server. Since we will be deploying NestJS and Vite React, having the ability to switch between different Node.js versions ensures compatibility and ease of updates.

## Installation

To install NVM, run the following command:

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
```

After installation, restart your terminal or run:

```
source ~/.bashrc  # or source ~/.zshrc if using zsh
```

## Installing Node.js with NVM

To install the latest version of Node.js, use:

```
nvm install node
```

To install a specific version, use:

```
nvm install 18
```

## Switching Node.js Versions

You can switch between installed Node.js versions with:

```
nvm use 18
```

To set a default version:

```
nvm alias default 18
```

## Verifying the Installation

To check the installed Node.js version:

```
node -v
```

To check the installed NVM version:

```
nvm --version
```

## Conclusion

NVM is an essential tool for managing Node.js versions on Linux. It ensures that our NestJS and Vite React deployments run smoothly with the required Node.js version.
