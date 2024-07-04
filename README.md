# Vite React WalletConnect WAGMI 

Follow these steps to create a Vite React application, integrate WalletConnect using WAGMI, publish it as a GitHub Page, and deploy it

## Step 4 Setup WalletConnect with WAGMI

WalletConnect's Web3Modal SDK (4.x) provides simple-to-use wallet integration into Web3 App.

Create `.env` and `.env.production` (for deploying as GitHub Pages).

```bash
VITE_WALLETCONNECT_PROJECT_ID=your_walletconnect_project_id
```

### Create Web3Modal Component

Refer to WalletConnect official [documentation](https://docs.walletconnect.com/web3modal/react/about?platform=wagmi) for implementation guide.


## Step 5: Creating Components

In this example, we will create a simple counter app that allows users to claim tokens once they have incremented the counter 10 times. Below are the components we will create: `Claim`, `Mint`, `Counter`, and `ConnectButton`.

### Folder Structure

```plaintext
src
├── Components
│   ├── Claim
│   │   └── index.tsx
│   ├── Mint
│   │   └── index.tsx
│   ├── Counter
│   │   └── index.tsx
│   └── ConnectButton
        └── index.tsx
├── App.jsx
├── main.jsx
└── index.css
```

### Homepage.jsx

Puts together all other components (`Counter`, `Claim`, `Mint`, and `ConnectButton`)

```jsx
import React from 'react';
import ConnectButton from './Components/ConnectButton';
import Counter from './Components/Counter';
import Claim from './Components/Claim';
import Mint from './Components/Mint';

type HomePageProps = {
  count: number;
  onCountChange: (newCount: number) => void;
};

const HomePage: React.FC<HomePageProps> = React.memo(({ count, onCountChange }) => {
  return (
    <main className='main-container'>
      <h1>Earn $GT</h1>
      <div className="button-container">
        <ConnectButton />
      </div>
      <Counter count={count} onCountChange={onCountChange} />
      <Claim count={count} />
      <Mint />
    </main>
  );
});

export default HomePage;
```

### App.jsx

Sets up the Web3 context using a custom Web3ModalProvider and includes a CounterProvider to manage the counter state.

```jsx
import { Web3ModalProvider } from './web3modal';
import { useCallback, useState } from 'react';
import HomePage from './Homepage';

const App = () => {
  return (
    <Web3ModalProvider>
      <CounterProvider />
    </Web3ModalProvider>
  );
};

const CounterProvider = () => {
  const [count, setCount] = useState(0);
  const handleCountChange = useCallback((newCount: number) => {
    setCount(newCount);
  }, []);

  return <HomePage count={count} onCountChange={handleCountChange} />;
};

export default App;

```

### Main Entry Point

The entry point of the React application. It renders the App component into the root element of the HTML file.

```jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App.tsx'
import './index.css'


ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
)

```

<br>

## Step 6: Publish to GitHub Pages

1. **Install the `gh-pages` package:**

    ```sh
    npm install gh-pages --save-dev
    ```

2. **Add the following scripts to your `package.json`:**

    ```json
    "scripts": {
      "predeploy": "npm run build",
      "deploy": "gh-pages -d dist"
    }
    ```

3. **Update the `vite.config.js` file to include the base path for GitHub Pages:**

    ```js
    // vite.config.js
    import { defineConfig } from 'vite';
    import react from '@vitejs/plugin-react';

    export default defineConfig({
      base: '/your-repo-name/',
      plugins: [react()],
    });
    ```

4. **Deploy your app to GitHub Pages:**

    ```sh
    npm run deploy
    ```

5. **Configure GitHub Pages settings:**

    - Go to your repository on GitHub.
    - Navigate to **Settings** > **Pages**.
    - Under **Source**, select `gh-pages` branch.

Your app should now be live at `https://your-username.github.io/your-repo-name/`.

## Step 7: Integrate with Telegram Mini App

1. **Set up a Telegram bot:**

    - Create a new bot using BotFather on Telegram.

2. **Configure the Telegram Mini App settings:**

    - Go to the BotFather chat and use the command `/newapp`.
    - Enter your app’s domain. Since you have deployed to GitHub Pages, the domain would be `https://your-username.github.io/your-repo-name/`.

<img src="https://github.com/kang5647/bnb-telegram-demo/assets/76279908/c54c17ba-d82b-4714-807b-10b99039f61e" alt="Example Image" width="450" height="100">

3. **Test the integration:**

    - Open your Telegram bot and start a chat.
    - Use the `/start` command to launch your Mini App.

## Disclaimer

WalletConnect currently does not work very well when used in a Telegram Mini App. There are no problems when accessing the webpage in a mobile browser.